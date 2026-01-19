# aahrm
# AetherAegis HRM (AAHRM)

[![Status](https://img.shields.io/badge/Status-Work--In--Progress-orange)](#)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Unity](https://img.shields.io/badge/Made%20with-Unity-black.svg?style=flat&logo=unity)](https://unity.com/)

**AetherAegis HRM** is a cross-platform biometric analysis engine designed to bridge the gap between raw physiological data and actionable AI-driven insights. 

Built in Unity, AAHRM monitors Bluetooth Low Energy (BLE) heart rate sensors and utilizes a modular backend to provide real-time, narrative status updates via advanced Text-to-Speech (TTS).

---

## ⚠️ Work In Progress (WIP)
This project is currently under active development. Current focus:
- [x] Initial Requirements Engineering
- [ ] Core BLE GATT Service Implementation (Android/Windows)
- [ ] Data Aggregation & Snapshot Logic
- [ ] OpenRouter / Deepgram Provider Modules

---

## 🚀 The Vision
The goal of AAHRM is to provide a "voice-first" workout companion that understands your effort. Instead of just hearing a number, the system analyzes your heart rate trends over time and provides context: *"You've been in Zone 4 for five minutes; your heart rate is stabilizing. Keep this pace for the final interval."*

## 🛠️ Technical Highlights
To demonstrate professional software engineering standards, this project implements:

* **Modular Strategy Pattern:** All AI (LLM) and Voice (TTS) components are decoupled via C# Interfaces, allowing seamless switching between providers like **OpenRouter**, **OpenAI**, and **Deepgram**.
* **Asynchronous Data Pipeline:** Heart rate samples are captured at user-defined intervals (e.g., 10s) and processed off the main thread to ensure high-performance UI rendering.
* **Cross-Platform Architecture:** Designed to run as both a mobile Android application and a SteamVR overlay.
* **JSON-Based Persistence:** Robust handling of user profiles, heart rate zones, and remembered hardware devices.

---

## 🏗️ Architecture at a Glance
The project follows a Service-Oriented approach:

```mermaid
graph TD
    A[BLE Heart Rate Sensor] --> B[BLE Service]
    B --> C[Data Aggregator]
    C --> D{Core Controller}
    D --> E[IAnalysisProvider / OpenRouter]
    D --> F[IVoiceProvider / Deepgram]
    E --> G[Narrative Feedback]
    F --> G
