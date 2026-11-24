---
description: NeoLinker receipts and history — every completed session writes a plain-text per-device receipt (applied-at times, stored counts) that can be exported; past sessions are listed and browsable, and the result page loops straight to the next device.
keywords: [NeoLinker, receipts, history, export, production line, provisioning records]
tags: [NeoLinker, Receipts & History]
---

# Receipts & History

NeoLinker treats "what was configured, and when" as a first-class concern: **every completed session leaves a trace**.

## Device receipts

After a successful delivery, the result page shows the session's **plain-text receipt**, organized per device, including:

- Device identity (model / serial number, …);
- The config sections written in this session;
- Per-section applied-at times and stored counts.

<img src="/img/neolinker/placeholders/receipt-en.png" alt="Result page receipt card (placeholder — to be replaced)" />

The receipt can be **exported** directly for archiving — a plain-text format that is easy to file with work orders or keep for audit trails.

## Session history

The **History** page in the sidebar lists past sessions with details and export. On a production line, "what was configured on this unit back then" is one serial-number lookup away.

:::tip History is organized per run
The history list focuses on the current batch of provisioning; switch runs to look further back.
:::

## The production-line loop

The result page has a **Configure next device** button: one click returns to the scan page for the next session. Repeat "scan / pick up the next unit → connect → deliver → receipt" — that loop is the main workflow NeoLinker is designed around.

---

Something went wrong? See [Troubleshooting](./5-troubleshooting.md).
