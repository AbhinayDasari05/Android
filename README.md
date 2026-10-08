# DROPGuard — Intelligent Android Device Loss Detection, Protection and Recovery System

[![Cybersecurity Capstone](https://img.shields.io/badge/Security-Capstone%20Project-00E5FF.svg)](#)
[![Android SDK](https://img.shields.io/badge/Android%20SDK-34%20%7C%20API%2026%2B-00F5D4.svg)](#)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#)

> **An Android cybersecurity application that detects probable phone drops or loss during travel, captures and sends the device's last known location to a trusted contact, protects sensitive information, activates a restricted Lost Mode, keeps incoming communication available where Android permits, and requires authenticated recovery.**

---

## 📋 Table of Contents

- [Problem Statement & Motivation](#-problem-statement--motivation)
- [System Architecture & Core Workflow](#-system-architecture--core-workflow)
- [Key Security Features](#-key-security-features)
- [Multi-Sensor Drop Detection Pipeline](#-multi-sensor-drop-detection-pipeline)
- [Encrypted Emergency Backup System](#-encrypted-emergency-backup-system)
- [Examiner Controlled Demonstration Guide](#-examiner-controlled-demonstration-guide)
- [Android Platform Limitations](#-android-platform-limitations)
- [Build & Run Instructions](#-build--run-instructions)
- [Project Quality & Test Verification](#-project-quality--test-verification)

---

## 🎯 Problem Statement & Motivation

Smartphones store sensitive personal, financial, and authentication data. When a device is accidentally dropped or lost during travel (riding a bike, commuting, walking, moving between locations), owners face two severe risks:

1. **Immediate Physical Loss**: Inability to quickly locate where the device was dropped.
2. **Unauthorized Data Access**: Physical access by unknown finders to unencrypted files, settings, and application credentials.

**DROPGuard** solves this dual problem by combining **real-time multi-sensor drop detection**, **instant last-known GPS location dispatch to trusted contacts**, **AES-256-GCM encrypted document protection**, and a **restricted Lost Mode with authenticated recovery**.

---

## 🏗 System Architecture & Core Workflow

```text
PHONE IN NORMAL USE
        ↓
TRAVEL MODE
        ↓
SENSOR MONITORING (ACCELEROMETER + GYROSCOPE)
        ↓
ABNORMAL MOTION & FREEFALL / IMPACT DETECTION
        ↓
ORIENTATION & POST-IMPACT INACTIVITY ANALYSIS
        ↓
DROP CONFIDENCE CALCULATION (0–100%)
        ↓
PROBABLE DROP DETECTED
        ↓
USER VERIFICATION (I'M SAFE / MARK AS LOST)
        ↓
CONFIRMED LOSS
        ↓
CAPTURE LAST KNOWN LOCATION (GPS/Fused)
        ↓
SEND TRUSTED CONTACT ALERT (SMS/Notification)
        ↓
ACTIVATE RESTRICTED LOST MODE
        ↓
PROTECT SENSITIVE INFORMATION (AES-256-GCM)
        ↓
ALLOW INCOMING CALLS WHERE ANDROID PERMITS
        ↓
OWNER AUTHENTICATION (PIN / Biometrics / Recovery Code)
        ↓
SECURE RECOVERY & RESTORE NORMAL OPERATION
```

---

## ⚙️ Key Security Features

- **Initial Security Setup Wizard**: Mandatory security configuration of PIN, optional biometrics, trusted contact details, drop sensitivity, and emergency backup targets.
- **Travel Protection Mode**: Dedicated monitoring state with clear status toggles and transparent privacy indicators.
- **Multi-Sensor Event Detector**: Vector magnitude $A = \sqrt{X^2+Y^2+Z^2}$ analysis combining accelerometer freefall/impact spikes with gyroscope angular velocity and tilt orientation changes.
- **Drop Confidence Engine**: Weighted 0–100% scoring model evaluating impact magnitude (35%), orientation change (20%), rotation speed (15%), sudden deceleration (15%), and post-impact inactivity (15%).
- **Verification Window**: 60-second interactive warning screen allowing users to select **"I'M SAFE"** or **"MARK AS LOST"**.
- **Last Known Location Telemetry**: Fused Location Provider capture recording precise coordinates, GPS accuracy, timestamp, battery level, and Google Maps URL.
- **Trusted Contact Alerting**: Formatted emergency notification and direct SMS dispatch.
- **Restricted Lost Mode**: Restricts access to app settings, security logs, and emergency backups while keeping incoming calls available.
- **Progressive Rate Limiting**: Exponential authentication lockout delays preventing brute-force PIN attempts.
- **Encrypted Emergency Backup**: Android KeyStore-backed AES-256-GCM file encryption with SHA-256 integrity verification.
- **Examiner Demo Mode**: Safe controlled sensor test trigger for academic evaluation.

---

## 🛠 Tech Stack

- **Language**: Kotlin 1.9.22
- **UI Framework**: Jetpack Compose with Material 3 Dark Theme
- **Architecture**: Clean Architecture (Data, Domain, Sensors, Location, Security, UI)
- **Database**: Room Database with Flow & Coroutines
- **Security & Cryptography**: Android KeyStore, `EncryptedSharedPreferences`, AES-256-GCM, PBKDF2 with SHA-256
- **Sensors & Location**: Android SensorManager (Accelerometer & Gyroscope), Google Play Services Fused Location Provider

---

## 📱 Build & Run Instructions

### Prerequisites
- JDK 17 or JDK 21 installed
- Android Studio Hedgehog (2023.1.1) or newer
- Android SDK 34 (Build Tools 34.0.0)

### Building via Gradle
```bash
# Clone or navigate to the project directory
cd antigravity

# Execute unit tests
./gradlew test

# Assemble debug APK
./gradlew assembleDebug
```

Output APK will be generated at: `app/build/outputs/apk/debug/app-debug.apk`

---

## 📜 Documentation Directory

- [`ARCHITECTURE.md`](ARCHITECTURE.md): Technical package structure and modular component breakdown.
- [`SECURITY.md`](SECURITY.md): Cryptographic algorithms, KeyStore key management, and security design principles.
- [`THREAT_MODEL.md`](THREAT_MODEL.md): Analysis of 10 primary threats, risks, detection mechanisms, and mitigations.
- [`ANDROID_LIMITATIONS.md`](ANDROID_LIMITATIONS.md): Transparent documentation of Android platform restrictions and device management limits.
- [`DEMO_GUIDE.md`](DEMO_GUIDE.md): Step-by-step examiner presentation and demonstration walkthrough.
- [`TESTING.md`](TESTING.md): Automated unit test coverage details and manual verification procedures.
- [`PRIVACY.md`](PRIVACY.md): User consent guidelines and data retention policy.
