```mermaid
graph TD
    %% Power Section
    USB[Micro-USB Port] -- 5V --> VReg[Onboard Regulator]
    VReg -- 3.3V --> ESP_3V3[3V3 Rail]

    subgraph ESP32_DevKit_V1["ESP32 DevKit V1 (30-Pin)"]
        direction TB
        P4[GPIO 4 / Mission Select]
        P2[GPIO 2 / Heartbeat LED]
        GND[Common Ground]
        
        subgraph Logic_Core["Internal Firmware"]
            Sim[Sim Engine] --> BLE[BLE Stack: Service 0x180D]
        end
    end

    %% Inputs & Indicators
    BTN((Push Button)) -- Signal --> P4
    ESP_3V3 -- High --> BTN
    P4 -- Logic Low --> RES[10k Ohm Resistor]
    RES -- Ground --> GND
    
    %% Output Flow
    BLE -.->|Broadcasting| HUB((AetherAegis Dashboard))
    P2 -.->|Visual Pulse| LED_INT[Onboard Blue LED]

    %% Styling
    style ESP32_DevKit_V1 fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style Logic_Core fill:#fff9c4,stroke:#fbc02d
    style BTN fill:#f8bbd0,stroke:#c2185b
    style RES fill:#cfd8dc,stroke:#455a64,stroke-dasharray: 5 5
