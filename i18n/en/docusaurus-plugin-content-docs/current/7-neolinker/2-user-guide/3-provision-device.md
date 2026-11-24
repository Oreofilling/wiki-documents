---
description: The NeoLinker provisioning session in detail — connect and pairing, capability readout, the seven config-section forms (Wi-Fi / cellular / HaLow / Ethernet / comm / MQTT / webhook), payload preview, delivery semantics and error codes, and link-lost recovery.
keywords: [NeoLinker, provisioning session, CT-Prov, config sections, MQTT, webhook, Wi-Fi config, error codes]
tags: [NeoLinker, Device Provisioning]
---

# Provision a Device

Selecting a device in the list starts a **phase-driven provisioning session**:

```mermaid
flowchart LR
    A["Connect"] --> B["Read capabilities"] --> C["Configure per module"] --> D["Deliver"] --> E["Result receipt"]
```

One screen per phase. If the link drops unexpectedly, the app detects it and **reconnects in place** — the session resumes where it left off, and late duplicate results are dropped idempotently.

## 1. Connect and pair

- **BLE channel**: the link is encrypted. Confirm the Windows pairing prompt when it appears — the bond it creates is the basis of link-layer encryption;
- **softAP channel**: follow the app's guidance to switch the PC's Wi-Fi to the device hotspot (`CT-AP-<address suffix>`);
- **LAN channel (NE503)**: same subnet, direct connection.

Once connected, the app first reads **device info** (model, serial number, …), then the **capability report**.

## 2. Capability readout

The device reports which config modules it supports, with the fields and constraints of each. The config forms are generated entirely from this report — **modules the device does not support never appear, and supported modules come prefilled with current values**, so you only change what you want to change.

## 3. Config-section forms

CT-Prov/1 defines seven config sections. Which ones appear depends on the device model and its comm modules:

| Section | Purpose | Typical models |
| ------ | ------ | ------ |
| `wifi` | Wi-Fi access: SSID, password (can trigger a device-side scan to pick a network) | NE101 / NE301 |
| `cellular` | Cellular (CAT1) dial-up: APN, etc. | NE101 / NE301 |
| `halow` | Wi-Fi HaLow low-power long-range access | module-dependent |
| `ethernet` | Ethernet port configuration | model-dependent |
| `comm` | Network selector: which uplink to prefer | NE101 / NE301 |
| `mqtt` | MQTT service: broker address, port, topics, etc. | NE101 / NE301 / NE503 |
| `webhook` | Webhook push: URL, events, etc. | NE101 / NE301 |

Common form behavior:

- **Password fields** have a show/hide toggle — avoid typos on the production line;
- **Payload preview** expands to show the exact JSON that will be delivered;
- Cancelling keeps the **draft**, so you can come back and continue.

<!-- TODO(screenshot): config page — multi-section form + payload preview -->

## 4. Deliver

Click **Deliver** and the app writes the selected sections to the device, module by module. Delivery semantics worth knowing:

- **Whole-section overwrite** — each section is overwritten from this session's form; it is idempotent: delivering the same content twice has the same result;
- **Success means persisted** — a success response means the configuration is in non-volatile storage and survives reboot;
- **No connectivity verification** — the tool does not confirm the device can actually reach the Wi-Fi / broker; runtime outcomes are surfaced by device logs, the device's local page, or the platform side.

### Common error codes

| Code | Meaning | What to do |
| ------ | ------ | ------ |
| `INVALID_ARGUMENT` | Invalid argument type / range / combination | Fix per the form's validation hints and retry |
| `SECTION_NOT_SUPPORTED` | Section unsupported or not configurable | The model has no such module — drop the section and deliver again |
| `FIELD_NOT_SUPPORTED` | Request contains a field outside the capability report | Refresh capabilities and use the new form |
| `BUSY` | Device serving another connection or scan | Wait and retry; make sure no other host is configuring it |
| `STORAGE_FAILED` | Persisting the configuration failed | Retry; if it persists, check device storage health |
| `INVALID_MESSAGE` / `UNSUPPORTED_VERSION` / `UNSUPPORTED_OPERATION` / `INTERNAL_ERROR` | Message / version / operation mismatch, or internal error | Upgrade device firmware and the app, then retry |

## 5. Link lost and reconnect

If the link drops mid-session (device sleep, distance, hotspot switching):

- The app marks **link lost** and attempts an **in-place reconnect** without discarding the filled form;
- After reconnecting, the session resumes from where it stopped;
- Results arriving after the session has ended are dropped idempotently — receipts stay clean.

---

On success the app moves to the result page — see [Receipts & History](./4-receipts-history.md).
