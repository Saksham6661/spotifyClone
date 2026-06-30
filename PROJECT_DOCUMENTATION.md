# Spotify Local
## Complete Project Documentation

**Version:** 1.0.0  
**Document Type:** End-to-End Project Description  
**Estimated Length:** ~50 pages  
**Last Updated:** June 2026  

---

# Table of Contents

1. [Document Overview](#1-document-overview)
2. [Executive Summary](#2-executive-summary)
3. [Introduction](#3-introduction)
   - 3.1 [Background](#31-background)
   - 3.2 [Problem Statement](#32-problem-statement)
   - 3.3 [Project Vision](#33-project-vision)
   - 3.4 [Intended Audience](#34-intended-audience)
4. [Project Objectives and Scope](#4-project-objectives-and-scope)
   - 4.1 [Primary Objectives](#41-primary-objectives)
   - 4.2 [Functional Scope](#42-functional-scope)
   - 4.3 [Non-Functional Requirements](#43-non-functional-requirements)
   - 4.4 [Out of Scope](#44-out-of-scope)
5. [Technology Stack](#5-technology-stack)
   - 5.1 [Frontend Technologies](#51-frontend-technologies)
   - 5.2 [Backend Technologies](#52-backend-technologies)
   - 5.3 [Data and Storage](#53-data-and-storage)
   - 5.4 [Development Tools](#54-development-tools)
   - 5.5 [Technology Selection Rationale](#55-technology-selection-rationale)
6. [System Architecture](#6-system-architecture)
   - 6.1 [High-Level Architecture](#61-high-level-architecture)
   - 6.2 [Layered Architecture Model](#62-layered-architecture-model)
   - 6.3 [Process Topology](#63-process-topology)
   - 6.4 [Communication Patterns](#64-communication-patterns)
   - 6.5 [Deployment Topology (Local)](#65-deployment-topology-local)
7. [Domain Model and Data Design](#7-domain-model-and-data-design)
   - 7.1 [Entity Relationship Overview](#71-entity-relationship-overview)
   - 7.2 [Database Schema](#72-database-schema)
   - 7.3 [Table Descriptions](#73-table-descriptions)
   - 7.4 [Relationships and Constraints](#74-relationships-and-constraints)
   - 7.5 [Seed Data Catalog](#75-seed-data-catalog)
   - 7.6 [API Data Shapes](#76-api-data-shapes)
8. [Backend System Design](#8-backend-system-design)
   - 8.1 [Backend Overview](#81-backend-overview)
   - 8.2 [Server Initialization](#82-server-initialization)
   - 8.3 [Middleware Stack](#83-middleware-stack)
   - 8.4 [REST API Endpoints](#84-rest-api-endpoints)
   - 8.5 [SQL Query Strategy](#85-sql-query-strategy)
   - 8.6 [Static Audio Serving](#86-static-audio-serving)
   - 8.7 [Database Module (`db.js`)](#87-database-module-dbjs)
   - 8.8 [Seed Script (`seed.js`)](#88-seed-script-seedjs)
   - 8.9 [Import Audio Script (`import-audio.js`)](#89-import-audio-script-import-audiojs)
9. [Frontend System Design](#9-frontend-system-design)
   - 9.1 [Frontend Overview](#91-frontend-overview)
   - 9.2 [Application Entry and Bootstrap](#92-application-entry-and-bootstrap)
   - 9.3 [Application Shell (`App.jsx`)](#93-application-shell-appjsx)
   - 9.4 [Navigation Model](#94-navigation-model)
   - 9.5 [API Client Layer](#95-api-client-layer)
   - 9.6 [View Components](#96-view-components)
   - 9.7 [Shared UI Components](#97-shared-ui-components)
   - 9.8 [Styling and Layout System](#98-styling-and-layout-system)
   - 9.9 [Vite Configuration and Proxy](#99-vite-configuration-and-proxy)
10. [Audio Playback System](#10-audio-playback-system)
    - 10.1 [Audio Architecture Overview](#101-audio-architecture-overview)
    - 10.2 [Remote vs Local Audio Sources](#102-remote-vs-local-audio-sources)
    - 10.3 [Player Engine (`usePlayer.js`)](#103-player-engine-useplayerjs)
    - 10.4 [Queue Management](#104-queue-management)
    - 10.5 [Player Context and Global State](#105-player-context-and-global-state)
    - 10.6 [Player Bar UI](#106-player-bar-ui)
    - 10.7 [Local Audio Import Workflow](#107-local-audio-import-workflow)
11. [User Interface Design](#11-user-interface-design)
    - 11.1 [Design Philosophy](#111-design-philosophy)
    - 11.2 [Layout Structure](#112-layout-structure)
    - 11.3 [Color Palette and Theme](#113-color-palette-and-theme)
    - 11.4 [Screen Inventory](#114-screen-inventory)
    - 11.5 [Player Bar Controls](#115-player-bar-controls)
    - 11.6 [Responsive Behavior](#116-responsive-behavior)
12. [End-to-End User Flows](#12-end-to-end-user-flows)
    - 12.1 [Application Startup Flow](#121-application-startup-flow)
    - 12.2 [Home Browsing Flow](#122-home-browsing-flow)
    - 12.3 [Search and Discovery Flow](#123-search-and-discovery-flow)
    - 12.4 [Library and Playlist Flow](#124-library-and-playlist-flow)
    - 12.5 [Album Detail Flow](#125-album-detail-flow)
    - 12.6 [Track Playback Flow](#126-track-playback-flow)
    - 12.7 [Queue and Auto-Advance Flow](#127-queue-and-auto-advance-flow)
    - 12.8 [Local Song Import Flow](#128-local-song-import-flow)
13. [Installation and Setup Guide](#13-installation-and-setup-guide)
    - 13.1 [Prerequisites](#131-prerequisites)
    - 13.2 [First-Time Setup](#132-first-time-setup)
    - 13.3 [Running the Application](#133-running-the-application)
    - 13.4 [Running Services Separately](#134-running-services-separately)
    - 13.5 [Importing Local Audio Files](#135-importing-local-audio-files)
    - 13.6 [Resetting the Database](#136-resetting-the-database)
14. [Development Workflow](#14-development-workflow)
    - 14.1 [Repository Structure](#141-repository-structure)
    - 14.2 [npm Scripts Reference](#142-npm-scripts-reference)
    - 14.3 [Adding New Tracks](#143-adding-new-tracks)
    - 14.4 [Modifying the API](#144-modifying-the-api)
    - 14.5 [Modifying the UI](#145-modifying-the-ui)
    - 14.6 [Git and Ignored Files](#146-git-and-ignored-files)
15. [API Reference](#15-api-reference)
    - 15.1 [Health Check](#151-health-check)
    - 15.2 [Home Endpoint](#152-home-endpoint)
    - 15.3 [Playlists Endpoints](#153-playlists-endpoints)
    - 15.4 [Albums Endpoint](#154-albums-endpoint)
    - 15.5 [Tracks Endpoints](#155-tracks-endpoints)
    - 15.6 [Static Audio Endpoint](#156-static-audio-endpoint)
    - 15.7 [Error Responses](#157-error-responses)
16. [Component Reference](#16-component-reference)
    - 16.1 [Backend Files](#161-backend-files)
    - 16.2 [Frontend Files](#162-frontend-files)
    - 16.3 [File Dependency Graph](#163-file-dependency-graph)
17. [Testing and Validation](#17-testing-and-validation)
    - 17.1 [Manual Test Plan](#171-manual-test-plan)
    - 17.2 [API Verification](#172-api-verification)
    - 17.3 [Playback Verification](#173-playback-verification)
    - 17.4 [Common Issues and Debugging](#174-common-issues-and-debugging)
18. [Security and Legal Considerations](#18-security-and-legal-considerations)
    - 18.1 [Security Posture](#181-security-posture)
    - 18.2 [Copyright and Audio Content](#182-copyright-and-audio-content)
    - 18.3 [Local-Only Design Benefits](#183-local-only-design-benefits)
19. [Future Enhancements](#19-future-enhancements)
    - 19.1 [Short-Term Improvements](#191-short-term-improvements)
    - 19.2 [Medium-Term Features](#192-medium-term-features)
    - 19.3 [Long-Term Vision](#193-long-term-vision)
20. [Detailed Implementation Walkthrough](#20-detailed-implementation-walkthrough)
    - 20.1 [Backend Route Handler Deep Dive](#201-backend-route-handler-deep-dive)
    - 20.2 [Frontend Component Deep Dive](#202-frontend-component-deep-dive)
    - 20.3 [Player Hook Line-by-Line Behavior](#203-player-hook-line-by-line-behavior)
    - 20.4 [CSS Architecture Breakdown](#204-css-architecture-breakdown)
    - 20.5 [Sequence Diagrams](#205-sequence-diagrams)
    - 20.6 [Use Case Catalog](#206-use-case-catalog)
    - 20.7 [Production Build Guide](#207-production-build-guide)
    - 20.8 [Performance and Scalability Notes](#208-performance-and-scalability-notes)
21. [Appendices](#21-appendices)
    - Appendix A: [Sample API Responses](#appendix-a-sample-api-responses)
    - Appendix B: [Sample Seed Data](#appendix-b-sample-seed-data)
    - Appendix C: [Glossary](#appendix-c-glossary)
    - Appendix D: [Complete File Listing](#appendix-d-complete-file-listing)
    - Appendix E: [Environment and Ports Reference](#appendix-e-environment-and-ports-reference)
    - Appendix F: [Frequently Asked Questions](#appendix-f-frequently-asked-questions)
    - Appendix G: [Comparison with Spotify](#appendix-g-comparison-with-spotify)
22. [Conclusion](#22-conclusion)

---

# 1. Document Overview

This document provides a complete, end-to-end description of **Spotify Local** — a self-contained music streaming application that mimics the Spotify user experience while running entirely on a developer's local machine. It is intended to serve as:

- A **project description** for stakeholders, reviewers, or academic submission
- An **architectural reference** for developers joining the project
- An **operational guide** for setup, usage, and extension
- A **technical narrative** explaining how every major part of the system works together

The document covers business context, system design, implementation details, user flows, API specifications, and future direction. Companion documents `DESIGN.md` and `CODE.md` provide condensed architecture and code walkthroughs; this file is the authoritative, comprehensive project record.

---

# 2. Executive Summary

**Spotify Local** is a full-stack web application that delivers a Spotify-inspired music browsing and playback experience without relying on Spotify's commercial API or cloud infrastructure. The project demonstrates modern web development practices using **React 19**, **Vite**, **Express**, and **SQLite**, combined into a cohesive local-first music player.

### What the Project Delivers

| Capability | Status |
|------------|--------|
| Browse home page with playlists, albums, and recent tracks | ✅ Complete |
| Search tracks, artists, and albums | ✅ Complete |
| View playlist and album details | ✅ Complete |
| Play, pause, seek, volume control | ✅ Complete |
| Queue management with auto-advance | ✅ Complete |
| Remote demo audio (SoundHelix) | ✅ Complete |
| Local MP3 import (e.g., Ed Sheeran — Perfect) | ✅ Complete |
| Spotify-style player bar with extended icons | ✅ Complete |
| User authentication | ❌ Not in scope |
| Playlist editing from UI | ❌ Not in scope |

### Architecture at a Glance

The system consists of two cooperating processes:

1. **Frontend (port 5173)** — React single-page application for all user interaction
2. **Backend (port 4000)** — Express REST API, SQLite database, and static audio file server

The frontend communicates with the backend exclusively through HTTP. Audio playback is handled by the browser's native HTML5 Audio API, streaming from either remote URLs or locally stored MP3 files.

### Key Achievement

The project successfully implements a **complete end-to-end music application loop**: data is stored in SQLite, exposed via REST API, rendered in a polished React UI, and played back through an integrated player — including support for user-imported local audio files such as *Perfect* by Ed Sheeran.

---

# 3. Introduction

## 3.1 Background

Music streaming applications have become the dominant way people consume music. Spotify, Apple Music, and similar platforms offer vast catalogs, personalized recommendations, and seamless playback across devices. However, building even a simplified version of such a system requires understanding multiple disciplines: frontend UI development, backend API design, database modeling, audio streaming, and state management.

Spotify Local was created as a **learning and demonstration project** that captures the essential user experience of a streaming app while remaining simple enough to run on a single machine with minimal setup. It is not affiliated with Spotify AB and does not use any Spotify proprietary services.

## 3.2 Problem Statement

Developers and students often need a practical project that:

- Mirrors real-world streaming app UX patterns
- Uses a modern JavaScript full-stack toolchain
- Runs locally without cloud dependencies or paid API keys
- Demonstrates end-to-end data flow from database to audio playback
- Can be extended incrementally with new features

Existing tutorials often cover only isolated pieces — a React UI without a real backend, or an API without playback. Spotify Local addresses this gap by providing a **working, integrated system** where every layer connects to the next.

## 3.3 Project Vision

The vision for Spotify Local is to create a **credible local music hub** that:

1. Looks and feels like a professional streaming application
2. Stores and serves its own catalog data
3. Plays audio reliably from multiple sources
4. Allows users to bring their own music files
5. Serves as a foundation for future feature expansion

The project prioritizes clarity and maintainability over feature completeness, making it an ideal codebase for study, demonstration, and iteration.

## 3.4 Intended Audience

This documentation is written for:

- **Developers** implementing or extending the application
- **Reviewers** evaluating the project architecture and completeness
- **Students** learning full-stack web development
- **Stakeholders** seeking a high-level understanding of capabilities and design

## 3.5 Document Structure Guide

This document is organized into 22 major sections plus 5 appendices, designed to be read either sequentially (for a complete understanding) or by section (for targeted reference):

| Sections | Focus |
|----------|-------|
| 1–3 | Context, motivation, and audience |
| 4–5 | Requirements and technology choices |
| 6–7 | Architecture and data model |
| 8–10 | Backend, frontend, and audio implementation |
| 11–12 | UI design and user flows |
| 13–14 | Setup and development workflow |
| 15–17 | API reference, components, and testing |
| 18–19 | Security, legal, and future work |
| 20 | Deep implementation walkthrough |
| 21 | Appendices with samples and reference data |
| 22 | Conclusion |

When printed at standard formatting (12pt font, 1-inch margins, single spacing), this document is approximately **50 pages** in length.

## 3.6 Project Evolution Summary

Spotify Local evolved through several development phases during its creation:

**Phase 1 — Foundation:** Initial project scaffolding with React frontend, Express backend, SQLite database, and SoundHelix demo audio. Basic home page and track playback.

**Phase 2 — Feature Completion:** Search, library, playlist/album detail views, queue management, and auto-advance. Full player bar with seek and volume controls.

**Phase 3 — UI Polish:** Spotify-style player bar icons (lyrics, queue, device, volume, mini-player, fullscreen). Dark theme refinement and responsive layout.

**Phase 4 — Local Audio:** Backend static file serving, Vite audio proxy, import-audio CLI, and Ed Sheeran — *Perfect* as a locally hosted track demonstrating real music playback.

**Phase 5 — Documentation:** Comprehensive documentation suite including design guide, code guide, and this full project document.

---

# 4. Project Objectives and Scope

## 4.1 Primary Objectives

| # | Objective | Description |
|---|-----------|-------------|
| O1 | Spotify-like UI | Dark-themed interface with sidebar, content area, and persistent player bar |
| O2 | Catalog management | Store artists, albums, tracks, and playlists in SQLite |
| O3 | REST API | Expose catalog data through documented HTTP endpoints |
| O4 | Audio playback | Play tracks in-browser with standard transport controls |
| O5 | Search | Find music by track title, artist name, or album title |
| O6 | Local audio support | Import and play user-owned MP3 files from the project directory |
| O7 | Developer experience | Single-command setup and dual-server development workflow |

## 4.2 Functional Scope

### Included Features

**Navigation**
- Home, Search, and Library views via sidebar
- Playlist and album detail pages
- Back navigation from detail views

**Content Display**
- Playlist cards with cover art and descriptions
- Album cards with artist name and year
- Track lists with title, artist, album, duration, and playing indicator

**Playback**
- Play/pause toggle
- Previous and next track
- Seek bar with elapsed and total time
- Volume slider
- Automatic queue advance when a track ends
- Queue built from the list the user clicked from

**Data**
- Pre-seeded demo catalog (4 artists, 4 albums, 7 tracks, 3 playlists)
- Ed Sheeran — *Perfect* with local MP3 support
- Remote SoundHelix demo tracks for other songs

**Tooling**
- Database seed script
- Local audio import CLI
- Concurrent dev server startup

## 4.3 Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| Setup time | Under 5 minutes on a machine with Node.js installed |
| Startup time | Both servers running within ~3 seconds |
| API response time | Sub-50ms for local SQLite queries |
| Browser support | Modern evergreen browsers (Chrome, Firefox, Safari, Edge) |
| Dependencies | No Docker, no external DB server required |
| Portability | macOS, Linux, Windows (with Node.js) |

## 4.4 Out of Scope

The following are explicitly **not** part of the current project:

- User registration, login, or sessions
- Spotify OAuth or real Spotify catalog integration
- Creating or editing playlists from the UI
- Social features (sharing, following)
- Recommendations or machine learning
- Lyrics display, device casting, mini-player (icons present but non-functional)
- Mobile native apps or PWA offline mode
- Production deployment, CI/CD, or Docker containers
- Automated test suite

---

# 5. Technology Stack

## 5.1 Frontend Technologies

| Technology | Version | Role |
|------------|---------|------|
| React | 19.x | UI component library |
| Vite | 6.x | Dev server, bundler, HMR |
| JavaScript (ES modules) | — | Application language |
| CSS (plain) | — | Styling, no CSS framework |
| HTML5 Audio API | — | Audio playback engine |

## 5.2 Backend Technologies

| Technology | Version | Role |
|------------|---------|------|
| Node.js | 18+ | Runtime environment |
| Express | 4.x | HTTP server and routing |
| better-sqlite3 | 11.x | Synchronous SQLite driver |
| cors | 2.x | Cross-origin resource sharing |

## 5.3 Data and Storage

| Component | Location | Purpose |
|-----------|----------|---------|
| SQLite database | `backend/data/spotify.db` | Catalog storage |
| Local audio files | `backend/audio/*.mp3` | User-imported songs |
| Remote audio URLs | Stored in DB | SoundHelix demo streams |
| Cover art URLs | Stored in DB | picsum.photos placeholders |

## 5.4 Development Tools

| Tool | Purpose |
|------|---------|
| concurrently | Run frontend and backend simultaneously |
| node --watch | Auto-restart backend on file changes |
| Vite HMR | Hot module replacement for frontend |
| npm workspaces (manual) | Separate package.json per package |

## 5.5 Technology Selection Rationale

**React** — Industry-standard UI library with excellent component model and ecosystem. React 19 provides modern hooks and concurrent features.

**Vite** — Fast dev server with built-in proxy support, essential for forwarding `/api` and `/audio` to the backend during development.

**Express** — Minimal, well-understood HTTP framework. Ideal for a read-only REST API without unnecessary complexity.

**SQLite + better-sqlite3** — Zero-configuration embedded database. A single file (`spotify.db`) contains the entire catalog. Synchronous API simplifies seed scripts and route handlers.

**HTML5 Audio** — Native browser capability eliminates need for Web Audio API complexity or third-party players for basic streaming use cases.

**No React Router** — View state managed in `App.jsx` reduces dependencies. Acceptable for a demo app with four views.

---

# 6. System Architecture

## 6.1 High-Level Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                         USER BROWSER                                 │
│                                                                      │
│   ┌────────────────────────────────────────────────────────────┐    │
│   │                  REACT APPLICATION (:5173)                  │    │
│   │                                                             │    │
│   │  ┌──────────┐  ┌─────────────────────────┐  ┌───────────┐  │    │
│   │  │ Sidebar  │  │   Main Content Area     │  │           │  │    │
│   │  │          │  │  Home | Search |       │  │           │  │    │
│   │  │ Home     │  │  Library | Detail      │  │           │  │    │
│   │  │ Search   │  │                         │  │           │  │    │
│   │  │ Library  │  └─────────────────────────┘  │           │  │    │
│   │  └──────────┘                                 │           │  │    │
│   │  ┌──────────────────────────────────────────────────────┐  │    │
│   │  │              PlayerBar (always visible)               │  │    │
│   │  └──────────────────────────────────────────────────────┘  │    │
│   │                                                             │    │
│   │  ┌─────────────────┐         ┌─────────────────────────┐   │    │
│   │  │  api/client.js  │         │  usePlayer (Audio)    │   │    │
│   │  │  fetch /api/*   │         │  src = audio_url      │   │    │
│   │  └────────┬────────┘         └───────────┬─────────────┘   │    │
│   └───────────┼──────────────────────────────┼─────────────────┘    │
│               │                              │                       │
└───────────────┼──────────────────────────────┼───────────────────────┘
                │         Vite Proxy           │
                ▼                              ▼
┌──────────────────────────────────────────────────────────────────────┐
│                    EXPRESS SERVER (:4000)                            │
│                                                                      │
│   ┌─────────────┐    ┌──────────────┐    ┌─────────────────────┐    │
│   │  /api/*     │    │  SQLite DB   │    │  /audio/* static    │    │
│   │  REST API   │───▶│  spotify.db  │    │  backend/audio/     │    │
│   └─────────────┘    └──────────────┘    └─────────────────────┘    │
│                                                                      │
└──────────────────────────────┬───────────────────────────────────────┘
                               │
                               ▼ (for remote tracks)
                        Internet — SoundHelix CDN
```

## 6.2 Layered Architecture Model

| Layer | Components | Responsibility |
|-------|------------|----------------|
| **Presentation** | React components, CSS | Render UI, capture user input |
| **Application (FE)** | usePlayer, PlayerContext, App state | Client-side logic, playback, navigation |
| **API Client** | api/client.js | HTTP communication abstraction |
| **Transport** | HTTP/REST, Vite proxy | Request routing between processes |
| **Application (BE)** | Express routes, middleware | Business logic, query orchestration |
| **Data Access** | better-sqlite3, SQL | Persist and retrieve catalog |
| **Storage** | SQLite file, MP3 files | Physical data persistence |
| **External** | SoundHelix URLs | Remote audio streaming |

## 6.3 Process Topology

When the developer runs `npm run dev`, two Node.js processes start:

```
Process 1: Backend
  Command: node --watch src/server.js
  Port: 4000
  Responsibilities: API, static audio, SQLite

Process 2: Frontend
  Command: vite
  Port: 5173
  Responsibilities: Serve React app, proxy /api and /audio
```

The root `package.json` uses `concurrently` to launch both with labeled, color-coded output (`api` in green, `ui` in cyan).

## 6.4 Communication Patterns

**Frontend → Backend (JSON API)**
- Pattern: Request-response over HTTP GET
- Format: JSON payloads
- Example: `GET /api/home` → `{ playlists: [], recent: [], albums: [] }`

**Frontend → Backend (Audio)**
- Pattern: Browser media fetch
- Format: MP3 binary stream
- Example: `GET /audio/perfect.mp3` → audio/mpeg stream

**Backend → SQLite**
- Pattern: Synchronous prepared statements
- Format: Row objects
- Example: `db.prepare('SELECT ...').all()`

**CLI Scripts → SQLite**
- Pattern: Direct database access (seed, import-audio)
- No HTTP involved

## 6.5 Deployment Topology (Local)

```
spotify-local/
├── package.json              ← orchestration
├── backend/
│   ├── src/
│   │   ├── server.js         ← HTTP entry point
│   │   ├── db.js
│   │   ├── seed.js
│   │   └── import-audio.js
│   ├── data/
│   │   └── spotify.db        ← generated, gitignored
│   └── audio/
│       └── perfect.mp3       ← user content, gitignored
└── frontend/
    ├── src/                  ← React source
    ├── vite.config.js        ← proxy config
    └── dist/                 ← production build output
```

---

# 7. Domain Model and Data Design

## 7.1 Entity Relationship Overview

```
┌──────────┐       ┌──────────┐       ┌──────────┐
│  Artist  │───1:N─│  Album   │───1:N─│  Track   │
└──────────┘       └──────────┘       └────┬─────┘
                                           │
                                           │ N:M
                                           │
                                    ┌──────┴───────┐
                                    │playlist_tracks│
                                    └──────┬───────┘
                                           │
                                    ┌──────┴───────┐
                                    │  Playlist    │
                                    └──────────────┘
```

**Cardinality:**
- One artist has many albums
- One album has many tracks
- One playlist has many tracks (via join table)
- One track can appear in many playlists

## 7.2 Database Schema

```sql
CREATE TABLE artists (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  image_url TEXT
);

CREATE TABLE albums (
  id INTEGER PRIMARY KEY,
  title TEXT NOT NULL,
  artist_id INTEGER NOT NULL REFERENCES artists(id),
  cover_url TEXT,
  year INTEGER
);

CREATE TABLE tracks (
  id INTEGER PRIMARY KEY,
  title TEXT NOT NULL,
  album_id INTEGER NOT NULL REFERENCES albums(id),
  duration_ms INTEGER NOT NULL,
  audio_url TEXT NOT NULL,
  track_number INTEGER DEFAULT 1
);

CREATE TABLE playlists (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT,
  cover_url TEXT
);

CREATE TABLE playlist_tracks (
  playlist_id INTEGER NOT NULL REFERENCES playlists(id),
  track_id INTEGER NOT NULL REFERENCES tracks(id),
  position INTEGER NOT NULL,
  PRIMARY KEY (playlist_id, track_id)
);
```

## 7.3 Table Descriptions

### artists
Stores music artist information. Each artist has a display name and optional profile image URL.

### albums
Represents a music album belonging to exactly one artist. Includes title, cover art URL, and release year.

### tracks
Individual songs. Each track belongs to one album and stores:
- `duration_ms` — length in milliseconds (displayed as m:ss in UI)
- `audio_url` — either remote HTTPS URL or local path like `/audio/perfect.mp3`
- `track_number` — ordering within the album

### playlists
Curated collections of tracks. Contains name, description, and cover art. Tracks are linked via the join table.

### playlist_tracks
Associates tracks with playlists and defines play order via the `position` column. Composite primary key prevents duplicate track entries in the same playlist.

## 7.4 Relationships and Constraints

- **Foreign keys enabled** via `PRAGMA foreign_keys = ON`
- **WAL journal mode** for better concurrent read performance
- **No cascading deletes** in schema — seed script manually deletes in correct order
- **No auto-increment** — seed script assigns explicit IDs for predictable references

## 7.5 Seed Data Catalog

### Artists (4)

| ID | Name |
|----|------|
| 1 | Neon Pulse |
| 2 | Midnight Echo |
| 3 | Solar Drift |
| 4 | Ed Sheeran |

### Albums (4)

| ID | Title | Artist | Year |
|----|-------|--------|------|
| 1 | City Lights | Neon Pulse | 2024 |
| 2 | After Hours | Midnight Echo | 2023 |
| 3 | Horizon Run | Solar Drift | 2025 |
| 4 | ÷ (Divide) | Ed Sheeran | 2017 |

### Tracks (7)

| ID | Title | Album | Audio Source |
|----|-------|-------|--------------|
| 1 | Downtown Drive | City Lights | SoundHelix remote |
| 2 | Late Night Loop | City Lights | SoundHelix remote |
| 3 | Velvet Sky | After Hours | SoundHelix remote |
| 4 | Echo Chamber | After Hours | SoundHelix remote |
| 5 | Sunrise Sprint | Horizon Run | SoundHelix remote |
| 6 | Coastal Wind | Horizon Run | SoundHelix remote |
| 7 | Perfect | ÷ (Divide) | Local `/audio/perfect.mp3` |

### Playlists (3)

| ID | Name | Description |
|----|------|-------------|
| 1 | Today's Hits | Fresh picks for your day |
| 2 | Focus Flow | Deep work without distraction |
| 3 | Chill Vibes | Easy listening for unwinding |

*Perfect* appears first in Today's Hits and Chill Vibes playlists.

## 7.6 API Data Shapes

Every track returned by the API includes joined fields for immediate UI rendering:

```json
{
  "id": 7,
  "title": "Perfect",
  "duration_ms": 263000,
  "audio_url": "/audio/perfect.mp3",
  "track_number": 5,
  "album_id": 4,
  "album_title": "÷ (Divide)",
  "album_cover": "https://picsum.photos/seed/divide/400/400",
  "artist_id": 4,
  "artist_name": "Ed Sheeran"
}
```

This flat structure avoids additional frontend requests for album or artist metadata when displaying track rows.

---

# 8. Backend System Design

## 8.1 Backend Overview

The backend is a single-file Express application (`server.js`) supported by three utility modules. It follows a **thin controller** pattern: route handlers parse requests, execute SQL, and return JSON. No service layer or ORM is used — SQL is written directly for transparency and simplicity.

## 8.2 Server Initialization

On startup, the server:

1. Creates `backend/data/` and `backend/audio/` directories if missing
2. Opens the SQLite database and initializes schema
3. Logs a warning if the database has zero tracks (prompts user to run seed)
4. Configures Express middleware
5. Registers all route handlers
6. Listens on port 4000 (or `process.env.PORT`)

## 8.3 Middleware Stack

| Order | Middleware | Purpose |
|-------|------------|---------|
| 1 | cors() | Allow cross-origin requests (dev convenience) |
| 2 | express.json() | Parse JSON request bodies (future-proofing) |
| 3 | express.static('/audio') | Serve local MP3 files |

No authentication middleware, rate limiting, or request logging is configured.

## 8.4 REST API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | /api/health | Health check with track count |
| GET | /api/home | Aggregated home page data |
| GET | /api/playlists | All playlists with track counts |
| GET | /api/playlists/:id | Single playlist with ordered tracks |
| GET | /api/albums/:id | Single album with ordered tracks |
| GET | /api/tracks | All tracks, or search with ?q= |
| GET | /api/tracks/:id | Single track by ID |
| GET | /audio/* | Static MP3 file serving |

All endpoints are **read-only**. No POST, PUT, PATCH, or DELETE handlers exist.

## 8.5 SQL Query Strategy

The `trackSelect()` function centralizes the track join query used by every endpoint that returns tracks:

```sql
SELECT
  t.id, t.title, t.duration_ms, t.audio_url, t.track_number,
  a.id AS album_id, a.title AS album_title, a.cover_url AS album_cover,
  ar.id AS artist_id, ar.name AS artist_name
FROM tracks t
JOIN albums a ON a.id = t.album_id
JOIN artists ar ON ar.id = a.artist_id
```

Endpoints append WHERE, ORDER BY, and LIMIT clauses as needed. Search uses SQL `LIKE` with `%term%` wildcards on title, artist name, and album title.

## 8.6 Static Audio Serving

```javascript
app.use('/audio', express.static(audioDir));
```

Files in `backend/audio/` are served with standard HTTP semantics including `Accept-Ranges: bytes` for seeking support. The browser's Audio element relies on range requests for efficient seeking in MP3 files.

## 8.7 Database Module (`db.js`)

Exports two functions:

- `openDb()` — Opens SQLite connection with WAL and foreign key pragmas
- `initSchema(db)` — Creates all tables if they do not exist

Database path: `backend/data/spotify.db`

## 8.8 Seed Script (`seed.js`)

The seed script performs a **full reset** of catalog data:

1. DELETE all rows (playlist_tracks → tracks → albums → artists → playlists)
2. INSERT demo data inside a transaction
3. Log summary counts

Running seed is idempotent — it always produces the same baseline catalog. User-imported MP3 files in `backend/audio/` are **not** deleted by seeding; only database references are reset.

## 8.9 Import Audio Script (`import-audio.js`)

CLI tool for associating local MP3 files with tracks:

```
npm run import-audio -- <source-path> [trackId] [destFilename]
```

**Default behavior:**
- Copies file to `backend/audio/perfect.mp3`
- Updates track ID 7 with `audio_url = '/audio/perfect.mp3'`

This script was used to import the user's downloaded *Ed Sheeran - Perfect (Official Music Video).mp3* from their Downloads folder.

---

# 9. Frontend System Design

## 9.1 Frontend Overview

The frontend is a React single-page application with no external routing library. State is managed through React hooks at the app level (navigation) and context level (audio player). All data fetching occurs in view components via the centralized API client.

## 9.2 Application Entry and Bootstrap

**`main.jsx`** mounts the root React component:

```javascript
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

**`index.html`** provides the DOM mount point and loads the Vite-bundled JavaScript module.

## 9.3 Application Shell (`App.jsx`)

The App component defines the persistent layout:

```
PlayerProvider
└── div.app
    ├── Sidebar
    ├── main → {current view}
    └── PlayerBar
```

**State variables:**
- `view` — current screen identifier
- `detail` — `{ kind, id }` when viewing playlist/album detail

**Key functions:**
- `openDetail(kind, id)` — navigate to detail view
- `renderMain()` — switch between HomeView, SearchView, LibraryView, DetailView

## 9.4 Navigation Model

| User Action | State Change | Component Rendered |
|-------------|--------------|-------------------|
| Click Home in sidebar | view = 'home' | HomeView |
| Click Search | view = 'search' | SearchView |
| Click Library | view = 'library' | LibraryView |
| Click playlist/album card | view = 'detail', detail = {kind, id} | DetailView |
| Click Back on detail | view = 'library', detail = null | LibraryView |

There is no URL-based routing. Refreshing the browser always returns to the Home view.

## 9.5 API Client Layer

**`api/client.js`** provides a typed interface to all backend endpoints:

```javascript
export const api = {
  health: () => request('/health'),
  home: () => request('/home'),
  playlists: () => request('/playlists'),
  playlist: (id) => request(`/playlists/${id}`),
  album: (id) => request(`/albums/${id}`),
  search: (q) => request(`/tracks?q=${encodeURIComponent(q)}`),
};
```

The `request()` helper handles fetch, error parsing, and JSON deserialization. All paths are relative to `/api`, which Vite proxies to the backend.

## 9.6 View Components

### HomeView
- Fetches `/api/home` on mount
- Displays greeting hero section
- Renders "Made for you" playlist grid
- Renders "Jump back in" album grid
- Renders "Recently added" track list (6 most recent tracks)

### SearchView
- Provides search input with autofocus
- Debounces queries by 300ms
- Calls `/api/tracks?q=` when query is non-empty
- Shows loading state and results in TrackList

### LibraryView
- Fetches all playlists
- Renders playlist grid via CardGrid

### DetailView
- Accepts `kind` ('playlist' | 'album'), `id`, and `onBack` callback
- Fetches appropriate endpoint based on kind
- Renders hero section with large cover art
- Renders full track list

## 9.7 Shared UI Components

### CardGrid
Reusable grid of clickable cards for playlists or albums. Invokes `onOpen(type, id)` when a card is clicked.

### TrackList
Tabular list of tracks with:
- Row number or playing indicator (♪)
- Thumbnail, title, artist
- Optional album column
- Duration formatted as m:ss
- Click handler: `playTrack(track, tracks)` — entire list becomes queue

### Sidebar
Three navigation buttons with active state highlighting. Logo area at top.

### PlayerBar
Three-column footer:
- **Left:** Now-playing info (cover, title, artist)
- **Center:** Transport controls and seek bar
- **Right:** Spotify-style icons (lyrics, queue, device, volume, mini-player, fullscreen)

### PlayerBarIcons
SVG icon components matching Spotify's visual language. Volume icon adapts to mute/low/high states.

## 9.8 Styling and Layout System

**Layout:** CSS Grid on `.app`
- Columns: 240px sidebar + flexible main
- Rows: flexible content + 90px player bar
- Full viewport height (`100vh`)

**Theme:** CSS custom properties
- Background: `#000000`, `#121212`, `#181818`
- Accent: `#1db954` (Spotify green)
- Text: `#ffffff` primary, `#b3b3b3` muted

**Responsive:** At 900px width, sidebar text labels hide, showing icons only.

## 9.9 Vite Configuration and Proxy

```javascript
export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
    proxy: {
      '/api': 'http://localhost:4000',
      '/audio': 'http://localhost:4000',
    },
  },
});
```

The proxy makes the frontend and backend appear same-origin to the browser, eliminating CORS issues for both JSON API calls and audio streaming.

---

# 10. Audio Playback System

## 10.1 Audio Architecture Overview

Audio playback is entirely client-side. The backend's role is limited to storing `audio_url` values and serving local files. No server-side transcoding, playlist scheduling, or WebSocket synchronization exists.

```
User clicks track
    → playTrack(track, allTracks)
    → loadTrack(track, queue, index)
    → audio.src = track.audio_url
    → audio.play()
    → Browser fetches audio (remote URL or /audio/* proxy)
    → PlayerBar reflects state via React context
```

## 10.2 Remote vs Local Audio Sources

| Type | Example URL | Served By | Network Required |
|------|-------------|-----------|------------------|
| Remote | `https://www.soundhelix.com/.../Song-1.mp3` | SoundHelix CDN | Yes |
| Local | `/audio/perfect.mp3` | Express static → Vite proxy | No (after import) |

Local audio enables offline playback of imported files once they are copied into the project.

## 10.3 Player Engine (`usePlayer.js`)

The custom hook creates and manages a single `HTMLAudioElement` for the application lifetime.

**Refs (persist across renders):**
- `audioRef` — the Audio DOM element
- `queueRef` — current track array
- `queueIndexRef` — current position
- `loadTrackRef` — stable reference for event handlers

**React state (triggers re-renders):**
- `currentTrack`, `queue`, `queueIndex`
- `isPlaying`, `progress`, `duration`, `volume`

**Event listeners:**
- `timeupdate` → updates progress
- `loadedmetadata` → sets duration
- `play` / `pause` → syncs isPlaying
- `ended` → auto-advances to next track

## 10.4 Queue Management

When a user clicks a track in any list:

1. That list becomes the **queue**
2. The clicked track's index becomes the **queue position**
3. Next/Previous navigate within the queue
4. At queue end, next wraps to beginning (circular)
5. Previous within first 3 seconds restarts current track instead of going back

This mimics standard music player behavior.

## 10.5 Player Context and Global State

`PlayerProvider` wraps the entire app and exposes `usePlayerContext()` to any descendant component. This ensures:

- One audio instance (no overlapping playback)
- PlayerBar and TrackList stay synchronized
- Playing indicator updates across all views

## 10.6 Player Bar UI

| Section | Controls | Wired Up |
|---------|----------|----------|
| Left | Cover art, title, artist | ✅ Display only |
| Center | Prev, Play/Pause, Next | ✅ Functional |
| Center | Seek slider, time labels | ✅ Functional |
| Right | Lyrics icon | ❌ Decorative |
| Right | Queue icon | ❌ Decorative |
| Right | Device icon | ❌ Decorative |
| Right | Volume icon + slider | ✅ Functional |
| Right | Mini player icon | ❌ Decorative |
| Right | Fullscreen icon | ❌ Decorative |

## 10.7 Local Audio Import Workflow

```
1. User obtains MP3 legally (purchase, rip, download)
2. npm run import-audio -- /path/to/file.mp3
3. Script copies to backend/audio/perfect.mp3
4. Script updates tracks.audio_url in SQLite
5. User plays "Perfect" in the app
6. Browser requests GET /audio/perfect.mp3
7. Vite proxies to Express static handler
8. MP3 streams to HTML5 Audio element
```

---

# 11. User Interface Design

## 11.1 Design Philosophy

The UI intentionally mirrors Spotify's design language:

- **Dark-first** — reduces eye strain, matches streaming app conventions
- **Content-forward** — large cover art, minimal chrome
- **Persistent player** — music controls always accessible
- **Familiar navigation** — left sidebar pattern used by Spotify, Discord, Slack

The design is implemented in plain CSS without component libraries, demonstrating that polished UIs do not require heavy frameworks like Tailwind or Material UI.

## 11.2 Layout Structure

```
┌────────────┬──────────────────────────────────────────┐
│            │                                          │
│  Sidebar   │           Main Content                   │
│  (240px)   │           (scrollable)                   │
│            │                                          │
│  🏠 Home   │   View-specific content:                 │
│  🔍 Search │   - Hero header                          │
│  📚 Library│   - Card grids                           │
│            │   - Track lists                          │
│            │                                          │
├────────────┼──────────────────────────────────────────┤
│            │  PlayerBar (90px)                        │
│            │  [track info] [controls] [extra icons]   │
└────────────┴──────────────────────────────────────────┘
```

## 11.3 Color Palette and Theme

| Token | Value | Usage |
|-------|-------|-------|
| --bg-base | #000000 | Page background |
| --bg-elevated | #121212 | Sidebar |
| --bg-card | #181818 | Cards, player bar |
| --bg-card-hover | #282828 | Hover states |
| --text | #ffffff | Primary text |
| --text-muted | #b3b3b3 | Secondary text |
| --accent | #1db954 | Active states, sliders |
| --accent-hover | #1ed760 | Button hover |

## 11.4 Screen Inventory

| Screen | Route State | Primary Actions |
|--------|-------------|-----------------|
| Home | view='home' | Browse playlists/albums, play recent tracks |
| Search | view='search' | Type query, play search results |
| Library | view='library' | Browse playlists, open detail |
| Playlist Detail | view='detail', kind='playlist' | View tracks, play playlist |
| Album Detail | view='detail', kind='album' | View tracks, play album |

## 11.5 Player Bar Controls

The player bar is divided into three equal columns using CSS Grid (`1fr 2fr 1fr`). Center column is wider to accommodate the seek slider. Transport buttons use Unicode symbols (⏮ ▶ ⏸ ⏭) for simplicity.

## 11.6 Responsive Behavior

At viewport widths below 900px:
- Sidebar collapses to icon-only (text labels hidden)
- Album column hidden in track lists
- Detail hero stacks vertically
- Album title font size reduced

The app remains functional on tablet-sized screens but is optimized for desktop use.

---

# 12. End-to-End User Flows

## 12.1 Application Startup Flow

```
Developer: npm run dev
    → concurrently starts backend (:4000) and frontend (:5173)
    → Backend opens SQLite, mounts routes
    → Vite serves React app with proxy

User: Opens http://localhost:5173
    → React mounts App in StrictMode
    → PlayerProvider initializes Audio element
    → HomeView mounts, fetches GET /api/home
    → Backend queries playlists, recent tracks, albums
    → JSON response renders Home screen
    → PlayerBar shows "Select a track to play"
```

## 12.2 Home Browsing Flow

```
User lands on Home
    → Sees "Good evening" greeting
    → "Made for you" shows 3 playlist cards
    → "Jump back in" shows 4 album cards
    → "Recently added" shows 6 tracks (Perfect listed first)

User clicks playlist card "Today's Hits"
    → App.openDetail('playlist', 1)
    → DetailView fetches GET /api/playlists/1
    → Shows playlist hero + track list
    → Perfect is track #1 in the list
```

## 12.3 Search and Discovery Flow

```
User clicks Search in sidebar
    → SearchView mounts with autofocus input
    → User types "perfect"
    → 300ms debounce elapses
    → GET /api/tracks?q=perfect
    → Backend: LIKE '%perfect%' on title, artist, album
    → Returns matching tracks
    → TrackList renders results with album column
```

## 12.4 Library and Playlist Flow

```
User clicks Library
    → LibraryView fetches GET /api/playlists
    → Shows all playlists with cover art
    → User clicks "Chill Vibes"
    → DetailView shows playlist with Perfect as first track
    → User clicks Perfect row
    → playTrack(track, allPlaylistTracks)
    → Queue = entire Chill Vibes playlist
    → Playback begins
```

## 12.5 Album Detail Flow

```
User on Home clicks "÷ (Divide)" album card
    → App.openDetail('album', 4)
    → DetailView fetches GET /api/albums/4
    → Shows album cover, artist, year
    → Track list shows Perfect (track #5 on album)
    → User can play any album track; queue = full album
```

## 12.6 Track Playback Flow

```
User clicks any track row
    → playTrack(track, tracksArray)
    → loadTrack sets:
        - currentTrack = track
        - queue = tracksArray
        - audio.src = track.audio_url
    → audio.play() called
    → PlayerBar updates:
        - Cover art from track.album_cover
        - Title and artist name
        - Play button becomes pause
    → Track row shows ♪ indicator
    → Progress bar advances via timeupdate events
```

## 12.7 Queue and Auto-Advance Flow

```
Track reaches end (ended event fires)
    → queueIndexRef incremented (wraps at end)
    → loadTrack called with next track
    → Playback continues seamlessly

User clicks Next button
    → Same logic as auto-advance

User clicks Previous within 3 seconds
    → currentTime reset to 0 (restart track)

User clicks Previous after 3 seconds
    → Navigate to previous queue index
```

## 12.8 Local Song Import Flow

```
User downloads Ed Sheeran - Perfect.mp3 to ~/Downloads
    → npm run import-audio -- "~/Downloads/Ed Sheeran - Perfect (Official Music Video).mp3"
    → File copied to backend/audio/perfect.mp3
    → DB updated: track 7 audio_url = '/audio/perfect.mp3'
    → User searches "Perfect" and clicks play
    → Browser streams local file through proxy
    → Original recording plays (not SoundHelix demo)
```

## 12.9 Complete Session Walkthrough

The following narrative describes a typical complete user session from start to finish, demonstrating how all system parts interact:

**Minute 0 — Launch:** The developer runs `npm run dev` in the project root. The terminal shows both servers starting. The user opens Chrome and navigates to `http://localhost:5173`. React bootstraps, PlayerProvider creates the Audio element, and HomeView fires `GET /api/home`. Within 200ms, the home page renders with three playlist cards, four album cards, and six recent tracks. *Perfect* by Ed Sheeran appears at the top of "Recently added."

**Minute 1 — Explore Home:** The user scans "Made for you" playlists. "Today's Hits" shows a colorful cover from picsum.photos. The user clicks it. App state changes to `view='detail'`, `detail={kind:'playlist', id:1}`. DetailView mounts and fetches `GET /api/playlists/1`. The playlist hero shows the name, description, and large cover art. Below, a track list shows four tracks with *Perfect* at position 1.

**Minute 2 — First Playback:** The user clicks the *Perfect* row. `playTrack` is called with the full playlist as queue. The Audio element's `src` is set to `/audio/perfect.mp3`. The browser requests the file through Vite's proxy. Express serves the 4MB MP3 from disk. Audio begins playing. The PlayerBar animates: cover art appears, "Perfect" and "Ed Sheeran" display, the play button becomes pause, and the ♪ indicator replaces the row number.

**Minute 3 — Player Controls:** The user drags the seek bar to the middle of the song. `seek(50)` sets `audio.currentTime` to half the duration. The progress label updates. The user lowers the volume slider to 50%. The `volume` state updates and the Audio element's volume property changes. The volume icon SVG switches to a lower-waveform variant.

**Minute 4 — Queue Navigation:** Without waiting for the song to end, the user clicks the Next button. `playNext()` advances the queue index from 0 to 1. "Downtown Drive" by Neon Pulse begins playing — this time from a SoundHelix remote URL. The player bar updates with new metadata. The ♪ indicator moves to the second row.

**Minute 5 — Search:** The user clicks Search in the sidebar. SearchView mounts with an autofocused input. They type "sheeran". After 300ms debounce, `GET /api/tracks?q=sheeran` returns the *Perfect* track. The user clicks it. Playback switches back to *Perfect* with a single-item queue.

**Minute 6 — Album Browse:** The user clicks Library, then navigates to Home via sidebar. They click the "÷ (Divide)" album card under "Jump back in." Album detail shows Ed Sheeran's 2017 album with *Perfect* as track 5. The user plays it again from the album context — this time the queue contains all album tracks.

**Minute 7 — Session End:** The user pauses playback. The PlayerBar shows the paused state. They close the browser tab. The Audio element is garbage collected. The backend continues running, ready for the next session.

This walkthrough demonstrates that every major feature — home browsing, detail views, local and remote playback, search, queue navigation, and player controls — works together seamlessly in a single continuous session.

## 12.10 Error and Edge Case Flows

### Empty Database

If the user starts the app without seeding:
- Backend logs: "Database empty — run: npm run seed"
- Home page API returns empty arrays
- TrackList shows "No tracks found"
- Player bar shows "Select a track to play"
- **Fix:** Run `npm run seed`

### Missing Local Audio File

If `perfect.mp3` is not imported but the DB references `/audio/perfect.mp3`:
- Track appears in UI normally
- Clicking play sets audio.src but fetch returns 404
- Audio element fires error; playback silently fails
- **Fix:** Run `npm run import-audio -- /path/to/file.mp3`

### Backend Not Running

If only the frontend is started:
- All API fetches fail with network error
- Views show error messages ("Request failed" or "Failed to fetch")
- **Fix:** Start backend with `npm run dev:backend` or full `npm run dev`

### Autoplay Blocked

Some browsers block autoplay without user interaction:
- `audio.play()` rejects the promise
- `isPlaying` stays false even though a track is loaded
- User must click the play button manually
- This is expected browser behavior, not a bug

---

# 13. Installation and Setup Guide

## 13.1 Prerequisites

| Requirement | Minimum Version |
|-------------|-----------------|
| Node.js | 18.x or later |
| npm | 9.x or later |
| Modern browser | Chrome 100+, Firefox 100+, Safari 15+ |
| Disk space | ~100 MB (excluding imported MP3s) |
| Network | Required for remote demo tracks and cover art |

## 13.2 First-Time Setup

```bash
cd spotify-local

# Install root + backend + frontend dependencies
npm install
npm run install:all

# Create and populate SQLite database
npm run seed
```

The `setup` script combines install and seed: `npm run setup`

## 13.3 Running the Application

```bash
npm run dev
```

Expected output:
```
[api] Spotify API running at http://localhost:4000
[ui]  VITE ready — Local: http://localhost:5173/
```

Open **http://localhost:5173** in your browser.

## 13.4 Running Services Separately

```bash
# Terminal 1 — Backend only
cd backend && npm run dev

# Terminal 2 — Frontend only
cd frontend && npm run dev
```

Useful when debugging one layer in isolation.

## 13.5 Importing Local Audio Files

```bash
# Import and link to track 7 (Perfect)
npm run import-audio -- /path/to/your/song.mp3

# Custom track ID and filename
npm run import-audio -- /path/to/song.mp3 3 my-song.mp3
```

## 13.6 Resetting the Database

```bash
npm run seed
```

This wipes all catalog data and restores the default demo catalog. Local MP3 files in `backend/audio/` are preserved but may need re-import if filenames changed.

---

# 14. Development Workflow

## 14.1 Repository Structure

See Section 6.5 and `docs/CODE.md` for the complete file tree. The monorepo contains three npm packages: root (orchestration), backend (API), and frontend (UI).

## 14.2 npm Scripts Reference

### Root Package

| Script | Description |
|--------|-------------|
| `install:all` | Install backend and frontend dependencies |
| `setup` | Full first-time setup |
| `dev` | Run both servers concurrently |
| `dev:backend` | Backend only |
| `dev:frontend` | Frontend only |
| `seed` | Re-seed database |
| `import-audio` | Import local MP3 file |

### Backend Package

| Script | Description |
|--------|-------------|
| `dev` | Start with file watching |
| `start` | Production start (no watch) |
| `seed` | Run seed script |
| `import-audio` | Run import script |

### Frontend Package

| Script | Description |
|--------|-------------|
| `dev` | Vite dev server |
| `build` | Production build to dist/ |
| `preview` | Preview production build |

## 14.3 Adding New Tracks

1. Add artist/album entries to `seed.js` if needed
2. Add track entry with `audio_url` (remote or `/audio/filename.mp3`)
3. Add to playlist in `playlistTracks` array
4. If local audio: place MP3 in `backend/audio/` or use import script
5. Run `npm run seed`
6. No frontend changes required — existing views pick up new data automatically

## 14.4 Modifying the API

1. Edit route handlers in `server.js`
2. Backend auto-restarts via `--watch`
3. Update `api/client.js` if new endpoints added
4. Update consuming view components

## 14.5 Modifying the UI

1. Edit components in `frontend/src/components/`
2. Styles in `frontend/src/index.css`
3. Vite HMR updates browser instantly
4. Player changes may require editing `usePlayer.js`

## 14.6 Git and Ignored Files

| Path | Reason |
|------|--------|
| `node_modules/` | Dependencies |
| `backend/data/` | Generated SQLite database |
| `backend/audio/*` | User's MP3 files (keeps .gitkeep) |
| `dist/` | Production build |
| `*.log` | Log files |

---

# 15. API Reference

## 15.1 Health Check

```
GET /api/health
```

**Response 200:**
```json
{ "ok": true, "tracks": 7 }
```

## 15.2 Home Endpoint

```
GET /api/home
```

**Response 200:**
```json
{
  "playlists": [{ "id": 1, "name": "...", "description": "...", "cover_url": "..." }],
  "recent": [{ "id": 7, "title": "Perfect", "...": "..." }],
  "albums": [{ "id": 4, "title": "÷ (Divide)", "artist_name": "Ed Sheeran", "...": "..." }]
}
```

## 15.3 Playlists Endpoints

```
GET /api/playlists
GET /api/playlists/:id
```

List response includes `track_count`. Detail response includes full `tracks[]` ordered by position.

## 15.4 Albums Endpoint

```
GET /api/albums/:id
```

Returns album metadata plus `tracks[]` ordered by `track_number`.

## 15.5 Tracks Endpoints

```
GET /api/tracks          → all tracks
GET /api/tracks?q=term   → search
GET /api/tracks/:id      → single track
```

Search is case-sensitive LIKE matching on title, artist name, and album title. Maximum 50 results.

## 15.6 Static Audio Endpoint

```
GET /audio/:filename
```

Returns MP3 binary with appropriate Content-Type. Supports byte-range requests.

## 15.7 Error Responses

```json
{ "error": "Playlist not found" }
{ "error": "Album not found" }
{ "error": "Track not found" }
```

HTTP status 404 for missing resources.

---

# 16. Component Reference

## 16.1 Backend Files

| File | Lines (approx) | Purpose |
|------|----------------|---------|
| server.js | 135 | Express app, routes, static audio |
| db.js | 55 | SQLite connection and schema |
| seed.js | 72 | Demo data population |
| import-audio.js | 40 | CLI audio import tool |

## 16.2 Frontend Files

| File | Purpose |
|------|---------|
| main.jsx | React entry point |
| App.jsx | Shell layout and view routing |
| index.css | Global styles (~460 lines) |
| api/client.js | HTTP client |
| hooks/usePlayer.js | Audio engine (~130 lines) |
| hooks/PlayerContext.jsx | Context provider |
| components/Sidebar.jsx | Navigation |
| components/HomeView.jsx | Home screen |
| components/SearchView.jsx | Search screen |
| components/LibraryView.jsx | Library screen |
| components/DetailView.jsx | Playlist/album detail |
| components/CardGrid.jsx | Card grid layout |
| components/TrackList.jsx | Track row list |
| components/PlayerBar.jsx | Bottom player |
| components/PlayerBarIcons.jsx | SVG icons |

## 16.3 File Dependency Graph

```
main.jsx → App.jsx
  App.jsx → PlayerProvider → usePlayer.js
  App.jsx → Sidebar, HomeView, SearchView, LibraryView, DetailView, PlayerBar
  Views → api/client.js → Backend HTTP
  TrackList → usePlayerContext → playTrack()
  PlayerBar → usePlayerContext → controls
  server.js → db.js
  seed.js → db.js
  import-audio.js → db.js
```

---

# 17. Testing and Validation

## 17.1 Manual Test Plan

| # | Test Case | Expected Result |
|---|-----------|-----------------|
| 1 | Open http://localhost:5173 | Home page loads with playlists |
| 2 | Click Home/Search/Library | Views switch correctly |
| 3 | Click playlist card | Detail view with tracks |
| 4 | Click album card | Album detail with tracks |
| 5 | Click track row | Playback starts, player bar updates |
| 6 | Click pause | Audio pauses |
| 7 | Drag seek slider | Playback position changes |
| 8 | Adjust volume | Volume changes |
| 9 | Click next/previous | Queue navigation works |
| 10 | Let track finish | Next track auto-plays |
| 11 | Search "Perfect" | Ed Sheeran track appears |
| 12 | Play Perfect | Local MP3 streams and plays |
| 13 | GET /api/health | Returns ok: true |
| 14 | npm run seed | Database resets, app still works |

## 17.2 API Verification

```bash
curl http://localhost:4000/api/health
curl http://localhost:4000/api/home
curl 'http://localhost:4000/api/tracks?q=Perfect'
curl -I http://localhost:4000/audio/perfect.mp3
```

## 17.3 Playback Verification

1. Open browser DevTools → Network tab
2. Play a SoundHelix track — verify request to soundhelix.com
3. Play Perfect — verify request to `/audio/perfect.mp3` returns 200
4. Verify seek bar updates during playback
5. Verify ♪ indicator on active track row

## 17.4 Common Issues and Debugging

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Empty home page | Database not seeded | `npm run seed` |
| API fetch failed | Backend not running | Start with `npm run dev` |
| Track won't play (404) | MP3 not imported | `npm run import-audio` |
| No sound (remote) | No internet | Check network; use local import |
| CORS error | Hitting :4000 from :5173 without proxy | Use Vite dev server URL |
| Port in use | Previous process running | Kill process on 4000/5173 |

---

# 18. Security and Legal Considerations

## 18.1 Security Posture

Spotify Local is a **local development demo** with minimal security measures:

- No authentication or authorization
- No input sanitization beyond SQL parameterization
- CORS enabled for all origins
- No HTTPS (local HTTP only)
- No rate limiting

This is acceptable for localhost-only use but **must not be exposed to the public internet** without hardening.

## 18.2 Copyright and Audio Content

- **SoundHelix tracks** are royalty-free demo samples provided for testing
- **Imported MP3s** (such as Ed Sheeran — Perfect) are the user's responsibility
- Users must own or have legal rights to any audio they import
- The project is **not affiliated with Spotify** or any record label
- Cover art uses picsum.photos placeholders, not official album artwork

## 18.3 Local-Only Design Benefits

Running locally means:
- No user data transmitted to third parties (except remote audio/art URLs)
- Imported music stays on the user's machine
- Database file is private to the developer's filesystem
- No cloud billing or API key management

---

# 19. Future Enhancements

## 19.1 Short-Term Improvements

- Wire queue icon to show current queue in a slide-out panel
- Add React Router for shareable URLs (`/playlist/1`, `/album/4`)
- Persist volume and last-played track in localStorage
- Display actual album artwork for Ed Sheeran — Perfect
- Add loading skeletons instead of "Loading…" text

## 19.2 Medium-Term Features

- REST API write endpoints for creating/editing playlists
- Drag-and-drop UI for playlist management
- Upload endpoint for audio files (replacing CLI import)
- Shuffle and repeat modes
- Keyboard shortcuts (space = play/pause, arrows = seek)

## 19.3 Long-Term Vision

- User accounts with JWT authentication
- Full local music library scanner (scan a folder of MP3s)
- Integration with MusicBrainz for metadata enrichment
- PWA with offline support
- Docker Compose for one-command deployment
- Mobile-responsive redesign or React Native companion app

---

# 20. Detailed Implementation Walkthrough

This section provides an in-depth, implementation-level tour of the Spotify Local codebase. It is intended for developers who need to understand not just *what* the system does, but *exactly how* each part is implemented.

## 20.1 Backend Route Handler Deep Dive

### GET /api/home

The home endpoint aggregates three separate queries into one response object. This design minimizes frontend round-trips — HomeView needs only a single fetch on mount.

**Query 1 — Playlists:**
```sql
SELECT id, name, description, cover_url FROM playlists ORDER BY id
```
Returns all three seeded playlists in stable order.

**Query 2 — Recent tracks:**
```sql
SELECT ... FROM tracks t JOIN albums a ... JOIN artists ar ...
ORDER BY t.id DESC LIMIT 6
```
Returns the six most recently added tracks by ID. Because *Perfect* (id=7) has the highest ID, it appears first in the "Recently added" section after seeding.

**Query 3 — Albums:**
```sql
SELECT a.id, a.title, a.cover_url, a.year, ar.name AS artist_name
FROM albums a JOIN artists ar ON ar.id = a.artist_id ORDER BY a.id
```
Returns all albums with artist name denormalized for card display.

### GET /api/playlists/:id

This endpoint performs two queries:

1. Fetch playlist metadata by ID — returns 404 if not found
2. Fetch tracks joined through `playlist_tracks`, ordered by `position`

The position ordering is critical: it ensures *Perfect* appears first in "Today's Hits" because seed data assigns it `position = 1`.

### GET /api/tracks (search)

When the `q` query parameter is present and non-empty after trimming:

```sql
WHERE t.title LIKE ? OR ar.name LIKE ? OR a.title LIKE ?
ORDER BY t.title LIMIT 50
```

The same `%term%` pattern is bound three times. This means searching "Ed" matches artist "Ed Sheeran", searching "Divide" matches album "÷ (Divide)", and searching "Perfect" matches the track title.

When `q` is absent or empty, all tracks are returned ordered by ID.

### Error Handling Pattern

All detail endpoints follow the same pattern:

```javascript
const entity = db.prepare('SELECT ... WHERE id = ?').get(req.params.id);
if (!entity) return res.status(404).json({ error: 'Entity not found' });
```

No try/catch wrappers — better-sqlite3 throws on SQL errors, which Express converts to 500 responses.

## 20.2 Frontend Component Deep Dive

### HomeView.jsx

```javascript
useEffect(() => {
  api.home()
    .then(setData)
    .catch((e) => setError(e.message));
}, []);
```

The empty dependency array ensures home data is fetched exactly once on mount. Three render states:
- `error` — displays error message
- `!data` — displays loading text
- `data` — renders full home content

`CardGrid` receives `onOpen={onOpen}` which is App's `openDetail` function, creating a callback chain: CardGrid → App → DetailView.

### SearchView.jsx

The debounce implementation uses `setTimeout` inside `useEffect`:

```javascript
useEffect(() => {
  if (!query.trim()) {
    setResults([]);
    return;
  }
  const t = setTimeout(() => {
    setLoading(true);
    api.search(query).then(setResults).finally(() => setLoading(false));
  }, 300);
  return () => clearTimeout(t);
}, [query]);
```

The cleanup function clears the timeout on every query change, preventing stale API responses from overwriting newer results. When query is empty, results are cleared immediately without an API call.

### DetailView.jsx

Uses a conditional fetcher based on `kind`:

```javascript
const fetcher = kind === 'playlist' ? api.playlist(id) : api.album(id);
```

When `kind` or `id` changes, the effect resets `data` to null (showing loading) before fetching. The hero section adapts its labels:
- Playlist: shows `data.name`, type label "Playlist"
- Album: shows `data.title`, type label "Album", subtitle includes artist and year

`showAlbum={kind === 'playlist'}` on TrackList — album column appears in playlist detail (since tracks come from different albums) but not in album detail (redundant).

### TrackList.jsx

Each track row is a `<button>` for accessibility and keyboard support. The playing indicator logic:

```javascript
currentTrack?.id === track.id && '♪' || i + 1
```

When the current track matches, a musical note replaces the row number. Duration formatting converts milliseconds to `m:ss` display format.

The critical playback invocation:

```javascript
onClick={() => playTrack(track, tracks)}
```

Passing the full `tracks` array ensures the queue includes all visible tracks, not just the clicked one.

### PlayerBar.jsx

Uses `formatTime(sec)` for progress display — converts seconds to `m:ss`. The seek slider value is a percentage (0–100) calculated from `progress / duration`. On change, `seek(Number(e.target.value))` converts back to a time position.

The right-side controls use `PlayerBarIcons` SVG components styled with `player-extra-btn` class — 32px circular hit targets matching Spotify's compact control density.

## 20.3 Player Hook Line-by-Line Behavior

### Initialization (useEffect with empty deps)

Creates the Audio element once for the app lifetime:

```javascript
const audio = new Audio();
audio.volume = volume;
audioRef.current = audio;
```

Event listeners are registered for the full playback lifecycle. The cleanup function on unmount pauses audio and removes all listeners — important for StrictMode double-mounting in development.

### loadTrack Function

```javascript
const loadTrack = useCallback(async (track, newQueue, index = 0) => {
  queueRef.current = newQueue;
  queueIndexRef.current = index;
  setCurrentTrack(track);
  setQueue(newQueue);
  setQueueIndex(index);
  audio.src = track.audio_url;
  setProgress(0);
  try {
    await audio.play();
  } catch {
    setIsPlaying(false);
  }
}, []);
```

Setting both refs and state ensures event handlers (which read refs) and UI (which reads state) stay synchronized. The try/catch on `play()` handles browser autoplay policies — if autoplay is blocked, the player sets `isPlaying` to false rather than throwing.

### playPrev Special Behavior

```javascript
if (audio && audio.currentTime > 3) {
  audio.currentTime = 0;
  return;
}
```

This matches Spotify behavior: pressing previous near the start of a track restarts it rather than going to the previous queue item. Only after 3 seconds does previous navigate the queue.

### Volume Sync

A separate useEffect watches `volume` state and applies it to the audio element. This decouples volume changes from the main player lifecycle.

## 20.4 CSS Architecture Breakdown

### Grid Layout

```css
.app {
  display: grid;
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-rows: 1fr var(--player-height);
  height: 100vh;
  overflow: hidden;
}
```

The sidebar spans both rows via `grid-row: 1 / 3`. The player bar occupies the bottom-right cell. Main content scrolls independently within its grid cell.

### Card Grid

Cards use CSS Grid with `auto-fill` and `minmax(180px, 1fr)` for responsive column counts. Hover transitions darken the background from `--bg-card` to `--bg-card-hover`.

### Track List

Track rows use a nested grid:

```css
grid-template-columns: 32px 1fr 1fr 64px;
```

Columns: number, title (with thumbnail), album (optional), duration. The `.playing` class highlights the active row.

### Player Bar

Three-column grid with `1fr 2fr 1fr` gives the center controls section twice the width of the side sections — necessary for the seek slider.

### Sliders

Both progress and volume sliders use `accent-color: var(--accent)` for Spotify-green thumbs and fills.

## 20.5 Sequence Diagrams

### Sequence: Play Track from Search

```
User          SearchView       api/client       Express         SQLite       usePlayer       Audio
  |                |                |               |              |              |            |
  |--type "Perfect"->|              |               |              |              |            |
  |                |--debounce 300ms|               |              |              |            |
  |                |--search(q)--->|               |              |              |            |
  |                |                |--GET /tracks?q=Perfect-->    |              |            |
  |                |                |               |--LIKE query->|              |            |
  |                |                |               |<-rows--------|              |            |
  |                |                |<-JSON---------|              |              |            |
  |                |<-tracks[]------|               |              |              |            |
  |                |--render TrackList------------>|              |              |            |
  |--click track-->|                |               |              |              |            |
  |                |--playTrack(track, tracks)-------------------->|              |            |
  |                |                |               |              |         --loadTrack-->   |
  |                |                |               |              |              |--src=url->|
  |                |                |               |              |              |            |--fetch MP3
  |                |                |               |              |              |<-metadata-|
  |                |                |               |              |<-state update|            |
  |                |                |               |              |         PlayerBar updates
```

### Sequence: Import Local Audio

```
User              import-audio.js          Filesystem           SQLite
  |                      |                     |                  |
  |--npm run import-audio->|                   |                  |
  |                      |--validate source-->|                  |
  |                      |<-file exists--------|                  |
  |                      |--copyFileSync------>|                  |
  |                      |                     |--perfect.mp3---->|
  |                      |--UPDATE tracks------------------------>|
  |                      |<-changes=1---------------------------|
  |<-success message-----|                     |                  |
```

## 20.6 Use Case Catalog

| ID | Use Case | Actor | Precondition | Flow | Postcondition |
|----|----------|-------|--------------|------|---------------|
| UC-01 | Browse Home | User | App running | Open localhost:5173 | Home content visible |
| UC-02 | Play Track | User | Tracks visible | Click track row | Audio playing, player bar active |
| UC-03 | Search Music | User | On Search view | Type query, click result | Search results play |
| UC-04 | View Playlist | User | On Home/Library | Click playlist card | Playlist detail with tracks |
| UC-05 | View Album | User | On Home | Click album card | Album detail with tracks |
| UC-06 | Pause/Resume | User | Track playing | Click play/pause button | Playback toggles |
| UC-07 | Seek | User | Track playing | Drag seek slider | Playback position changes |
| UC-08 | Adjust Volume | User | Any time | Drag volume slider | Volume changes |
| UC-09 | Next Track | User | Queue has >1 track | Click next | Next queue track plays |
| UC-10 | Auto-Advance | System | Track ends | (automatic) | Next track starts |
| UC-11 | Import Song | Developer | MP3 file exists | Run import-audio CLI | Track plays local file |
| UC-12 | Reset Catalog | Developer | Any time | Run npm run seed | Default catalog restored |

## 20.7 Production Build Guide

While the project is designed for local development, a production build is possible:

**Frontend build:**
```bash
cd frontend && npm run build
```
Output: `frontend/dist/` — static HTML, JS, CSS bundles.

**Backend production:**
```bash
cd backend && npm start
```

**Production considerations (not implemented):**
- Serve `frontend/dist/` from Express as static files
- Set `PORT` environment variable
- Use a process manager (pm2, systemd)
- Enable HTTPS with reverse proxy (nginx)
- The Vite proxy is dev-only — production must serve API and audio from the same origin or configure CORS explicitly

**Example combined production serve (conceptual):**
```javascript
app.use(express.static(path.join(__dirname, '../../frontend/dist')));
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, '../../frontend/dist/index.html'));
});
```

## 20.8 Performance and Scalability Notes

### Current Performance Profile

| Operation | Expected Latency | Notes |
|-----------|------------------|-------|
| SQLite query | < 5ms | Single file, tiny dataset |
| API response | < 20ms | Including JSON serialization |
| Audio start (local) | < 500ms | Depends on file size |
| Audio start (remote) | 1–3s | Network dependent |
| React render | < 16ms | Small component tree |

### Scalability Limits

The current architecture handles the demo catalog (7 tracks) effortlessly. Scaling considerations if the catalog grows:

- **SQLite** handles millions of rows but search `LIKE` queries become slow without indexes. Add indexes on `tracks.title`, `artists.name`, `albums.title` for larger catalogs.
- **Static audio** works for hundreds of local files. Thousands of files may need object storage (S3) instead.
- **Single Audio element** — only one track plays at a time by design. No multi-room or sync needed.
- **No caching** — API responses are not cached. Adding HTTP cache headers or frontend SWR/React Query would help at scale.

### Memory Usage

- Backend: ~30–50 MB (Node.js + SQLite)
- Frontend: Browser-dependent, typically < 100 MB
- Audio: Streamed, not fully loaded into memory (browser handles buffering)

---

# 21. Appendices

## Appendix A: Sample API Responses

**GET /api/tracks?q=Perfect**
```json
[
  {
    "id": 7,
    "title": "Perfect",
    "duration_ms": 263000,
    "audio_url": "/audio/perfect.mp3",
    "track_number": 5,
    "album_id": 4,
    "album_title": "÷ (Divide)",
    "album_cover": "https://picsum.photos/seed/divide/400/400",
    "artist_id": 4,
    "artist_name": "Ed Sheeran"
  }
]
```

**GET /api/playlists/1** (excerpt)
```json
{
  "id": 1,
  "name": "Today's Hits",
  "description": "Fresh picks for your day",
  "cover_url": "https://picsum.photos/seed/hits/400/400",
  "tracks": [
    { "id": 7, "title": "Perfect", "artist_name": "Ed Sheeran", "...": "..." },
    { "id": 1, "title": "Downtown Drive", "...": "..." }
  ]
}
```

## Appendix B: Sample Seed Data

See Section 7.5 for the complete catalog table. Playlist track ordering:

| Playlist | Position | Track |
|----------|----------|-------|
| Today's Hits | 1 | Perfect |
| Today's Hits | 2 | Downtown Drive |
| Today's Hits | 3 | Velvet Sky |
| Today's Hits | 4 | Sunrise Sprint |
| Focus Flow | 1 | Late Night Loop |
| Focus Flow | 2 | Echo Chamber |
| Focus Flow | 3 | Coastal Wind |
| Chill Vibes | 1 | Perfect |
| Chill Vibes | 2 | Downtown Drive |
| Chill Vibes | 3 | Echo Chamber |
| Chill Vibes | 4 | Coastal Wind |

## Appendix C: Glossary

| Term | Definition |
|------|------------|
| **SPA** | Single Page Application — one HTML page, dynamic JS rendering |
| **REST** | Representational State Transfer — HTTP-based API style |
| **SQLite** | Embedded relational database in a single file |
| **WAL** | Write-Ahead Logging — SQLite journal mode for performance |
| **HMR** | Hot Module Replacement — instant dev updates without full reload |
| **Queue** | Ordered list of tracks for sequential playback |
| **Seed** | Script that populates the database with initial demo data |
| **Proxy** | Vite feature forwarding requests to another server |
| **Context** | React mechanism for sharing state across components |
| **HTML5 Audio** | Browser API for playing audio without plugins |
| **Monorepo** | Single repository containing multiple related packages |
| **Concurrently** | npm tool to run multiple commands in parallel |
| **better-sqlite3** | Synchronous Node.js binding for SQLite |
| **Vite** | Modern frontend build tool with fast dev server |

## Appendix D: Complete File Listing

```
spotify-local/
├── .gitignore
├── README.md
├── package.json
├── package-lock.json
├── docs/
│   ├── CODE.md
│   ├── DESIGN.md
│   └── PROJECT_DOCUMENTATION.md
├── backend/
│   ├── package.json
│   ├── package-lock.json
│   ├── audio/
│   │   ├── .gitkeep
│   │   └── perfect.mp3                 (gitignored, user-imported)
│   ├── data/
│   │   └── spotify.db                  (gitignored, generated)
│   └── src/
│       ├── db.js                       (55 lines)
│       ├── seed.js                     (72 lines)
│       ├── import-audio.js             (40 lines)
│       └── server.js                   (135 lines)
└── frontend/
    ├── index.html
    ├── package.json
    ├── package-lock.json
    ├── vite.config.js
    ├── public/
    └── src/
        ├── main.jsx                    (6 lines)
        ├── App.jsx                     (52 lines)
        ├── index.css                   (~460 lines)
        ├── api/
        │   └── client.js               (20 lines)
        ├── hooks/
        │   ├── PlayerContext.jsx       (16 lines)
        │   └── usePlayer.js            (128 lines)
        └── components/
            ├── CardGrid.jsx            (~30 lines)
            ├── DetailView.jsx          (37 lines)
            ├── HomeView.jsx            (33 lines)
            ├── LibraryView.jsx         (~25 lines)
            ├── PlayerBar.jsx           (~100 lines)
            ├── PlayerBarIcons.jsx      (~100 lines)
            ├── SearchView.jsx          (42 lines)
            ├── Sidebar.jsx             (~35 lines)
            └── TrackList.jsx           (46 lines)
```

**Total source files (excluding node_modules):** ~25 files  
**Total application source lines:** ~1,400 lines

## Appendix E: Environment and Ports Reference

| Variable / Setting | Default | Description |
|--------------------|---------|-------------|
| Backend PORT | 4000 | Express server port (`process.env.PORT`) |
| Frontend port | 5173 | Vite dev server (vite.config.js) |
| Database path | backend/data/spotify.db | SQLite file location |
| Audio directory | backend/audio/ | Local MP3 storage |
| API base URL (dev) | /api | Proxied to localhost:4000 |
| Audio base URL (dev) | /audio | Proxied to localhost:4000 |

**Required environment:**
- Node.js 18+ with npm
- No `.env` file required — all defaults are hardcoded for local dev

**URLs quick reference:**

| URL | Purpose |
|-----|---------|
| http://localhost:5173 | Application UI |
| http://localhost:5173/api/health | Health check (via proxy) |
| http://localhost:4000/api/health | Health check (direct) |
| http://localhost:5173/audio/perfect.mp3 | Local audio (via proxy) |
| http://localhost:4000/audio/perfect.mp3 | Local audio (direct) |

## Appendix F: Frequently Asked Questions

**Q: Do I need a Spotify account?**  
A: No. Spotify Local is completely independent of Spotify's services.

**Q: Can I use this without internet?**  
A: Partially. Local imported tracks (like Perfect) work offline. Remote SoundHelix tracks and picsum.photos cover art require internet.

**Q: How do I add more songs?**  
A: Edit `seed.js` to add track entries, place MP3 files in `backend/audio/`, and run `npm run seed`. Or use `npm run import-audio` for existing tracks.

**Q: Why does the page reset to Home on refresh?**  
A: The app uses React state for navigation, not URL routing. Refreshing resets state. Adding React Router would fix this.

**Q: Can I deploy this to production?**  
A: The codebase supports production builds (`npm run build` in frontend), but production deployment configuration is not included. See Section 20.7.

**Q: Is the Ed Sheeran song included in the repo?**  
A: No. The MP3 file is gitignored. You must import your own legally obtained copy via `npm run import-audio`.

**Q: What happens if I run seed after importing audio?**  
A: The database resets to defaults but the MP3 file remains in `backend/audio/`. Track 7 still points to `/audio/perfect.mp3`.

**Q: Why SQLite instead of MongoDB or PostgreSQL?**  
A: Zero configuration, single file, perfect for a local demo with a small catalog.

## Appendix G: Comparison with Spotify

| Feature | Spotify (Real) | Spotify Local |
|---------|----------------|---------------|
| Music catalog | 100M+ tracks | 7 demo tracks + imports |
| User accounts | Yes | No |
| Offline mode | Premium feature | Local files only |
| Playlists | Create/edit/share | View pre-seeded only |
| Search | Advanced algorithms | SQL LIKE query |
| Recommendations | ML-powered | Static "Made for you" |
| Audio quality | Up to 320kbps | Source file dependent |
| Device sync | Yes | No |
| Cost | Subscription | Free (local) |
| UI similarity | Original | Inspired clone |

Spotify Local captures the **essential UX patterns** of a streaming app — sidebar navigation, card grids, track lists, and a persistent player bar — without replicating Spotify's infrastructure, licensing, or scale.

---

# 22. Conclusion

## Project Summary

Spotify Local successfully delivers a **complete, end-to-end music streaming experience** that runs entirely on a developer's local machine. The project integrates a React frontend, Express REST API, SQLite database, and dual-source audio playback (remote demo streams and locally imported MP3 files) into a cohesive application that closely mirrors the look and feel of Spotify.

## What Was Achieved

Through the course of this project, the following capabilities were designed, implemented, and verified:

1. **Full-stack architecture** — A clean separation between presentation (React), application logic (Express), and data (SQLite) layers, communicating through well-defined HTTP APIs.

2. **Complete user experience** — Users can browse home content, search the catalog, explore playlists and albums, and control playback through a persistent, Spotify-style player bar with transport controls, seeking, and volume adjustment.

3. **Reliable audio playback** — The HTML5 Audio-based player engine supports queue management, auto-advance, and both remote and local audio sources. The import workflow allows users to play their own music — demonstrated with Ed Sheeran's *Perfect*.

4. **Developer-friendly tooling** — Single-command setup (`npm run setup`), concurrent dev servers, database seeding, and audio import CLI make the project easy to run, reset, and extend.

5. **Comprehensive documentation** — Architecture design, code guides, and this full project document provide multiple levels of detail for different audiences.

## Technical Highlights

The project demonstrates several important software engineering concepts:

- **Monorepo orchestration** with npm scripts and concurrently
- **RESTful API design** with consistent JSON responses and enriched data shapes
- **React state management** using hooks and context for global player state
- **Proxy-based development** eliminating CORS complexity
- **Embedded database** usage with schema design, seeding, and CLI mutations
- **Static asset serving** for local media files
- **UI component composition** with reusable grids, lists, and layout patterns

## Design Philosophy

Spotify Local intentionally prioritizes **clarity over complexity**. By choosing SQLite over PostgreSQL, plain CSS over UI frameworks, and view state over React Router, the project remains approachable for learning while still demonstrating production-relevant patterns. Every technology choice serves the goal of a working, understandable system that can be studied line-by-line.

## Current Limitations

The project is a **local demo**, not a production streaming service. It lacks user authentication, playlist editing, real Spotify integration, automated tests, and production deployment configuration. Several player bar icons (lyrics, queue panel, device connect, mini-player, fullscreen) are visual placeholders awaiting future implementation.

## Path Forward

The architecture provides clear extension points for growth: write APIs for playlist management, React Router for deep linking, a queue panel wired to existing player state, and a folder scanner for bulk local music import. The modular structure — separate backend and frontend packages, centralized API client, isolated player hook — ensures that new features can be added without restructuring the codebase.

## Final Statement

Spotify Local stands as a **fully functional proof of concept** that bridges the gap between tutorial-level snippets and real-world application architecture. It proves that a credible music streaming interface — complete with catalog browsing, search, playlist navigation, and audio playback including user-owned files — can be built with modern JavaScript tools in a lightweight, local-first package.

The system works end to end: from SQLite rows to REST JSON, from React components to browser audio output, from demo seeds to imported MP3 files. For developers learning full-stack web development, reviewers evaluating project completeness, or anyone seeking a foundation to build upon, Spotify Local provides a solid, documented, and extensible starting point.

---

*End of Document*

**Document:** PROJECT_DOCUMENTATION.md  
**Project:** Spotify Local v1.0.0  
**Total Sections:** 22 + 7 Appendices  
**Related Documents:** [DESIGN.md](./DESIGN.md) | [CODE.md](./CODE.md) | [README.md](../README.md)
