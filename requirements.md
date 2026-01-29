# AetherAegis HRM (AAHRM) Requirements Document

## 1. Project Overview
AetherAegis is a high-fidelity heart rate monitoring and simulation ecosystem. It bridges the gap between physical BLE hardware and immersive HUDs (Unity/Web) while providing advanced simulation, replay, and "personality-driven" coaching modes.

## 2. Functional Requirements

### 2.1 Core Logic & Data (P0)
| ID | Requirement Description | Priority |
| :--- | :--- | :--- |
| **REQ-101** | Live BLE Bridge: Node.js service using `noble-winrt` to connect to GATT/BLE HRM. | P0 |
| **REQ-102** | Parametric Simulation: Software generation of Sine/Step/Random HR data. | P0 |
| **REQ-103** | Unity HUD Integration: C# dashboard for real-time metric visualization. SteamVR Overlay functionaity with admin console. | P0 |
| **REQ-104** | Interpolation Engine: Logic to smooth 1Hz hardware updates for fluid UI. | P0 |

### 2.2 Advanced Features & Logic (P1)
| ID | Requirement Description | Priority |
| :--- | :--- | :--- |
| **REQ-201** | LLM/TTS Persona Engine: Personality layer (Trainer/Unhinged/TNG) reacting to HR trends. | P1 |
| **REQ-202** | Replay Source: Module to replay recorded .fit or JSON data for dev/debug. | P2 |
| **REQ-203** | Scan & Connect: Dynamic BLE device discovery and pairing UI. | P1 |

### 2.3 R&D & Extraction (P2)
| ID | Requirement Description | Priority |
| :--- | :--- | :--- |
| **REQ-301** | Native Android Extractor: Pull Series Data directly from Google Health Connect. | P3 |
| **REQ-302** | Automated .fit Ingestion: Automated parsing of Garmin/Polar historical files. | P3 |

## 3. Backend & User Interface Requirements

### 3.1 Backend Providers
| ID | Requirement Description |
| :--- | :--- |
| **REQ-401** | AI Backend: Support for local (edge) or API-based (OpenRouter/OpenAI/Gemini) LLM. |
| **REQ-402** | Storage: Local JSON-based storage for user profiles, session logs, and .fit caches. |
| **REQ-403** | Voice Provider: Integration with system TTS or neural voices (OpenAI/Deepgram/Other). |

### 3.2 User Interface (UI)
| ID | Requirement Description |
| :--- | :--- |
| **REQ-501** | Web Dashboard (Dev): web interface for VibeSim control and websocket bridge monitoring. |
| **REQ-502** | Unity HUD (Prod): 3D overlay with customizable "Mode" themes (TNG/Grit). |
| **REQ-503** | Mobile HUD (Future): Native Android View for standalone workout tracking. |

## 4. Non-Functional Requirements (NFRs)
| ID | Attribute | Requirement |
| :--- | :--- | :--- |
| **NFR-601** | Latency | End-to-end latency from BLE event to HUD visualization < 250ms. |
| **NFR-602** | Reliability | Automatic reconnection for dropped BLE signals without service restart. |
| **NFR-603** | Scalability | Support for at least 3 simultaneous local WebSocket subscribers. |
| **NFR-604** | Portability | Core logic must support eventual migration to Android Background Service. |

## 5. Technical Stacks

### 5.1 Prototype Stack (Current)
* **Runtime:** Node.js (Windows)
* **Language:** TypeScript
* **BLE Library:** `noble-winrt`
* **UI:** React/Vite + Tailwind
* **Comm:** WebSockets (Localhost)

### 5.2 Production Stack (Final)
* **Runtime:** Android JVM / Unity Mono
* **Language:** Kotlin / C#
* **AI Engine:** Integrated LLM API via Unity WebRequests
* **HUD:** Unity URP (Universal Render Pipeline) SteamVR
* **Comm:** UDP (Low latency broadcast) / WebSocket
