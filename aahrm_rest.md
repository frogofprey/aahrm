Create a separate Node.js/TypeScript application that:

## Purpose
- Connects to a BLE Heart Rate Monitor (HRM) and exposes heart rate data via a REST API (and optionally a minimal WebSocket for compatibility).

## Core functionality
1. **BLE HRM**
   - Scan for devices advertising Heart Rate Service (UUID 180d); report name and device ID (address).
   - Connect to a specific BLE HRM by device ID (12 hex chars, optional colons); discover HR Measurement characteristic (2a37); subscribe to notifications; decode 8- and 16-bit HR per BLE spec.
   - On disconnect, clean up and mark state as disconnected.

2. **REST API** (primary)
   - **GET /api/hr/current** — JSON: { heartRate, deviceId, timestamp, connected: true|false }. 200 with heartRate null when disconnected.
   - **GET /api/hr/history?seconds=1–300&interval=1** — Past window of HR; max 60 points; downsample if needed. Response: { windowSeconds, requestedIntervalSeconds, points: [ { time, heartRate, timestamp } ] }. 200 with points: [] when no data.
   - Maintain an in-memory ring buffer of the last 5 minutes of HR samples (at ~1 Hz) and feed it from each HR notification; use this for /api/hr/current and /api/hr/history.

## Dashboard / UI
Provide a simple web UI (Express on a configurable port, e.g. 3000) that allows the user to:

1. **Scan** — Button to start a BLE HRM discovery scan (e.g. 60 s or configurable); show list of found devices (name, ID). Allow stopping scan early.
2. **Connect** — Select a device from the list (or enter device ID) and connect. Show connection status (connecting / connected / disconnected). On disconnect, show a message and optionally allow reconnect.
3. **Configuration** — Minimal config: e.g. REST/HTTP port, WS port (if WS is included), scan duration, optional filter on/off. Persist in a simple config file or env if desired.
4. **Live feedback** — Display current HR and timestamp when connected; optional small log or “last N values” so the user sees that data is flowing.
5. **State** — Show which device is connected (or “None”); show “Scanning…” while scan is in progress.

No mode toggle (Live/Sim/Replay)—there is only “live BLE.” No profile dropdowns, no replay profiles, no fault injection unless you want a minimal subset (e.g. stall only) for testing.

## Tech stack
- Node.js, TypeScript, Express. BLE: use the same stack as vibesim (e.g. noble + noble-winrt on Windows) or equivalent.
- Single entry point: start HTTP server (dashboard + REST API); start BLE when user clicks Connect after a scan.

## Deliverables
- Package structure (e.g. src/, public/), package.json with scripts (build, start).
- BLE scan + connect + HR subscription logic (can be adapted from vibesim’s ble-controller).
- REST routes for /api/hr/current and /api/hr/history with the in-memory buffer.
- Simple HTML/JS dashboard for scan, device list, connect, disconnect, current HR display, and minimal config.
- README with: how to run, how to configure device, example REST calls.
