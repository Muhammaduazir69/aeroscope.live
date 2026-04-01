# AeroScope — Real-Time ADS-B Flight Surveillance Platform

![AeroScope Dashboard](images/aeroscope-dashboard.png)

**Live:** [aeroscope.live](https://aeroscope.live)

AeroScope is an open-source, intelligence-grade ADS-B surveillance platform built for security researchers, aviation analysts, and defense applications. It goes beyond passive flight tracking — it scores threats, detects drones, identifies spoofing, correlates air-maritime activity, and exports research-grade datasets.

---

## What Makes AeroScope Different

| Feature | AeroScope | Flightradar24 | FlightAware | ADS-B Exchange | Planefinder | RadarBox |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Open source | **Yes** | No | No | No | No | No |
| Threat scoring (0-100) | **Yes** | No | No | No | No | No |
| Drone / UAV detection | **Yes** | No | Limited | No | No | No |
| Flight pattern recognition | **Yes** | No | No | No | No | No |
| ADS-B spoofing detection | **Yes** | No | No | No | No | No |
| Geofencing with real-time alerts | **Yes** | Limited | Limited | No | Limited | Limited |
| Maritime AIS fusion | **Yes** | No | No | No | No | No |
| Multi-observer mesh network | **Yes** | No | No | No | No | No |
| Cross-domain correlation | **Yes** | No | No | No | No | No |
| Kalman filtering + prediction | **Yes** | No | No | No | No | No |
| Research export (CSV/JSON/GeoJSON/ARFF) | **Yes** | Limited | Limited | Limited | No | Limited |
| Zero-trust capability API | **Yes** | No | No | No | No | No |
| WebSocket real-time streaming | **Yes** | No | Enterprise only | No | No | No |
| Free & self-hostable | **Yes** | Freemium | Freemium | Freemium | Freemium | Freemium |

Every competing platform is a closed, centralized commercial service. AeroScope is the only open-source option with intelligence-layer analysis.

---

## Core Capabilities

### Surveillance & Detection
- **Threat Scoring** — Automated 0-100 assessment using 8+ weighted factors: proximity, altitude, speed anomaly, squawk codes, pattern behavior, transponder integrity, military flags, and historical deviation
- **Drone Detection** — Behavioral heuristic engine identifies UAV candidates by speed/altitude profile, flight pattern, category codes, and Remote ID absence. Confidence-scored as LOW/MEDIUM/HIGH
- **Pattern Recognition** — Detects orbit, racetrack, loiter, spiral, figure-eight, and formation patterns with military significance classification
- **Signal Integrity** — NIC/NAC/SIL/SDA transponder quality scoring, ghost track identification, ADS-B spoofing probability, and network health monitoring

### Cross-Domain Intelligence
- **AIS Maritime Fusion** — AISStream.io WebSocket integration. Correlates aircraft with vessels: orbit-over-vessel detection, parallel movement analysis, dwell time monitoring, vessel classification
- **Multi-Observer Mesh** — AES-256-GCM encrypted peer groups with differential privacy (Laplacian noise, epsilon=1.0). Threat bearing triangulation, trust scoring, coverage gap analysis
- **Cross-Domain Correlation** — Detects coordinated multi-domain operations: aircraft orbiting vessels, convoy escort patterns, area saturation by air + maritime assets

### Analysis & Prediction
- **Extended Kalman Filter** — 4-state tracking with maneuver detection (NIS > 6), 30/60/120s position prediction with uncertainty ellipses
- **Airspace Complexity** — Real-time 0-100 scoring with ATC load recommendations
- **Threat Projection** — Forward-looking risk cones at 5/15/30-minute horizons
- **Route Deviation** — Flags significant departures from expected flight routes

### Data & Export
- **60+ REST API endpoints** with WebSocket streaming
- **Research-grade export** — 200+ field CSV, JSON, GeoJSON, ARFF (Weka) formats
- **Zero-trust API** — Capability-based JWT tokens with precision levels (full/reduced/zone), token revocation, audit logging
- **Historical replay** — Query past airspace state by time window with full enrichment

---

## Architecture

```
ADS-B Sources (adsb.fi, adsb.lol, airplanes.live, satellite)
        |
        v
   27-Stage Enrichment Pipeline
   SDR Merge > Drone Detect > Kalman > Integrity > Threat Score >
   Tracks > Patterns > RF Analysis > Complexity > Predictions >
   Incidents > Intel > Maritime Fusion > Cross-Domain > Mesh
        |
   -----+------------------+------------------+
   |                       |                   |
   v                       v                   v
WebSocket              REST API             SQLite
(real-time)           (60+ routes)        (persistence)
   |                       |
   v                       v
        React SPA + Leaflet Map
        4 themes - PPI radar - aircraft table
```

**Stack:** Node.js, Express, WebSocket, SQLite (better-sqlite3), React, Leaflet, Recharts, deck.gl

---

## Quick Start

```bash
git clone https://github.com/muhammaduazir69/aeroscope.git
cd aeroscope
npm install
cp .env.example .env
npm start
```

---

## API

Auth: `POST /api/auth/login` with `{"password": "your_access_code"}`

| Category | Endpoints | Description |
|---|---|---|
| Aircraft | `/api/aircraft`, `/api/aircraft/:icao` | Live + individual aircraft |
| Threats | `/api/threats`, `/api/threats/projection` | Threat assessments and projection |
| Patterns | `/api/patterns` | Detected flight patterns |
| Drones | `/api/drones` | UAV candidates with confidence |
| Geofences | `/api/geofences`, `/api/geofences/events` | Virtual boundary management |
| Maritime | `/api/maritime` | AIS vessel data and correlations |
| Mesh | `/api/mesh/groups` | Multi-observer mesh network |
| Export | `/api/dataset/export?format=csv` | Research dataset (CSV/JSON/GeoJSON/ARFF) |

Docs: [aeroscope.live/api-docs](https://aeroscope.live/api-docs)

---

## License

MIT
