# Spotify Local — Code Guide

A file-by-file walkthrough of how the codebase works.

---

## 1. Project Structure

```
spotify-local/
├── package.json                 # Root scripts (dev, seed, import-audio)
├── docs/
│   ├── DESIGN.md                # High-level architecture
│   └── CODE.md                  # This file
├── backend/
│   ├── package.json
│   ├── audio/                   # Local MP3 files (gitignored)
│   │   └── perfect.mp3
│   ├── data/
│   │   └── spotify.db           # SQLite database (gitignored)
│   └── src/
│       ├── db.js                # Database connection + schema
│       ├── seed.js              # Demo data seeder
│       ├── import-audio.js      # CLI to import local MP3s
│       └── server.js            # Express API + static audio
└── frontend/
    ├── package.json
    ├── vite.config.js           # Dev server + API proxy
    ├── index.html
    └── src/
        ├── main.jsx             # React entry point
        ├── App.jsx              # App shell + view routing
        ├── index.css            # Global styles
        ├── api/
        │   └── client.js        # Fetch wrapper for API
        ├── hooks/
        │   ├── usePlayer.js     # Audio engine logic
        │   └── PlayerContext.jsx
        └── components/
            ├── Sidebar.jsx
            ├── HomeView.jsx
            ├── SearchView.jsx
            ├── LibraryView.jsx
            ├── DetailView.jsx
            ├── CardGrid.jsx
            ├── TrackList.jsx
            ├── PlayerBar.jsx
            └── PlayerBarIcons.jsx
```

---

## 2. Backend Code

### 2.1 `backend/src/db.js` — Database Setup

Opens SQLite and defines the schema.

```javascript
export function openDb() {
  const db = new Database(dbPath);
  db.pragma('journal_mode = WAL');   // Write-Ahead Logging for performance
  db.pragma('foreign_keys = ON');    // Enforce referential integrity
  return db;
}
```

**Tables created by `initSchema()`:**

- `artists` — id, name, image_url
- `albums` — id, title, artist_id (FK), cover_url, year
- `tracks` — id, title, album_id (FK), duration_ms, audio_url, track_number
- `playlists` — id, name, description, cover_url
- `playlist_tracks` — playlist_id + track_id (composite PK), position

The DB file lives at `backend/data/spotify.db` and is created on first access.

---

### 2.2 `backend/src/server.js` — Express API

**Startup:**

1. Ensures `data/` and `audio/` directories exist
2. Opens DB and runs schema init
3. Mounts middleware: CORS, JSON parser, static `/audio`
4. Registers route handlers
5. Listens on port 4000

**Core SQL helper — `trackSelect()`:**

Every track endpoint uses this join to return enriched track objects:

```sql
SELECT
  t.id, t.title, t.duration_ms, t.audio_url, t.track_number,
  a.id AS album_id, a.title AS album_title, a.cover_url AS album_cover,
  ar.id AS artist_id, ar.name AS artist_name
FROM tracks t
JOIN albums a ON a.id = t.album_id
JOIN artists ar ON ar.id = a.artist_id
```

**Route handlers:**

| Route | Handler logic |
|-------|---------------|
| `GET /api/health` | Returns `{ ok: true, tracks: count }` |
| `GET /api/home` | Playlists list + 6 most recent tracks (DESC by id) + all albums |
| `GET /api/playlists` | Playlists with `COUNT(track_id)` via LEFT JOIN |
| `GET /api/playlists/:id` | Single playlist + tracks ordered by `position` |
| `GET /api/albums/:id` | Single album + tracks ordered by `track_number` |
| `GET /api/tracks` | All tracks, or search with `?q=` using SQL `LIKE` |
| `GET /api/tracks/:id` | Single track by id |

**Static audio:**

```javascript
app.use('/audio', express.static(audioDir));
```

Serves files from `backend/audio/`. Example: `GET /audio/perfect.mp3`.

---

### 2.3 `backend/src/seed.js` — Demo Data

Runs inside a **transaction** for atomicity:

1. `DELETE` all rows from join and entity tables (order respects FKs)
2. Insert 4 artists, 4 albums, 7 tracks, 3 playlists
3. Insert playlist-track associations with positions

**Track audio URLs:**

| Tracks | audio_url |
|--------|-----------|
| 1–6 | SoundHelix remote MP3 URLs |
| 7 (Perfect) | `/audio/perfect.mp3` (local) |

**Run with:** `npm run seed` (from root or backend)

---

### 2.4 `backend/src/import-audio.js` — Import CLI

Copies a user's MP3 into the project and updates the database.

```
Usage: npm run import-audio -- <path-to-mp3> [trackId] [filename]
```

**Steps:**

1. Validate source file exists
2. `copyFileSync` → `backend/audio/<destName>` (default: `perfect.mp3`)
3. `UPDATE tracks SET audio_url = '/audio/<destName>' WHERE id = <trackId>` (default id: 7)

