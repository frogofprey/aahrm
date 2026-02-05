```mermaid
stateDiagram-v2
    [*] --> IDLE
    
    IDLE --> INITIALIZATION : Signal Detected (HR > 40)
    
    INITIALIZATION --> WARMUP : Baseline Established
    
    state WARMUP {
        [*] --> Scoring_Off
        Scoring_Off --> Scoring_Off : Telemetry Logged
    }
    
    WARMUP --> MAIN_ACTIVE : HR sustained in Zone 2 OR Timer End
    
    state MAIN_ACTIVE {
        [*] --> Scoring_On
        Scoring_On --> PAUSED : Manual Pause / Signal Loss
        PAUSED --> Scoring_On : Resume
        Scoring_On --> Scoring_On : Accruing HP/Kcal
    }
    
    MAIN_ACTIVE --> COOLDOWN_BONUS : Minimum Thresholds Met (Time/HP/Kcal)
    
    state COOLDOWN_BONUS {
        [*] --> Scoring_Off_Bonus
        Scoring_Off_Bonus --> Scoring_Off_Bonus : Victory Lap Logic
    }
    
    COOLDOWN_BONUS --> RECOVERY : Manual End / Max Overage Reached
    
    RECOVERY --> [*] : HR < 97 BPM (Recovery Floor)
