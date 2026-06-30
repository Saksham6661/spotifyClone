̌# Spotify Local — High-Level Design

End-to-end design for a self-contained, Spotify-style music app that runs entirely on your machine.

---

## 1. Purpose

Spotify Local is a **local demo music player** with a familiar Spotify UI. It lets you browse playlists, albums, and tracks, search the catalog, and play audio through a bottom player bar — without connecting to Spotify's real services.

The app is split into two independently runnable processes:

| Process | Role | Port |
|---------|------|------|
| **Frontend (UI)** | React SPA — browse, search, play | 5173 |
| **Backend (API)** | Express REST API + SQLite + audio files | 4000 |

Both are started together with `npm run dev` from the project root.

---

## 2. System Context

```
┌─────────────────────────────────────────────────────────────────┐
│                        User's Browser                           │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              React UI (localhost:5173)                    │  │
│  │  Sidebar │ Home / Search / Library / Detail │ Player Bar  │  │
│  └───────────────────────────┬───────────────────────────────┘  │
│                              │                                  │
│         fetch /api/*         │         HTML5 Audio              │
│         fetch /audio/*       │         loads audio_url          │
└──────────────────────────────┼──────────────────────────────────┘
                               │
                    Vite dev proxy (5173 → 4000)
                               │
┌──────────────────────────────▼──────────────────────────────────┐
│                   Express API (localhost:4000)                  │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────────────┐  │
│  │  REST API   │  │   SQLite DB  │  │  Static audio files   │  │
│  │  /api/*     │  │  spotify.db  │  │  /audio/*.mp3         │  │
│  └─────────────┘  └──────────────┘  └───────────────────────┘  │
└──────────────────────────────┬──────────────────────────────────┘
                               │
              Remote MP3 URLs (SoundHelix demo tracks)
                               │
                               ▼
                        Internet (optional)
```

**Key idea:** The UI never talks to SQLite directly. All catalog data flows through the REST API. Audio is streamed by the browser from either local files (served by Express) or remote URLs stored in the database.

---

## 3. Architecture Layers

### 3.1 Presentation Layer (Frontend)

- **Framework:** React 19 + Vite 6
- **Styling:** Plain CSS (Spotify-inspired dark theme)
- **Routing:** Client-side view state in `App.jsx` (no React Router)
- **State:** React `useState` for navigation; React Context for the global audio player

**Views:**

| View | Purpose |
|------|---------|
| Home | Playlists, albums, recently added tracks |
| Search | Live search across tracks, artists, albums |
| Library | Browse all playlists |
| Detail | Single playlist or album with full track list |

**Persistent UI:** Sidebar (navigation) and PlayerBar (playback controls) are always visible.

### 3.2 Application Layer (Backend)

- **Framework:** Express 4
- **Database:** SQLite via `better-sqlite3` (synchronous, embedded, file-based)
- **Static files:** Local MP3s served at `/audio/*`

The backend is a thin **read-only catalog API** plus static file hosting. There is no user auth, no write endpoints, and no upload API — data is seeded or updated via CLI scripts.

### 3.3 Data Layer

- **Database file:** `backend/data/spotify.db`
- **Seed script:** Wipes and repopulates demo data
- **Import script:** Copies a local MP3 and updates a track's `audio_url`

### 3.4 Audio Layer

Two audio sources coexist:

| Source | Used for | Example |
|--------|----------|---------|
| **Remote URL** | Demo tracks 1–6 | `https://www.soundhelix.com/.../SoundHelix-Song-1.mp3` |
| **Local file** | User-imported tracks | `/audio/perfect.mp3` → `backend/audio/perfect.mp3` |

The browser's native `HTML5 Audio` element handles streaming, seeking, and volume. No separate audio server.

---

## 4. Domain Model

```
Artist ──< Album ──< Track >──< PlaylistTrack >── Playlist
```

| Entity | Description |
|--------|-------------|
| **Artist** | Music artist (name, image) |
| **Album** | Album belonging to one artist (title, cover, year) |
| **Track** | Song on an album (title, duration, audio URL, track number) |
| **Playlist** | Curated collection (name, description, cover) |
| **PlaylistTrack** | Join table linking tracks to playlists with ordering |

Every track returned by the API is **enriched** with album and artist fields (cover art, artist name) via SQL joins — the frontend receives a flat, ready-to-display object.

---

## 5. End-to-End User Flows

