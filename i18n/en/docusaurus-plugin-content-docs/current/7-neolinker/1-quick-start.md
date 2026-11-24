---
description: "Get NeoLinker running in 5 minutes: download & unzip → self-check → scan → connect → configure Wi-Fi → deliver and check the receipt. NE101 over BLE as the main path, with checkpoints and troubleshooting tips."
keywords: [NeoLinker, quick start, 5 minutes, BLE provisioning, Wi-Fi config, NE101, device discovery]
tags: [NeoLinker, Quick Start]
---

# 5-Minute Quick Start

Take one NE101 from configuration mode to a finished receipt over the **BLE channel**. Every step has a ✓ checkpoint; if you get stuck, check the tip or [Troubleshooting](./2-user-guide/5-troubleshooting.md).

**Prerequisites**:

- A Windows 10/11 x64 PC with a Bluetooth adapter (required for BLE)
- One NE101 camera in configuration mode (see the material shipped with the device)
- A 2.4 GHz Wi-Fi router (the camera and the PC do not need to be on the same network)

---

## 1. Download and run

Download the latest `NeoLinker_v*_windows_x64.zip` from [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases), unzip it anywhere, and double-click `NeoLinker.exe`. No installer.

<img src="/img/neolinker/placeholders/selfcheck-en.png" alt="Self-check page (placeholder — to be replaced)" />

✓ **Checkpoint**: the app opens in English or 中文 (switch language and light/dark theme in the top-right corner).

> Tip: if the app or the NE503 console complains about a missing WebView2, install the [WebView2 Evergreen runtime](https://developer.microsoft.com/microsoft-edge/webview2/) and retry.

## 2. Self-check

The app opens on the **self-check page**, which checks each transport — BLE adapter / WLAN / LAN. Continue once BLE shows ready.

✓ **Checkpoint**: the BLE row shows green / ready.

> Troubleshoot: if BLE is unavailable, make sure system Bluetooth is on and the adapter driver is healthy; on a desktop PC, use a USB Bluetooth adapter. Details in [Install & Setup](./2-user-guide/1-install-setup.md).

## 3. Scan for devices

On the **scan page** the app scans all three channels (BLE advertisements / softAP SSIDs / LAN CT-Disc). Once the NE101 is in configuration mode it appears in the list within seconds, with model, channel, signal strength, and last-seen time.

- Filter by **model / channel**; the list is paginated;
- Or use **QR entry**: scan the code shipped with the device with your camera, or upload a photo of it.

<img src="/img/neolinker/placeholders/scan-en.png" alt="Scan page with device list and filters (placeholder — to be replaced)" />

✓ **Checkpoint**: your NE101 shows up (BLE channel) with a reasonable signal.

> Troubleshoot: if nothing appears, confirm the device is really in configuration mode and not too far away; see [Discover Devices](./2-user-guide/2-discover-devices.md).

## 4. Connect

Select the device in the list and click **Connect**. The BLE channel uses an encrypted link — if Windows shows a Bluetooth pairing prompt, confirm it. After connecting, the app reads device info and capabilities (which config modules it supports).

✓ **Checkpoint**: the connect page shows device info (model / serial number, …) and moves on to capability readout.

> Troubleshoot: on pairing failure or timeout, go back to the scan page and retry; repeated failures → [Troubleshooting](./2-user-guide/5-troubleshooting.md).

## 5. Configure Wi-Fi

On the config page, tick the **Wi-Fi** section:

1. Pick the target network — let the device scan nearby Wi-Fi and choose, or type the SSID manually;
2. Enter the password (the field has a show/hide toggle for verification);
3. Expand the **payload preview** below the form to check the JSON that will be delivered.

Sections the device does not support simply do not appear — forms are generated entirely from the device's capability report.

<img src="/img/neolinker/placeholders/config-en.png" alt="Config page Wi-Fi form + payload preview (placeholder — to be replaced)" />

✓ **Checkpoint**: the Wi-Fi form is complete with no red validation errors.

## 6. Deliver and check the receipt

Click **Deliver**. The device writes and persists the configuration; on success the app moves to the **result page** with the session receipt — per-section applied-at times and stored counts.

✓ **Checkpoint**: the result page shows a successful delivery whose receipt matches what you entered.

> Note: a successful delivery means the configuration is persisted on the device. The device then connects to Wi-Fi on its own; the runtime outcome is surfaced by device logs or the platform side, outside this tool's feedback scope.

## 7. Wrap up

- Click **Configure next device** to return to the scan page (production-line loop);
- Or open the **History** page to browse / export past sessions;
- NE503 users: after LAN discovery, the device's management console opens inside the app (WebView2).

---

## Next steps

- [Install & Setup](./2-user-guide/1-install-setup.md) — WebView2, Bluetooth adapter, and other runtime details
- [Discover Devices](./2-user-guide/2-discover-devices.md) — three channels, filters, and QR entry
- [Provision a Device](./2-user-guide/3-provision-device.md) — the seven config sections and delivery semantics
- [Receipts & History](./2-user-guide/4-receipts-history.md) — receipt export and the production-line loop
