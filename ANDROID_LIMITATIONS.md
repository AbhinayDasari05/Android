# DROPGuard — Android Platform Limitations & Architectural Compliance

## 📱 Platform Limitations Overview

As a non-root, legitimate Android security application, DROPGuard strictly complies with official Android SDK security models and Play Store policies. The application **does NOT use exploits, hidden APIs, accessibility service abuse, or unauthorized system overrides**.

Below is an honest technical breakdown of platform capabilities and constraints across Android versions:

---

## 1. Device Lock Screen & System UI Restrictions
- **Limitation**: Standard third-party Android applications cannot completely replace the Android OS system lock screen or prevent access to native status bars/quick settings without device owner provisioning.
- **DROPGuard Implementation**: DROPGuard enforces an internal application lock layer via `LostModeScreen` that restricts access to DROPGuard settings, emergency backups, and security logs. For total device kiosk locking, Android Enterprise Device Policy Controller (DPC) / LockTask mode is required.

---

## 2. Incoming Call & Communication Handling
- **Limitation**: Android restricts third-party apps from hijacking system telephony calls or creating exclusive "calls-only" operating system modes.
- **DROPGuard Implementation**: During Lost Mode, DROPGuard displays emergency dialing controls (`tel:` Intent) for trusted contacts while permitting normal incoming system phone calls, ensuring finders can receive calls from the device owner.

---

## 3. Background Sensor & Location Permissions
- **Limitation**:
  - Android 10+ (API 29) requires explicit user consent for `ACCESS_BACKGROUND_LOCATION`.
  - Android 12+ (API 31) restricts high sensor sampling rates without `HIGH_SAMPLING_RATE_SENSORS`.
  - Android 14+ (API 34) requires explicit foreground service types (`specialUse` / `location`).
- **DROPGuard Implementation**: Travel Mode operates via a compliant Foreground Service (`SensorService`) displaying an ongoing notification. Location is captured on-demand upon drop event detection to minimize background battery drain.

---

## 4. SMS Dispatch & Telephony Restrictions
- **Limitation**: Direct background SMS dispatch via `SmsManager.sendTextMessage()` requires `SEND_SMS` permission, which is restricted under Google Play Developer policies.
- **DROPGuard Implementation**: DROPGuard attempts direct SMS dispatch when permission is granted by the user, and seamlessly falls back to pre-formatted SMS Intents (`ACTION_SENDTO`), opening the system messaging app with pre-filled coordinates.

---

## 5. OEM Battery Optimization & Background Execution
- **Limitation**: Manufacturers (Samsung, Xiaomi, Huawei, OnePlus) enforce aggressive OEM background battery optimization that may terminate long-running background services.
- **DROPGuard Implementation**: Uses `START_STICKY` foreground service lifecycle with `BootReceiver` auto-restart on device reboot. Users are instructed to exclude DROPGuard from aggressive OEM battery savers.