### 5.1 Browse and Play

```
User opens Home
    → Frontend: GET /api/home
    → Backend: Query playlists, recent tracks, albums from SQLite
    → Frontend: Render cards and track list
User clicks a track
    → Frontend: playTrack(track, tracks) — sets queue + audio.src
    → Browser: Streams from audio_url (local or remote)
    → PlayerBar: Shows now-playing, progress, controls
```

### 5.2 Search

```
User types in Search view
    → Frontend: Debounce 300ms, then GET /api/tracks?q=...
    → Backend: SQL LIKE on title, artist name, album title
    → Frontend: Render results in TrackList
User clicks result → same play flow as above
```

### 5.3 Playlist / Album Detail

```
User clicks playlist or album card
    → App sets view = 'detail', stores { kind, id }
    → Frontend: GET /api/playlists/:id or /api/albums/:id
    → Backend: Returns entity + ordered tracks[]
    → Frontend: DetailView with hero + TrackList
```

### 5.4 Queue and Auto-Advance

```
User plays track from a list
    → Queue = entire list; index = clicked track position
Track ends (ended event)
    → Player advances to next track in queue (wraps around)
User clicks Next / Prev
    → Same queue navigation; Prev within 3s restarts current track
```

### 5.5 Local Audio Import

```
User runs: npm run import-audio -- /path/to/song.mp3
    → Script copies file to backend/audio/
    → Script UPDATE tracks SET audio_url = '/audio/...' WHERE id = 7
User plays track
    → Browser requests /audio/perfect.mp3
    → Vite proxy → Express static → file on disk
```

---

## 6. API Surface (Summary)

| Endpoint | Returns |
|----------|---------|
| `GET /api/health` | Server status + track count |
| `GET /api/home` | Playlists, recent tracks, albums |
| `GET /api/playlists` | All playlists with track counts |
| `GET /api/playlists/:id` | Playlist + tracks |
| `GET /api/albums/:id` | Album + tracks |
| `GET /api/tracks?q=` | All tracks or search results |
| `GET /api/tracks/:id` | Single track |
| `GET /audio/*` | Static MP3 files |

All API responses are JSON. CORS is enabled for local development.

---

## 7. Deployment Topology (Local Dev)

```
spotify-local/
├── frontend/          ← Vite dev server (:5173)
│   └── proxies /api, /audio → backend
├── backend/           ← Express (:4000)
│   ├── data/          ← SQLite (gitignored)
│   └── audio/         ← Local MP3s (gitignored)
└── package.json       ← concurrently runs both
```

**Startup sequence:**

1. `npm install` — root + backend + frontend dependencies
2. `npm run seed` — create/populate SQLite
3. `npm run dev` — start API + UI concurrently
4. Open `http://localhost:5173`

---

## 8. Design Decisions

| Decision | Rationale |
|----------|-----------|
| SQLite over Postgres | Zero setup; single file; perfect for local demo |
| No React Router | Simple view switching; fewer dependencies |
| Context for player | Single global audio instance; avoids prop drilling |
| Vite proxy | Frontend and API appear same-origin; no CORS issues for audio |
| Seed wipes data | Predictable demo state; easy reset |
| Remote + local audio | Demo works out of the box; user can add real songs |
| Read-only API | Scope kept minimal; mutations via CLI only |

---

## 9. Non-Goals (Current Scope)

- User accounts / authentication
- Playlist editing from the UI
- Spotify OAuth or real Spotify catalog
- Mobile app or PWA
- Lyrics, queue panel, device connect (icons are decorative)
- Production deployment / Docker

---

## 10. Extension Points

If you want to grow the app later, natural extension points are:

1. **Write API** — CRUD for playlists and favorites
2. **React Router** — Deep links to albums/playlists
3. **More local audio** — Extend `import-audio.js` to register new tracks in seed
4. **Queue UI** — Wire PlayerBar queue icon to `usePlayer` queue state
5. **Album art** — Store local cover images alongside audio in `backend/audio/`

---

## 11. Quick Reference

```bash
npm run setup      # install deps + seed DB
npm run dev        # start frontend + backend
npm run seed       # reset database
npm run import-audio -- /path/to/file.mp3   # add local song
```

| URL | What |
|-----|------|
| http://localhost:5173 | App UI |
| http://localhost:4000/api/health | API health check |
| http://localhost:4000/audio/perfect.mp3 | Local audio file |