Does not modify seed.js — only updates the live DB. Re-running `npm run seed` will reset the URL back to `/audio/perfect.mp3` (still local, but you'll need to re-import if you used a custom filename).

---

## 3. Frontend Code

### 3.1 `frontend/vite.config.js` — Dev Proxy

```javascript
server: {
  port: 5173,
  proxy: {
    '/api': 'http://localhost:4000',
    '/audio': 'http://localhost:4000',
  },
},
```

The browser sees everything as same-origin on `:5173`. API calls go to `/api/...` and audio to `/audio/...` — Vite forwards both to Express.

---

### 3.2 `frontend/src/api/client.js` — API Client

Thin wrapper around `fetch`:

```javascript
const BASE = '/api';

async function request(path) {
  const res = await fetch(`${BASE}${path}`);
  if (!res.ok) throw new Error(...);
  return res.json();
}

export const api = {
  health: () => request('/health'),
  home: () => request('/home'),
  playlists: () => request('/playlists'),
  playlist: (id) => request(`/playlists/${id}`),
  album: (id) => request(`/albums/${id}`),
  search: (q) => request(`/tracks?q=${encodeURIComponent(q)}`),
};
```

All view components import `api` from here — no direct DB access.

---

### 3.3 `frontend/src/App.jsx` — App Shell

**State:**

- `view` — `'home' | 'search' | 'library' | 'detail'`
- `detail` — `{ kind: 'playlist'|'album', id: number } | null`

**Layout:**

```
PlayerProvider
└── .app (CSS grid)
    ├── Sidebar        — always visible
    ├── main           — switches view via renderMain()
    └── PlayerBar      — always visible
```

**Navigation:**

- `Sidebar.onNavigate(v)` — sets view, clears detail
- `openDetail(kind, id)` — sets detail + view = 'detail'
- `DetailView.onBack()` — clears detail, returns to library

No URL routing — refreshing the page always lands on Home.

---

### 3.4 `frontend/src/hooks/usePlayer.js` — Audio Engine

The heart of playback. Creates one `HTMLAudioElement` and manages all player state.

**Refs (stable across renders):**

| Ref | Purpose |
|-----|---------|
| `audioRef` | The HTML5 Audio element |
| `queueRef` | Current track list for next/prev |
| `queueIndexRef` | Current position in queue |
| `loadTrackRef` | Avoid stale closure in `ended` handler |

**State:**

| State | Purpose |
|-------|---------|
| `currentTrack` | Now-playing track object from API |
| `queue` / `queueIndex` | React state mirrors refs for UI |
| `isPlaying` | Synced via audio `play`/`pause` events |
| `progress` / `duration` | From `timeupdate` / `loadedmetadata` |
| `volume` | 0–1, applied to `audio.volume` |

**Key functions:**

```javascript
loadTrack(track, newQueue, index)
  → Sets queue, currentTrack, audio.src = track.audio_url, calls audio.play()

playTrack(track, allTracks)
  → Finds track index in list, calls loadTrack (list becomes queue)

togglePlay()
  → audio.play() or audio.pause()

playNext() / playPrev()
  → Navigate queue; prev restarts track if < 3 seconds in

seek(pct)
  → Sets audio.currentTime from percentage of duration
```

**Auto-advance on `ended`:**

```javascript
audio.addEventListener('ended', () => {
  const next = (queueIndexRef.current + 1) % q.length;
  loadTrackRef.current?.(q[next], q, next);
});
```

---

### 3.5 `frontend/src/hooks/PlayerContext.jsx` — Context Provider

```javascript
const PlayerContext = createContext(null);

export function PlayerProvider({ children }) {
  const player = usePlayer();
  return <PlayerContext.Provider value={player}>{children}</PlayerContext.Provider>;
}

export function usePlayerContext() {
  const ctx = useContext(PlayerContext);
  if (!ctx) throw new Error('usePlayerContext must be used within PlayerProvider');
  return ctx;
}
```

Any component can call `usePlayerContext()` to access `playTrack`, `currentTrack`, `togglePlay`, etc.

---

### 3.6 View Components

#### `HomeView.jsx`

- `useEffect` → `api.home()` on mount
- Renders: hero, `CardGrid` (playlists), `CardGrid` (albums), `TrackList` (recent)

#### `SearchView.jsx`

- Local `query` state + 300ms debounce
- Calls `api.search(q)` when query is non-empty
- Renders results in `TrackList`

#### `LibraryView.jsx`

- `api.playlists()` on mount
- Renders playlist grid via `CardGrid`

#### `DetailView.jsx`

- Props: `kind` ('playlist' | 'album'), `id`, `onBack`
- Fetches `api.playlist(id)` or `api.album(id)`
- Renders hero (cover, title, metadata) + `TrackList`

---

### 3.7 Shared Components

#### `CardGrid.jsx`

Renders a grid of cards (playlists or albums). Each card is a button that calls `onOpen(type, item.id)`.

#### `TrackList.jsx`

Renders clickable track rows. On click:

```javascript
onClick={() => playTrack(track, tracks)}
```

Highlights the currently playing track. Passes the full `tracks` array so the entire list becomes the queue.

#### `Sidebar.jsx`

Three nav buttons: Home, Search, Library. Highlights active view.

#### `PlayerBar.jsx`

Three-column footer:

| Column | Content |
|--------|---------|
| Left | Cover art + track title/artist |
| Center | Prev / Play-Pause / Next + seek slider |
| Right | Lyrics, Queue, Device, Volume, Mini player, Fullscreen icons |

Volume slider calls `setVolume`. Other right-side icons are visual only (no handlers yet).

#### `PlayerBarIcons.jsx`

SVG icon components: `IconLyrics`, `IconQueue`, `IconDevice`, `IconVolume`, `IconMiniPlayer`, `IconFullscreen`.

`IconVolume` accepts a `level` prop to show muted / low / high speaker states.

---

### 3.8 `frontend/src/index.css` — Layout

**CSS Grid on `.app`:**

```css
grid-template-columns: 240px 1fr;      /* sidebar | main */
grid-template-rows: 1fr 90px;          /* content | player bar */
height: 100vh;
```

Sidebar spans both rows. Player bar spans the main column bottom.

**Theme variables:**

```css
--bg-base: #000000;
--accent: #1db954;        /* Spotify green */
--sidebar-width: 240px;
--player-height: 90px;
```

Responsive breakpoint at 900px collapses sidebar labels.

---

## 4. Data Flow Examples

### Playing a track from search

```
SearchView
  → user types "Perfect"
  → api.search("Perfect")
  → GET /api/tracks?q=Perfect
  → server.js: trackSelect() + WHERE LIKE
  → returns [{ id: 7, title: "Perfect", audio_url: "/audio/perfect.mp3", ... }]
  → TrackList renders results
  → user clicks row
  → playTrack(track, tracks)
  → usePlayer: audio.src = "/audio/perfect.mp3"
  → browser: GET /audio/perfect.mp3 (via Vite proxy)
  → Express static serves backend/audio/perfect.mp3
  → PlayerBar updates with now-playing info
```

### Opening a playlist

```
LibraryView
  → CardGrid shows playlists from api.playlists()
  → user clicks "Today's Hits"
  → App.openDetail('playlist', 1)
  → DetailView mounts
  → api.playlist(1)
  → GET /api/playlists/1
  → returns { name, tracks: [...] } ordered by position
  → TrackList shows all tracks; click to play with full playlist as queue
```

---

## 5. npm Scripts Reference

### Root `package.json`

| Script | What it runs |
|--------|--------------|
| `install:all` | `npm install` in backend + frontend |
| `setup` | `install:all` + `seed` |
| `dev` | `concurrently` backend + frontend dev servers |
| `dev:backend` | Backend only |
| `dev:frontend` | Frontend only |
| `seed` | Re-seed database |
| `import-audio` | Pass-through to backend import script |

### Backend `package.json`

| Script | Command |
|--------|---------|
| `dev` | `node --watch src/server.js` |
| `start` | `node src/server.js` |
| `seed` | `node src/seed.js` |
| `import-audio` | `node src/import-audio.js` |

### Frontend `package.json`

| Script | Command |
|--------|---------|
| `děv` | `vite` |
| `build` | `vite build` |
| `preview` | `vite preview` |

---

## 6. Gitignored Files

| Path | Why |
|------|-----|
| `backend/data/` | SQLite DB is generated locally |
| `backend/audio/*` | User's MP3 files (except `.gitkeep`) |
| `node_modules/` | Dependencies |
| `dist/` | Production build output |

---

## 7. Adding a New Track (Checklist)

1. Add artist/album/track entries in `backend/src/seed.js`
2. Place MP3 in `backend/audio/` or run `npm run import-audio`
3. Add track to a playlist in `playlistTracks` array in seed.js
4. Run `npm run seed`
5. Track appears in Home, Search, and playlist detail automatically — no frontend changes needed if using existing views

---

## 8. Common Debugging

| Problem | Check |
|---------|-------|
| Empty home page | Run `npm run seed`; check `GET /api/health` |
| Track won't play | Verify `audio_url` in API response; check Network tab for 404 on `/audio/...` |
| API unreachable | Ensure backend is running on :4000; check Vite proxy config |
| CORS errors | Should not happen with proxy; if hitting :4000 directly, CORS is enabled |
| Perfect silent / 404 | Run `npm run import-audio -- /path/to/file.mp3`; confirm file in `backend/audio/` |
| DB reset lost import | `npm run seed` resets data but keeps `/audio/perfect.mp3` path — re-import if needed |

---

## 9. File Dependency Graph

```
main.jsx
  └── App.jsx
        ├── PlayerProvider (PlayerContext.jsx → usePlayer.js)
        ├── Sidebar.jsx
        ├── HomeView.jsx ────── api/client.js
        ├── SearchView.jsx ──── api/client.js
        ├── LibraryView.jsx ── api/client.js
        ├── DetailView.jsx ──── api/client.js
        └── PlayerBar.jsx ───── PlayerBarIcons.jsx
              └── usePlayerContext()

server.js
  └── db.js ← seed.js, import-audio.js

TrackList.jsx → usePlayerContext().playTrack()
CardGrid.jsx  → onOpen → App.openDetail()
```

This is the complete code map for the project. For architecture and design rationale, see [DESIGN.md](./DESIGN.md).
