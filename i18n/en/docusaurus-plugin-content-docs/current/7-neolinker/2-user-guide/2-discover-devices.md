---
description: Discovering devices with NeoLinker — the three channels (BLE advertisement scan, softAP SSID scan, LAN CT-Disc), model and channel filters, QR-code entry, how each model enters configuration mode, and the embedded NE503 management page.
keywords: [NeoLinker, device discovery, BLE scan, softAP, CT-Disc, QR entry, RSSI, EK410]
tags: [NeoLinker, Device Discovery]
---

# Discover Devices

The scan page runs **three discovery channels** at once; which one a device shows up on depends on its model and current state. The unified list shows model, channel, signal strength (RSSI), and last-seen time, with pagination and filters.

| Channel | Models | Prerequisite | Best for |
| ------ | ------ | ------ | ------ |
| **BLE advertisement scan** | NE101, EK410 (NE301 planned) | Bluetooth adapter available | Production-line / field batch provisioning — devices need no network |
| **softAP SSID scan** | NE101, NE301, EK410 | WLAN adapter in the PC | Single-device setup; direct link to the device hotspot |
| **LAN discovery (CT-Disc)** | NE503 | PC and device on the same subnet | Management entry for networked devices |

Both EK410 channels are adapted, but not yet verified with NeoLinker on real hardware.

## BLE advertisement scan

A device in configuration mode advertises itself over BLE (model code, address suffix, and a provisionable flag). NeoLinker scans continuously and aggregates:

- **Signal strength (RSSI)** helps you judge distance — closer is more stable;
- The **BUSY / provisionable flags** from the advertisement are reflected in the list state (a device being configured elsewhere shows busy);
- Devices fade out after going offline, keeping their **last-seen** time;
- Mind the **configuration window** (NE101): a device stays configurable for about 60 s after power-on (LED flashing blue) and auto-sleeps after ~1 minute idle — press the config button to wake it and retry;
- EK410 works differently: hold the **PAGE** key for about 2.5 s; the status LED flashes cyan and the advertised name is `EK410-XXXXXX` (hyphen). The window closes by itself after about 2 minutes idle or about 15 minutes at most. SoftAP and BLE windows are mutually exclusive — while the hotspot is on, BLE does not show up. See the [EK410 docs](/docs/epaper-ek410-series/ek410-quick-start).

## softAP SSID scan

In configuration mode a device opens a hotspot: the target SSID convention is `CT-AP-<address suffix>`, while current NE101 firmware still uses the legacy `<MODEL>_<MAC suffix>` (e.g. `NE101-P_BBC6DC`) and EK410 uses the same legacy form (`EK410_<MAC suffix>`) — recognize both prefixes. NeoLinker scans the WLAN for such SSIDs and lists them. After picking a device, **the app joins the device hotspot itself** (your current Wi-Fi drops meanwhile and is restored afterwards; the EK410 hotspot is WPA2-protected and the app supplies the passphrase), and the provisioning session then runs with the same semantics as BLE (CT-Prov over HTTP).

Default softAP addresses by model:

| Model | Default softAP address |
| ------ | ------ |
| NE101 | `192.168.1.1` |
| NE301 | `192.168.10.10` |
| NE503 | `10.0.0.1` (protocol-reserved; NeoLinker does not support NE503 over softAP) |
| EK410 | `192.168.4.1` (hotspot passphrase `12345678`, shown on the device's setup screen) |

## LAN discovery (CT-Disc)

Once an NE503 is on the network, it multicasts `ct-announce` (serial number, IP, port, capabilities) every few seconds. As long as the PC is on the **same subnet**, the LAN channel lists it. From there you can open the device's management page inside the app (no provisioning session for NE503 yet).

## Filters and QR entry

- **Model / channel filters**: narrow the list from the top of the page — on a production line, show only the target model;
- **QR entry**: open the QR entry point and scan the code shipped with the device using the PC camera, or upload a photo of the code, to jump straight to that device.

<img src="/img/neolinker/placeholders/scan-en.png" alt="Scan page — filters + device list (placeholder — to be replaced)" />

## Embedded NE503 management page

For an NE503 discovered over LAN, the list offers **Open management page**: it opens inside the app — no browser, no typing IP addresses.

---

Next: pick a device and continue with [Provision a Device](./3-provision-device.md).
