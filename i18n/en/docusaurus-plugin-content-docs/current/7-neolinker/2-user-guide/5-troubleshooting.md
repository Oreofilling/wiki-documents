---
description: NeoLinker troubleshooting — BLE device not found, unknown model shown, Bluetooth pairing failures, softAP connection problems, delivery error codes, missing WebView2, NE503 not showing up on the LAN, and issues on macOS, with symptoms, causes, and fixes.
keywords: [NeoLinker, troubleshooting, FAQ, BLE, pairing, softAP, WebView2, error codes, macOS, EK410]
tags: [NeoLinker, Troubleshooting]
---

# Troubleshooting

Work through the scenarios below. If nothing helps, open an issue in the [NeoLinker repository](https://github.com/camthink-ai/NeoLinker/issues) with the exported session history and environment info (OS and version, NeoLinker version, Bluetooth adapter, …).

## BLE scan finds nothing

**Symptom**: the BLE channel stays empty for a long time.

- Confirm the device is really in **configuration mode** (see the material shipped with the device);
- Confirm the self-check page shows **BLE ready**: system Bluetooth on, adapter driver healthy; on desktop PCs use a USB Bluetooth adapter;
- **Move closer** — broadcasts may not be received at very low RSSI;
- Make sure no other host is configuring the device (a device broadcasting its BUSY flag is hard to spot);
- Mind the NE101 **configuration window**: a device stays configurable for about 60 s after power-on (LED flashing blue) and auto-sleeps after ~1 minute idle — press the config button to wake it and retry;
- EK410: hold the **PAGE** key for about 2.5 s to open BLE provisioning (status LED flashing cyan); the advertised name is `EK410-XXXXXX`. SoftAP and BLE windows are mutually exclusive — while the hotspot is on, BLE does not show up;
- Cross-check with another device to isolate whether it is the device or the PC.

## The scan list shows an "unknown model"

**Symptom**: the device appears in the list, but the model column does not name a model.

- The app is too old to know this device: the released v0.3.0 supports NE101 / NE301 / NE503 only; EK410 support arrives in a later release. Update to the latest version from [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases);
- The version is shown at the bottom of the app's sidebar. Note a known issue in the released v0.3.0: the sidebar and exported receipts show v0.2.0 — a display error, fixed in later versions.

## Bluetooth pairing fails / connect times out

**Symptom**: pairing fails after clicking Connect, or the connection hangs.

- Remove any existing pairing record in Windows Bluetooth settings (on macOS, in System Settings › Bluetooth), then reconnect from the scan page;
- The BLE link is encrypted — always confirm the pairing prompt (do not dismiss it);
- Mind distance and interference: keep away from microwaves / USB 3.0 hubs;
- For repeated failures, toggle the PC's Bluetooth off and on (airplane mode or restart the Bluetooth service) and retry.

## softAP will not connect

**Symptom**: the SSID is visible on the softAP channel, but the connection / session fails.

- The app joins the device hotspot itself (current NE101 hotspots are mostly named `<MODEL>_<MAC suffix>`, e.g. `NE101-P_BBC6DC`); your current Wi-Fi drops meanwhile and is restored afterwards;
- The EK410 hotspot is WPA2-protected and the app joins it itself; to join manually, the passphrase is `12345678` (shown on the device's setup screen) and the management address is `http://192.168.4.1`;
- If joining fails, manually disconnect the PC's other Wi-Fi networks and retry;
- If you access the device manually, see the address table in [Discover Devices](./2-discover-devices.md);
- A device serves one configuration connection at a time — if another host is configuring it, the device reports BUSY; wait and retry.

## Delivery fails (error codes)

**Symptom**: the result page reports an error after delivering.

Check against the error-code table in [Provision a Device](./3-provision-device.md):

- `INVALID_ARGUMENT` — fix per the form's validation hints;
- `SECTION_NOT_SUPPORTED` / `FIELD_NOT_SUPPORTED` — the model / module does not support this section or field; remove it and retry;
- `BUSY` — wait and retry; make sure no other host is configuring the same device;
- `STORAGE_FAILED` — check the error detail first: over the EK410 softAP channel, `wifi associate failed` means the Wi-Fi association failed (most likely a wrong password); otherwise retry, and repeated occurrences point to a storage problem on the device.

## Missing WebView2 (Windows)

**Symptom**: a WebView2 prompt at startup or when opening the NE503 management page.

Install the [WebView2 Evergreen runtime](https://developer.microsoft.com/microsoft-edge/webview2/) (x64) and restart the app. See [Install & Setup](./1-install-setup.md).

## NE503 not discovered on the LAN

**Symptom**: both the device and the PC are on the network, but the LAN channel stays empty.

- Confirm both are on the **same subnet** (CT-Disc uses UDP multicast, which does not cross subnets by default);
- Check that switches / routers forward multicast;
- Wait a few seconds — the device announces `ct-announce` periodically;
- Confirm the device firmware supports CT-Disc.

## Delivery succeeded, but the device never joins Wi-Fi

On most models this is not a tool fault: NeoLinker only **writes and persists** the configuration; it does not verify runtime connectivity. Check:

- SSID and password (if the device only supports 2.4 GHz, make sure the router has that band enabled — follow the device manual);
- The device-side logs or local page for association status;
- Router-side MAC filtering / allow-lists.

The exception is the EK410 softAP channel: the device tries to join the Wi-Fi first, so when it cannot, the delivery itself fails (`STORAGE_FAILED`) — "delivered but not connected" does not occur.

## Issues on macOS

The macOS build is experimental: it has passed CI only and has never been run on hardware.

- **The app will not open, with an "unidentified developer" warning**: the build is unsigned. Find the app in Finder, right-click it, choose "Open", and confirm once more;
- **The self-check shows the softAP channel unavailable**: make sure Wi-Fi is turned on. With Wi-Fi off, the scan page shows a limited state and offers an "Open Wi-Fi settings" button;
- **The softAP channel stays empty**: on macOS 10.15 and later, Wi-Fi scans require Location Services authorization; without it this channel stays empty and reports no error. Check whether NeoLinker is allowed under System Settings › Privacy & Security › Location Services; this behaviour has not been confirmed on hardware yet — if needed, fall back to the BLE channel or QR entry;
- **Camera QR scanning unavailable**: make sure NeoLinker is allowed under System Settings › Privacy & Security › Camera; if it still fails, switch to image upload;
- **The embedded management page fails to load**: the failure panel offers a button to open the page in the system browser instead.

## Other

- **UI language / theme**: switch from the top-right corner or Settings — see [Install & Setup](./1-install-setup.md);
- **Build-from-source** problems: see the [NeoLinker repository README](https://github.com/camthink-ai/NeoLinker) and its CI configuration.
