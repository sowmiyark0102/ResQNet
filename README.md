# 🗺️ ResQNet — Hyperlocal Disaster Response Coordinator

> A real-time platform where during floods/earthquakes, civilians mark their status on a map, AI clusters them by urgency and proximity, and auto-generates optimized rescue routes for volunteers — **without needing internet** (mesh networking via phones).

## ✨ Features

### 🗺️ Live Situation Map
- **Leaflet.js** dark map with real-time civilian markers
- Color-coded urgency: 🔴 Critical → 🟠 Moderate → 🟢 Stable
- Cluster polygon overlays (DBSCAN spatial grouping)
- Volunteer team positions + active route polylines
- One-tap **"Mark My Location"** SOS button (Geolocation API)

### 🤖 AI Clustering & Routing
- **DBSCAN spatial clustering** — groups civilians by proximity + urgency weighting
  - No fixed k required — adapts to any disaster footprint
  - Critical civilians have boosted "attraction radius"
- **Nearest-Neighbor TSP** route optimization per cluster
  - Critical stops always visited first within a route
  - Multi-team parallel routing (one route per volunteer team)
- **Claude AI** situation briefing (falls back to rule-based when offline)

### 📡 Offline-First P2P Mesh Network
- **WebRTC DataChannels** — phone-to-phone, no internet required
- **CRDT (Conflict-free Replicated Data Type)** state sync
  - Last-write-wins merge — no data loss when peers reconnect
  - Vector clock causality tracking
- **Gossip protocol** — each peer re-broadcasts to others
- **IndexedDB** local persistence — survives app restarts
- Signaling via QR code (completely offline) or minimal STUN

### 🧠 Intelligent Features
- AI flood path prediction ("Bridge St impassable in 45 min")
- Volunteer capacity-aware route assignment
- Medical skill matching (medical team → medical emergency)
- Offline Claude fallback with rule-based analysis
- Real-time victim count & urgency dashboard in header

---

## 🚀 Quick Start

### Option A — Open directly in browser
```bash
open index.html
# No build needed — pure HTML/JS + CDN Leaflet
```

### Option B — Run with local server (recommended)
```bash
npx serve .
# Open http://localhost:3000
```

### Option C — Full React PWA build
```bash
npm install
npm run dev
# Open http://localhost:5173
```

---

## 🏗️ Architecture

```
Browser / PWA
├── index.html              ← Main map UI (Leaflet + vanilla JS)
├── src/
│   ├── mesh-core.ts        ← WebRTC P2P mesh + CRDT state sync
│   ├── clusterer.ts        ← DBSCAN spatial clustering
│   ├── route-optimizer.ts  ← TSP nearest-neighbor routing
│   └── ai-analyzer.ts      ← Claude API + offline fallback
└── sw.js                   ← Service Worker (offline PWA caching)

Mesh Network (No Internet):
  Phone A ←──WebRTC──→ Phone B
     ↕                    ↕
  Phone C ←──WebRTC──→ Phone D
  (Gossip: each peer forwards to all others)

State Sync (CRDT):
  Add civilian on Phone A → broadcast to all peers
  Conflicting edits → LWW merge by timestamp
  Reconnect after outage → full state sync on reconnect
```

## 📦 File Structure
```
resqnet/
├── index.html              # Full map dashboard (no build needed)
├── src/
│   └── mesh-core.ts        # Full P2P + AI logic
├── package.json
└── README.md
```

## 🛣️ Roadmap
- [ ] Bluetooth mesh (no WiFi needed)
- [ ] Satellite SMS integration (Garmin inReach)
- [ ] Hospital capacity API integration
- [ ] Drone coordination overlay
- [ ] Multi-language SOS interface
- [ ] Government GIS data import (flood maps, building databases)
