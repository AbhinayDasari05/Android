# DROPGuard — System Architecture & Component Design

## 🏛 Package Architecture Overview

DROPGuard follows a clean, decoupled Android architecture organized into data, domain, sensors, location, security, notification, and UI layers:

```text
app/src/main/java/com/dropguard/app/
├── MainActivity.kt                      # Main Compose Navigation Activity
├── DropGuardApplication.kt              # App Application class initializing repositories
│
├── data/
│   ├── database/
│   │   ├── AppDatabase.kt               # Room Database instance
│   │   ├── SecurityEventDao.kt          # DAO for security audit log persistence
│   │   └── SecurityEventEntity.kt       # Room Entity for events
│   ├── preferences/
│   │   └── SecurityPreferences.kt       # EncryptedSharedPreferences wrapper
│   ├── repository/
│   │   └── SecurityRepository.kt        # Unified Repository pattern bridge
│   └── backup/
│       ├── BackupFile.kt                # Metadata model for encrypted backups
│       └── EncryptedBackupManager.kt    # AES-256-GCM file encryption manager
│
├── domain/
│   ├── detection/
│   │   ├── SensorData.kt                # Accelerometer & Gyroscope data classes
│   │   └── DropDetector.kt              # Signal processing & windowing pipeline
│   ├── risk/
│   │   ├── DropConfidenceResult.kt      # Confidence score data model
│   │   └── DropRiskEngine.kt            # Multi-factor 0-100% confidence scoring engine
│   ├── recovery/
│   │   └── RecoveryManager.kt           # Authenticated owner recovery controller
│   └── security/
│       ├── KeyStoreManager.kt           # Android KeyStore & PBKDF2/SHA-256 utility
│       ├── AuthenticationManager.kt     # PIN, Biometric & Recovery code verifier
│       └── RateLimiter.kt               # Progressive delay lockout manager
│
├── sensors/
│   ├── AccelerometerManager.kt          # Listener wrapper for TYPE_ACCELEROMETER
│   ├── GyroscopeManager.kt              # Listener wrapper for TYPE_GYROSCOPE
│   ├── MotionAnalyzer.kt                # Real-time sensor stream fusion pipeline
│   └── SensorService.kt                 # Foreground service for continuous Travel Mode
│
├── location/
│   ├── LocationData.kt                  # Model for GPS coordinates & metadata
│   └── DropLocationManager.kt           # Fused Location Provider client
│
├── security/
│   ├── LostModeManager.kt               # State machine for Lost Mode transition
│   └── SecurityEventLogger.kt           # Helper for logging structured Room events
│
├── notification/
│   └── AlertManager.kt                  # High-priority alert notification & SMS manager
│
└── ui/
    ├── theme/                           # Cybersecurity dark color scheme & typography
    ├── components/                      # Reusable cards, meters, status badges
    ├── navigation/                      # Navigation routes & screens
    ├── onboarding/                      # Intro onboarding walkthrough
    ├── setup/                           # Initial security wizard
    ├── dashboard/                       # Central dashboard screen
    ├── travel/                          # Travel Mode control screen
    ├── detection/                       # Sensor test & verification screens
    ├── lostmode/                        # Restricted Lost Mode screen
    ├── recovery/                        # Authenticated recovery screen
    ├── backup/                          # Encrypted emergency backup screen
    ├── contacts/                        # Trusted contact manager screen
    ├── timeline/                        # Room audit log timeline screen
    ├── privacy/                         # Privacy controls screen
    └── settings/                        # Security settings screen
```

---

## 🔄 Sensor Data Flow Pipeline

```text
[ Hardware Accelerometer ] ──┐
                             ├──> [ MotionAnalyzer ] ──> [ DropDetector ]
[ Hardware Gyroscope ] ─────┘                                    │
                                                                 ▼
                                                        [ DropRiskEngine ]
                                                                 │
                                                       Calculates Score (0-100%)
                                                                 │
                                                                 ▼
                                                        Confidence > Threshold?
                                                        /                  \
                                                      YES                   NO
                                                       │                     │
                                                       ▼                     ▼
                                            [ VerificationScreen ]    [ Normal Motion ]
                                            (60s Countdown Timer)
                                             /                 \
                                            /                   \
                                    [ I'M SAFE ]           [ MARK AS LOST / TIMEOUT ]
                                         │                           │
                                         ▼                           ▼
                                  [ Event Cancelled ]      [ Location Captured ]
                                                                     │
                                                                     ▼
                                                          [ Alert Dispatched ]
                                                                     │
                                                                     ▼
                                                         [ Lost Mode Activated ]
```
