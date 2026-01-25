```mermaid
graph LR
    subgraph "ESP32 DevKit V1 (30-Pin)"
        GND[GND]
        3V3[3V3]
        P4[GPIO 4]
        P2[GPIO 2 / Onboard LED]
    end

    subgraph "External Components"
        BTN((Mission Select Button))
        RES[10k Ohm Resistor]
        PWR((USB Power))
    end

    %% Wiring Connections
    3V3 --- BTN
    BTN --- P4
    P4 --- RES
    RES --- GND
    PWR --- GND
    
    %% Onboard Indicator
    P2 -.->|Blinks with Pulse| P2

    style BTN fill:#f9f,stroke:#333,stroke-width:2px
    style RES fill:#fff,stroke:#333,stroke-dasharray: 5 5
    style P2 fill:#fff2cc,stroke:#d6b656,stroke-width:2px
