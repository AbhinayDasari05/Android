# DROPGuard — Automated Testing & Test Suite Documentation

## 🧪 Test Suite Summary

DROPGuard includes automated unit tests covering all core domain logic, algorithms, encryption, authentication, and rate-limiting modules:

```text
app/src/test/java/com/dropguard/app/
├── DropRiskEngineTest.kt            # Multi-factor confidence score & threshold tests
├── AccelerationMagnitudeTest.kt     # Vector magnitude & angular velocity math tests
├── AuthenticationManagerTest.kt     # PIN hashing & recovery code verification tests
├── RateLimiterTest.kt               # Progressive lockout delay calculation tests
└── EncryptionManagerTest.kt         # AES-256-GCM encryption/decryption & SHA-256 tests
```

---

## 📋 Automated Unit Test Coverage Breakdown

### 1. `DropRiskEngineTest`
- `testHighImpactHighConfidenceDrop()`: Asserts that high magnitude (36 m/s²), freefall, and rotation yield > 85% drop confidence and `isProbableDrop = true`.
- `testNormalMotionLowConfidence()`: Asserts that everyday motion (10.5 m/s²) yields low confidence (< 40%) and `isProbableDrop = false`.
- `testSensitivityThresholdAdjustments()`: Verifies that sensitivity levels (`LOW` = 80%, `HIGH` = 55%) alter the boolean classification for identical sensor inputs.

### 2. `AccelerationMagnitudeTest`
- `testVectorMagnitudeCalculation()`: Tests 3D Pythagorean acceleration magnitude $A = \sqrt{X^2 + Y^2 + Z^2}$.
- `testEarthGravityMagnitude()`: Verifies baseline gravity magnitude (9.81 m/s²).
- `testGyroscopeAngularSpeed()`: Tests 3D angular rotation speed math.

### 3. `RateLimiterTest`
- `testLockoutDelayCalculation()`: Validates progressive delay rules (1-3 attempts = 0s, 4 = 30s, 5 = 60s, 6+ = 300s).

### 4. `EncryptionManagerTest`
- `testEncryptionAndDecryptionCycle()`: Validates AES-256-GCM symmetric encryption payload and decryption recovery.
- `testSHA256ChecksumVerification()`: Verifies 256-bit SHA-256 checksum generation and integrity matching.
- `testPBKDF2PinHashing()`: Validates salted PBKDF2/SHA-256 PIN hashing consistency and mismatch rejection.

---

## 💻 Running Tests via Command Line

```bash
# Execute Gradle unit test suite
./gradlew test
```
