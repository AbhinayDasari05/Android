# DROPGuard — Threat Model & Risk Analysis

## 🎯 Threat Analysis Matrix (Threats 1 – 10)

### Threat 1: Unauthorized Physical Access After Device Loss
- **Risk**: An unknown finder picks up a dropped device and attempts to browse local files and settings.
- **Detection**: Sudden drop confidence score calculation followed by user verification timeout.
- **Mitigation**: Immediate activation of Restricted Lost Mode, blocking access to DROPGuard settings and emergency file repositories.
- **Evidence**: `LostModeManager.kt` blocks protected routes and enforces `isLostModeActive` state checks.

---

### Threat 2: Attacker Attempts to Access Sensitive Local Files
- **Risk**: Attacker connects device via USB or file browser to extract confidential identity or insurance documents.
- **Detection**: File access request without valid owner session state.
- **Mitigation**: Emergency backup files are stored exclusively in AES-256-GCM encrypted format with IV payloads.
- **Evidence**: `EncryptedBackupManager.kt` requires Android KeyStore key decryption and SHA-256 integrity verification.

---

### Threat 3: Attacker Attempts to Access Emergency Backup
- **Risk**: Direct unauthorized decryption attempt on emergency backup payloads.
- **Detection**: Unauthenticated call to `decryptFile()`.
- **Mitigation**: `RecoveryManager.kt` asserts `!lostModeManager.isProtectedAccessBlocked()` before invoking decryption.
- **Evidence**: Unit test `EncryptionManagerTest.kt` verifies cryptographic failure when keys/payloads are tampered with.

---

### Threat 4: Attacker Repeatedly Guesses Owner Authentication PIN
- **Risk**: Brute-force dictionary attack on 4-digit security PIN.
- **Detection**: Sequential invalid PIN submissions in `AuthenticationManager.kt`.
- **Mitigation**: Progressive rate-limiting enforcement (30s after 4 failed attempts, 60s after 5, 300s after 6+).
- **Evidence**: `RateLimiter.kt` and `RateLimiterTest.kt` confirm delay calculation and persistent timestamp lockout.

---

### Threat 5: Attacker Attempts to Modify Security State
- **Risk**: Unauthorized disabling of Travel Mode or resetting of trusted contact info while Lost Mode is active.
- **Detection**: Navigation attempt to settings or travel configuration screens during Lost Mode.
- **Mitigation**: Screen routes are redirected to `LostModeScreen` until owner authenticates.
- **Evidence**: `MainActivity.kt` checks `isLostModeActive` on initial launch and route navigation.

---

### Threat 6: Location Data Exposure & Privacy Leak
- **Risk**: Continuous GPS tracking exposing user daily movement habits.
- **Detection**: Audit of location request triggers.
- **Mitigation**: Location is requested **exclusively on demand** when a probable drop/loss event occurs. No background tracking logs are uploaded to third parties.
- **Evidence**: `DropLocationManager.kt` is triggered only when `confirmDeviceLoss()` or `captureLastKnownLocation()` is invoked.

---

### Threat 7: False-Positive Drop Detection Lockout
- **Risk**: Device locks during normal activities (running, riding on rough roads, throwing phone onto bed).
- **Detection**: Multi-sensor fusion engine analyzing sudden stop and post-impact inactivity alongside magnitude.
- **Mitigation**: 60-second interactive verification window with prominent **"I'M SAFE"** cancellation button and configurable sensitivity levels (LOW, MEDIUM, HIGH).
- **Evidence**: `VerificationScreen.kt` and `DropRiskEngine.kt` test cases.

---

### Threat 8: Emergency Backup File Corruption or Tampering
- **Risk**: Attacker modifies encrypted file bytes on storage disk to induce application failure or inject malicious payload.
- **Detection**: SHA-256 integrity hash verification during decryption.
- **Mitigation**: Decryption computes `calculateSHA256(decryptedData)` and compares it against pre-encryption metadata. Mismatches trigger immediate exception.
- **Evidence**: `EncryptedBackupManager.kt` integrity assertion check.

---

### Threat 9: Unauthorized Recovery API / Intent Triggering
- **Risk**: Third-party malicious application calling recovery intent to bypass authentication.
- **Detection**: Internal component launch check.
- **Mitigation**: All activities, receivers, and services are marked `android:exported="false"` unless explicitly required (e.g., MainActivity launcher).
- **Evidence**: `AndroidManifest.xml` component declaration review.

---

### Threat 10: Insecure Storage of Security Credentials
- **Risk**: Plaintext storage of Security PINs, salt, or cryptographic keys in standard SharedPreferences.
- **Detection**: Inspection of storage xml files.
- **Mitigation**: Credentials stored in `EncryptedSharedPreferences` backed by AES-256-SIV key encryption and Android KeyStore master key. PIN stored only as PBKDF2/SHA-256 salted hash.
- **Evidence**: `SecurityPreferences.kt` and `KeyStoreManager.kt` implementations.
