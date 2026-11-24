---
description: Discovering devices with NeoLinker — the three channels (BLE advertisement scan, softAP SSID scan, LAN CT-Disc), model and channel filters, QR-code entry, and the embedded NE503 management console.
keywords: [NeoLinker, device discovery, BLE scan, softAP, CT-Disc, QR entry, RSSI]
tags: [NeoLinker, Device Discovery]
---

# Discover Devices

The scan page runs **three discovery channels** at once; which one a device shows up on depends on its model and current state. The unified list shows model, channel, signal strength (RSSI), and last-seen time, with pagination and filters.

| Channel | Models | Prerequisite | Best for |
| ------ | ------ | ------ | ------ |
| **BLE advertisement scan** | NE101 (NE301 planned) | Bluetooth adapter available | Production-line / field batch provisioning — devices need no network |
| **softAP SSID scan** | NE101, NE301 | WLAN adapter in the PC | Single-device setup; direct link to the device hotspot |
| **LAN discovery (CT-Disc)** | NE503 | PC and device on the same subnet | Management entry for networked devices |

## BLE advertisement scan

A device in configuration mode advertises itself over BLE (model code, address suffix, and a provisionable flag). NeoLinker scans continuously and aggregates:

- **Signal strength (RSSI)** helps you judge distance — closer is more stable;
- The **BUSY / provisionable flags** from the advertisement are reflected in the list state (a device being configured elsewhere shows busy);
- Devices fade out after going offline, keeping their **last-seen** time.

## softAP SSID scan

In configuration mode a device opens a hotspot with an SSID convention of `CT-AP-<address suffix>`. NeoLinker scans the WLAN for such SSIDs and lists them. After picking a device, follow the app's guidance to switch the PC's Wi-Fi to the device hotspot; the provisioning session then runs with the same semantics as BLE (CT-Prov over HTTP).

Default softAP addresses by model:

| Model | Default softAP address |
| ------ | ------ |
| NE101 | `192.168.1.1` |
| NE301 | `192.168.10.10` |
| NE503 | `10.0.0.1` (normally used via the LAN channel) |

## LAN discovery (CT-Disc)

Once an NE503 is on the network, it multicasts `ct-announce` (serial number, IP, port, capabilities) every few seconds. As long as the PC is on the **same subnet**, the LAN channel lists it. From there you can start a provisioning session or open the device's management console inside the app.

## Filters and QR entry

- **Model / channel filters**: narrow the list from the top of the page — on a production line, show only the target model;
- **QR entry**: open the QR entry point and scan the code shipped with the device using the PC camera, or upload a photo of the code, to jump straight to that device.

<img src="/img/neolinker/placeholders/scan-en.png" alt="Scan page — filters + device list (placeholder — to be replaced)" />

## Embedded NE503 console

For an NE503 discovered over LAN, the list offers **Open console**: the device's management page opens inside the app (WebView2) — no browser, no typing IP addresses.

---

Next: pick a device and continue with [Provision a Device](./3-provision-device.md).
