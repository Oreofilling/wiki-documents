---
description: NeoLinker install and environment — unzip and run, the WebView2 Evergreen runtime, Bluetooth adapter requirements, language and theme settings, experimental macOS support, and what each channel needs from your PC.
keywords: [NeoLinker, install, WebView2, Bluetooth adapter, system requirements, Windows, macOS]
tags: [NeoLinker, Install & Setup]
---

# Install & Setup

NeoLinker is a portable app: unzip and run. On Windows the environment requirements boil down to two things — the **WebView2 runtime** and a **Bluetooth adapter**; for macOS see the "macOS (experimental)" section below.

## Download and run

1. Download the latest `NeoLinker_v*_windows_x64.zip` from [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases);
2. Unzip it to any writable folder;
3. Double-click `NeoLinker.exe`.

Runs with normal user rights, but **on Windows the softAP channel's hotspot enumeration needs administrator rights** — without elevation that channel degrades automatically (BLE / LAN are unaffected). Uninstall by deleting the folder.

## System requirements

| Item | Requirement | Notes |
| ---- | ---- | ---- |
| OS | Windows 10/11 x64 | Primary supported platform |
| OS | macOS (Apple Silicon) | Experimental: compiled and tested by CI, never run on hardware |
| Runtime | WebView2 Evergreen | Windows only; preinstalled on Windows 11 and most updated Windows 10; the embedded NE503 management page depends on it |
| Bluetooth | A BLE-capable Bluetooth adapter | Only needed for the BLE channel; softAP / LAN work without it |
| Network | WLAN adapter / Ethernet | The softAP channel uses WLAN; the LAN channel (CT-Disc) uses Ethernet or same-subnet WLAN |

## WebView2 Evergreen runtime (Windows)

The embedded browser (the NE503 management page) is based on WebView2. If the app complains about a missing runtime at startup or when opening the page:

1. Open the [WebView2 download page](https://developer.microsoft.com/microsoft-edge/webview2/);
2. Download the **Evergreen Standalone** installer (x64) and run it;
3. Restart NeoLinker.

## Bluetooth adapter (BLE channel)

- Laptops usually have Bluetooth built in — make sure the system switch is on;
- Desktop PCs can use any USB adapter that supports BLE (Bluetooth 4.0+);
- The **self-check page** at startup checks BLE / WLAN / LAN readiness — fix whichever row shows red.

## Language and theme

- The UI supports **English / 中文** and **light / dark** themes;
- Switch from the top-right corner or the **Settings page**; the choice is persisted across launches.

## macOS (experimental)

The macOS build passes CI (static analysis, tests, and a release build on a macOS runner) and shares the Windows feature set: BLE / softAP / LAN discovery, provisioning, receipts, the self-check's Wi-Fi state, the embedded device management page, and camera QR scanning. But it **has never been run on hardware** — every macOS-specific path is compile-validated only. Treat it as experimental until the checks below pass on a real Mac:

- The embedded management page loading the softAP page over plain http, and the NE503 page over its self-signed certificate;
- `mobile_scanner` camera selection and detection rate;
- How device web pages handle focus under the system web view (WKWebView).

Getting a build: run the `ci` workflow manually with **build** enabled under [NeoLinker's Actions](https://github.com/camthink-ai/NeoLinker/actions/workflows/ci.yml), then download the `neolinker-macos-arm64` artifact. The artifact is unsigned: right-click the app and choose "Open" the first time.

If the embedded management page cannot load, its failure panel can open the page in the system browser; if the camera cannot open, image upload still works.

## Build from source (optional)

NeoLinker is open source (Apache-2.0) and can be built with Flutter. The steps below are for Windows:

- Prerequisites: Windows 10/11 x64, Flutter 3.47.8 (pinned), Visual Studio 2022 Build Tools with the *Desktop development with C++* workload, and `nuget.exe` on `PATH`;
- Steps: clone the [NeoLinker repository](https://github.com/camthink-ai/NeoLinker), then in `apps/desktop_app` run `flutter pub get` → `flutter gen-l10n` → `flutter run -d windows` (or `flutter build windows --release`).

See the repository [README](https://github.com/camthink-ai/NeoLinker) for details.
