# DROPGuard — Examiner Demonstration Guide

## 🎬 18-Step Step-by-Step Examiner Walkthrough

Follow these steps to execute a complete, defensible capstone demonstration for examiners and evaluators:

---

### Step 1: Launch DROPGuard
Open the **DROPGuard** application on an Android device or Android Emulator.

### Step 2: Onboarding & Initial Setup
Complete the initial setup wizard by setting:
- **Security PIN**: `1234`
- **Trusted Contact**: `Emergency Contact` / `+15550199`
- **Drop Sensitivity**: `MEDIUM`
- Save and activate protection.

### Step 3: Main Dashboard Overview
Observe the central dashboard displaying:
- **Device Status**: `🟢 PROTECTED`
- **Travel Mode**: `OFF`
- **Drop Confidence**: `0%`
- **Encrypted Backup Count**: `2 Files`

### Step 4: Enable Travel Mode
Toggle the **Travel Protection Mode** switch to **ON**. Note the status updates to `🟢 ACTIVE`.

### Step 5: Open Sensor Test & Demo Mode
Navigate to **Sensor Test & Demo Mode** tile.

### Step 6: Inspect Real-Time Sensor Telemetry
Observe live streaming telemetry:
- **Accelerometer**: X, Y, Z coordinates and vector magnitude $A = \sqrt{X^2+Y^2+Z^2}$
- **Gyroscope**: Angular rotation speed (rad/s)
- **Engine Audit**: Individual breakdown of impact, orientation, rotation, and inactivity scores.

### Step 7: Execute Controlled Examiner Drop Test
Click the red button: **`[ START CONTROLLED DROP TEST ]`**.

### Step 8: View Drop Detection Engine Analysis
Observe real-time signal processing output:
- **Impact Spike**: `31.8 m/s²`
- **Angular Velocity**: `5.1 rad/s`
- **Drop Confidence**: `89%`
- **Status**: `Probable Drop Detected (89%)`

### Step 9: Interactive Drop Verification Screen
The screen transitions to the 60-second warning dialog:
```text
⚠️ POSSIBLE PHONE DROP DETECTED

Confidence: 89%
Auto-Lock Timer: 60s
Time: 4:42 PM
```

### Step 10: Confirm Device Loss
Click **`[ MARK AS LOST IMMEDIATELY ]`** (or let the 60s timer expire).

### Step 11: Last Known Location Capture
The app captures Fused Location Provider telemetry (coordinates `37.774929, -122.419416`, accuracy `12m`, battery `85%`, Google Maps URL).

### Step 12: Trusted Contact Alert Dispatch
The system formats and dispatches a high-priority system alert notification and emergency SMS payload to `+15550199`.

### Step 13: Enforce Restricted Lost Mode
The device transitions to **`🔒 LOST MODE ACTIVE`**:
- Emergency finder message displayed: *"ALERT: Potential phone drop or loss detected."*
- Incoming calls & emergency dialer accessible.
- Private files, settings, and recovery controls restricted.

### Step 14: Demonstrate Unauthorized Access Rejection
Tap **AUTHENTICATE AS OWNER** and enter an incorrect PIN (`9999`).
Observe rejection error: *"Invalid Security PIN (3 attempts left)"*.

### Step 15: Authenticate as Owner
Enter correct PIN (`1234`) or Emergency Recovery Code (`DG-8892-3401`).
Observe transition to **DEVICE RECOVERY**.

### Step 16: Decrypt Protected Emergency Files
Under **PROTECTED EMERGENCY BACKUP**, click **`[ DECRYPT ]`** next to `Passport_ID_Scan.pdf`.
Verify successful AES-256-GCM decryption and SHA-256 integrity verification.

### Step 17: End Lost Mode
Click **`[ END LOST MODE & RESTORE NORMAL OPERATION ]`**.

### Step 18: Audit Security Timeline Log
Navigate to **Security Event Log**. Verify complete chronological Room database audit records:
- `TRAVEL_MODE_ENABLED`
- `DROP_CONFIRMED`
- `LOCATION_CAPTURED`
- `TRUSTED_CONTACT_ALERT`
- `LOST_MODE_ENABLED`
- `AUTHENTICATION_SUCCESS`
- `LOST_MODE_DISABLED`

---

## 🏆 Demonstration Summary Matrix

| Phase | System Action | Examiner Verification |
| :--- | :--- | :--- |
| **DROP** | Travel motion spike detected | Accel magnitude > 28 m/s² |
| **DETECT** | Risk engine scores multi-sensor signals | Drop Confidence = 89% |
| **LOCATE** | GPS/Fused location captured | Coordinates & Maps link recorded |
| **ALERT** | High-priority notification & SMS dispatched | Alert message delivered |
| **PROTECT** | Lost Mode active & files locked | Settings & backups restricted |
| **COMMUNICATE** | Telephony & incoming calls accessible | Phone dialer functional |
| **AUTHENTICATE** | Owner PIN / Recovery code verified | Rate-limited authentication |
| **RECOVER** | File decrypted & state restored | AES-256 integrity verified |
