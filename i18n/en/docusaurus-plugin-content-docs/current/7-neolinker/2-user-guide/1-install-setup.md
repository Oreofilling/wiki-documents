---
description: NeoLinker install and environment — unzip and run, the WebView2 Evergreen runtime, Bluetooth adapter requirements, language and theme settings, and what each channel needs from your PC.
keywords: [NeoLinker, install, WebView2, Bluetooth adapter, system requirements, Windows]
tags: [NeoLinker, Install & Setup]
---

# Install & Setup

NeoLinker is a portable app: unzip and run. The environment requirements boil down to two things — the **WebView2 runtime** and a **Bluetooth adapter**.

## Download and run

1. Download the latest `NeoLinker_v*_windows_x64.zip` from [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases);
2. Unzip it to any writable folder;
3. Double-click `NeoLinker.exe`.

No administrator rights, no registry entries — uninstall by deleting the folder.

## System requirements

| Item | Requirement | Notes |
| ---- | ---- | ---- |
| OS | Windows 10/11 x64 | macOS support planned |
| Runtime | WebView2 Evergreen | Preinstalled on Windows 11 and most updated Windows 10; the embedded NE503 console depends on it |
| Bluetooth | A BLE-capable Bluetooth adapter | Only needed for the BLE channel; softAP / LAN work without it |
| Network | WLAN adapter / Ethernet | The softAP channel uses WLAN; the LAN channel (CT-Disc) uses Ethernet or same-subnet WLAN |

## WebView2 Evergreen runtime

The embedded browser (the NE503 management console) is based on WebView2. If the app complains about a missing runtime at startup or when opening the console:

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

## Build from source (optional)

NeoLinker is open source (Apache-2.0) and can be built with Flutter:

- Prerequisites: Windows 10/11 x64, Flutter 3.47.8 (pinned), Visual Studio 2022 Build Tools with the *Desktop development with C++* workload, and `nuget.exe` on `PATH`;
- Steps: clone the [NeoLinker repository](https://github.com/camthink-ai/NeoLinker), then in `apps/desktop_app` run `flutter pub get` → `flutter gen-l10n` → `flutter run -d windows` (or `flutter build windows --release`).

See the repository [README](https://github.com/camthink-ai/NeoLinker) for details.
