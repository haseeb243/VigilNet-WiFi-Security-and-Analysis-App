### Vigilant — Wi‑Fi Security and Network Analysis (Flutter, Android)

Vigilant is a Flutter Android app for monitoring local networks and personal connectivity. It helps you scan Wi‑Fi networks, discover devices on your subnet, test internet speed, and track daily data usage. The app uses Firebase for optional sign‑in and to save historical results. The UI is built with a clean, neumorphic design and haptic feedback.

## Features

- **Wi‑Fi Scanner**: Nearby SSID/BSSID, frequency (GHz), channel, encryption type, and RSSI (dBm). Flags weak security (OPEN/WEP) and potential rogue APs when multiple BSSIDs share the same SSID. Live signal mini‑chart for the connected AP with detailed view and actions (save scan, report rogue AP).
- **Device Discovery**: Discovers active devices on your subnet (IP/hostname) and scans curated common ports (e.g., 22, 80, 443, 445, 3389) per device.
- **Internet Speed Test**: Real‑time download/upload speeds, ping, server, connection type, external IP, and ISP with a custom speedometer and history chart. Save completed results to your profile.
- **Data Usage Tracking**: Reads Wi‑Fi and mobile usage from Android via a platform channel. Configurable limits with warnings when exceeded, plus visual breakdowns and saved history snapshots.
- **Profile & History**: Email/password auth to view all saved Wi‑Fi scans, speed tests, and usage logs in one place, with dark/light theme toggle.

## Architecture Overview

- **Presentation**: `lib/screens/*` organized into tabs — `scan`, `devices`, `speed`, `usage`, and `profile`.
- **Services**: `lib/api/*` encapsulates Wi‑Fi scanning, device discovery, Android platform channel for usage stats, and Firestore persistence.
- **Models**: `lib/models/*` define `WiFiNetwork`, `DiscoveredDevice`, and `DataUsageModel` (serialized for Firestore).
- **State/Theme**: `provider` + `ThemeManager` for theme mode; `flutter_neumorphic_plus` for UI styling.

## Tech Stack

- UI: `flutter_neumorphic_plus`, `fl_chart`, `font_awesome_flutter`
- Networking: `wifi_scan`, `network_info_plus`, `network_tools`, `connectivity_plus`
- Device/OS: `permission_handler`, `android_intent_plus`, `path_provider`, `vibration`
- Speed Test: `speed_checker_plugin`
- State/Utils: `provider`, `intl`, `async`
- Backend: `firebase_core`, `firebase_auth`, `cloud_firestore`

## Permissions (Android)

- Location (Wi‑Fi scanning)
- Usage Access (data usage tracking)
- Network access (device discovery, speed tests)

## Getting Started

1. Clone the repository:
   ```
   git clone https://github.com/haseeb243/VigilNet-WiFi-Security-and-Analysis-App.git
   cd VigilNet-WiFi-Security-and-Analysis-App
   ```
2. Install dependencies:
   ```
   flutter pub get
   ```
3. Firebase setup (do not commit secrets):
   - Add `android/app/google-services.json`
   - Add `lib/firebase_options.dart`
   - Follow the official Firebase guide for Flutter: [Firebase setup docs](https://firebase.flutter.dev/docs/overview/)
4. Run the app:
   ```
   flutter run
   ```

## Data Model (Firestore)

- `users/{uid}/scanHistory`: Wi‑Fi scan entries (`ssid`, `bssid`, `frequency`, `signalStrength`, `security`, `channel`, `scannedAt`)
- `users/{uid}/speedTestHistory`: Speed test results (`downloadSpeedMbps`, `uploadSpeedMbps`, `pingMs`, `server`, `connectionType`, `ipAddress`, `isp`, `timestamp`)
- `users/{uid}/usageHistory`: Data usage snapshots (`wifi_mb`, `mobile_mb`, `date`)
- `rogueAPReports`: Reported suspected APs (Wi‑Fi scan payload)

## Folder Highlights

- `lib/main.dart`: App init (Firebase), theme/provider setup, and tabbed navigation.
- `lib/screens/*`: Feature tabs and flows (scan, devices, speed, usage, profile/auth).
- `lib/api/*`: Wi‑Fi scanning, device discovery, Android platform channel, Firestore I/O.
- `lib/models/*`: Data models persisted to Firestore.
- `lib/utils/*`: Theme manager and usage permission helper.

## Privacy & Security

- Keep `google-services.json`, `firebase_options.dart`, keystores, and any `.env` files out of source control.
- Network scans and usage data are stored only for the current (anonymous or signed‑in) user.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you’d like to change.

## License

[MIT](LICENSE)
