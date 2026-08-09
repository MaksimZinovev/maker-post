---
shaping: true
---

# D1 Spike: iOS → Cloudflare R2 Auto-Sync Feasibility

## Context

Shape D (selected) builds the classification + organization pipeline now and treats the ingest mechanism as a spike. The core unknown: **how to auto-sync photos/videos from an iPhone to Cloudflare R2 without a manual share action.**

Andrei wants zero-friction capture: "I can just take a photo. Everything else should happen by itself" [Source 27]. The original shaping rejected auto-sync (Architecture C) because iCloud has no public API. This spike investigates whether viable alternatives exist for MVP.

## Goal

Identify which (if any) ingest mechanism can achieve auto-sync from iOS to Cloudflare R2 (or equivalent object storage) with acceptable friction, cost, and complexity for a solo maker. If none work, confirm the fallback (D1-B: iOS Shortcut manual share).

## Questions

| # | Question |
|---|----------|
| **D1-Q1** | Which iOS apps support auto-upload of camera roll to S3-compatible storage (Cloudflare R2)? What are their pricing models? |
| **D1-Q2** | Can iOS Shortcuts run as background automations that upload new photos without user interaction? What are the limitations (trigger types, background execution time, reliability)? |
| **D1-Q3** | Does iCloud Photos sync to iCloud Drive on a Mac in a way that a local daemon could watch and push to R2? What setup is required? |
| **D1-Q4** | Can Cloudflare R2 receive uploads directly from iOS via pre-signed URLs? Could a Shortcut or background automation use this? |
| **D1-Q5** | What is the minimum viable setup for each mechanism? How many steps? How much configuration? |
| **D1-Q6** | Are there iOS photo-management apps with webhook/API integrations that could trigger an upload on new photo? |
| **D1-Q7** | Could rclone run on iOS (via iSH or similar) to sync camera roll to R2? What are the limitations? |

## Alternatives Under Investigation

| Alt | Mechanism | Key Questions |
|-----|-----------|---------------|
| D1-A | Third-party mobile sync app → R2 | Q1, Q5, Q6 |
| D1-B | iOS Shortcut manual share → Worker → R2 (fallback) | Already understood — no spike needed |
| D1-C | iCloud Drive desktop folder watch → R2 | Q3, Q5 |
| D1-D | iOS Shortcuts background automation → R2 | Q2, Q4, Q5 |
| D1-E | rclone on iOS (iSH) → R2 | Q7 |
| D1-F | Pre-signed URL upload from Shortcut/automation | Q4 |

## Acceptance

Spike is complete when we can describe:

- For each alternative: the concrete steps to set it up, the cost, the friction level, and whether it achieves true auto-sync (no per-photo user action)
- A recommendation for which alternative (if any) to use for MVP
- If none achieve auto-sync: confirmation that D1-B (manual share) is the fallback, with the accepted tradeoff on R0/R4
