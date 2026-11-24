---
description: NeoLinker troubleshooting — BLE device not found, Bluetooth pairing failures, softAP connection problems, delivery error codes, missing WebView2, and NE503 not showing up on the LAN, with symptoms, causes, and fixes.
keywords: [NeoLinker, troubleshooting, FAQ, BLE, pairing, softAP, WebView2, error codes]
tags: [NeoLinker, Troubleshooting]
---

# Troubleshooting

Work through the scenarios below. If nothing helps, open an issue in the [NeoLinker repository](https://github.com/camthink-ai/NeoLinker/issues) with the exported session history and environment info (Windows version, Bluetooth adapter, …).

## BLE scan finds nothing

**Symptom**: the BLE channel stays empty for a long time.

- Confirm the device is really in **configuration mode** (see the material shipped with the device);
- Confirm the self-check page shows **BLE ready**: system Bluetooth on, adapter driver healthy; on desktop PCs use a USB Bluetooth adapter;
- **Move closer** — broadcasts may not be received at very low RSSI;
- Make sure no other host is configuring the device (a device broadcasting its BUSY flag is hard to spot);
- Cross-check with another device to isolate whether it is the device or the PC.

## Bluetooth pairing fails / connect times out

**Symptom**: pairing fails after clicking Connect, or the connection hangs.

- Remove any existing pairing record in Windows Bluetooth settings, then reconnect from the scan page;
- The BLE link is encrypted — always confirm the pairing prompt (do not dismiss it);
- Mind distance and interference: keep away from microwaves / USB 3.0 hubs;
- For repeated failures, toggle the PC's Bluetooth off and on (airplane mode or restart the Bluetooth service) and retry.

## softAP will not connect

**Symptom**: the SSID is visible on the softAP channel, but the connection / session fails.

- Confirm the PC's Wi-Fi has switched to the device hotspot `CT-AP-<address suffix>` as the app guides;
- Some adapters hold on to the previous Wi-Fi — **disconnect other Wi-Fi networks** during provisioning;
- If you access the device manually, see the address table in [Discover Devices](./2-discover-devices.md);
- The device hotspot has a client limit: make sure nothing else is holding the link.

## Delivery fails (error codes)

**Symptom**: the result page reports an error after delivering.

Check against the error-code table in [Provision a Device](./3-provision-device.md):

- `INVALID_ARGUMENT` — fix per the form's validation hints;
- `SECTION_NOT_SUPPORTED` / `FIELD_NOT_SUPPORTED` — the model / module does not support this section or field; remove it and retry;
- `BUSY` — wait and retry; make sure no other host is configuring the same device;
- `STORAGE_FAILED` — retry; repeated occurrences point to a storage problem on the device.

## Missing WebView2

**Symptom**: a WebView2 prompt at startup or when opening the NE503 console.

Install the [WebView2 Evergreen runtime](https://developer.microsoft.com/microsoft-edge/webview2/) (x64) and restart the app. See [Install & Setup](./1-install-setup.md).

## NE503 not discovered on the LAN

**Symptom**: both the device and the PC are on the network, but the LAN channel stays empty.

- Confirm both are on the **same subnet** (CT-Disc uses UDP multicast, which does not cross subnets by default);
- Check that switches / routers forward multicast;
- Wait a few seconds — the device announces `ct-announce` periodically;
- Confirm the device firmware supports CT-Disc.

## Delivery succeeded, but the device never joins Wi-Fi

That is not a tool fault: NeoLinker only **writes and persists** the configuration; it does not verify runtime connectivity. Check:

- SSID and password (2.4 GHz vs 5 GHz);
- The device-side logs or local page for association status;
- Router-side MAC filtering / allow-lists.

## Other

- **UI language / theme**: switch from the top-right corner or Settings — see [Install & Setup](./1-install-setup.md);
- **Build-from-source** problems: see the [NeoLinker repository README](https://github.com/camthink-ai/NeoLinker) and its CI configuration.
