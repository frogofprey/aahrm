# Software Requirements Specification: AetherAegis HRM (AAHRM)

## 1. Project Overview
**AetherAegis HRM (AAHRM)** is a cross-platform (Android/Windows/VR) biometric utility built in Unity. It bridges Bluetooth Low Energy (BLE) heart rate data with a decoupled AI backend to provide narrative, context-aware physiological feedback via Text-to-Speech (TTS).

### 1.1 Objective
The primary goal is to create a robust, production-grade coding portfolio piece that demonstrates:
* **Modular Software Design:** Swappable LLM/TTS backends.
* **Hardware Integration:** Real-time BLE GATT communication.
* **Data Lifecycle Management:** Aggregation of raw sensor data into actionable AI prompts.

---

## 2. System Architecture
The application utilizes a **Service-Oriented Architecture** to ensure high decoupling.

* **Core Controller:** Manages the workout state machine and timing.
* **Provider Interfaces:** `IAnalysisProvider` and `IVoiceProvider` define the contract for external services.
* **Data Sampler:** Periodically captures heart rate values and buffers them for the analyzer.
* **Persistence Manager:** Handles JSON-based storage for user settings and device UUIDs.

---

## 3. Functional Requirements (FR)

### 3.1 Sensor Management (BLE)
* **FR-1.1 Discovery:** Scan for and list devices broadcasting the Heart Rate Service (0x180D).
* **FR-1.2 Persistence:** Securely save the UUID of a preferred sensor and attempt auto-connection on application launch.
* **FR-1.3 Data Ingestion:** Subscribe to the Heart Rate Measurement characteristic (0x2A37).
* **FR-1.4 Status Monitor:** Provide a UI "Thumbnail" on the dashboard showing connection health and battery status.

### 3.2 Analysis & Logic (The "Aegis" Core)
* **FR-2.1 Default Zoning:** Automate Heart Rate Zone calculation (Z1–Z5) using the $220 - \text{Age}$ formula for the MVP.
* **FR-2.3 Buffer Logic:** * **Capture Interval:** Polling frequency for HR data (Default: 10s; Range: 1s–60s).
    * **Reporting Interval:** Frequency of AI-generated status updates (Default: 60s; Range: 30s–300s).
* **FR-2.4 Snapshot Generation:** Compile buffered data (Average HR, Max HR, Time-in-Zone) into a structured JSON payload for the LLM.

### 3.3 Backend Providers
* **FR-3.1 LLM Support:** Implement a modular wrapper for **OpenRouter** (supporting Claude/GPT-4/Llama) and OpenAI.
* **FR-3.2 Voice Support:** Implement a modular wrapper for **Deepgram Aura** and Google Cloud TTS.

### 3.4 User Interface
* **FR-4.1 Dashboard:** Landing page with a sensor status summary and "Start Workout" functionality.
* **FR-4.2 Configuration Dialog:** Persistent settings for User Age, Sampling/Reporting intervals, and API Provider selection.

---

## 4. Non-Functional Requirements (NFR)
* **NFR-4.1 Modularity:** All external services must be swappable via the **Strategy Pattern** (C# Interfaces).
* **NFR-4.2 Threading:** All network I/O and BLE operations must be asynchronous to maintain a consistent 60+ FPS Unity framerate.
* **NFR-4.3 Reliability:** Implement graceful degradation (e.g., if the LLM is unreachable, the system reverts to a basic text-to-speech zone announcement).

---

## 5. Technical Stack
* **Engine:** Unity 2022.3 LTS
* **Platform Support:** Android (APK) & Windows (SteamVR Overlay)
* **Data Handling:** C# System.Text.Json or Newtonsoft.Json
* **Communication:** Bluetooth LE (GATT) & REST API
