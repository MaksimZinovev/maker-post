---
shaping: true
---

# Maker Post — Shaping (v2)

> **Reframe:** The Andrei interview (`docs/maker-post-app-andrei-interview.md`) revealed the core problem is **organization**, not caption generation. This is a full re-shaping. See `docs/frame.md` for the problem frame. v1 shaping is superseded.

## Requirements (R)

| ID | Requirement | Status |
|----|-------------|--------|
| R0 | Dead-simple / lowest friction — zero manual effort beyond taking photos; "I can just take a photo, everything else happens by itself" [Source 27] | Core goal |
| R1 | Auto-classify media by project — vision model groups photos/videos by topic, product, or activity; zero manual tagging [Source 3, 27, 32] | Core goal |
| R2 | Post-idea detection + status tracking — surface "post ideas" when enough content accumulates per project; track "to be posted" → "posted" [Source 32, 35] | Must-have |
| R3 | Web review app — view grouped files by project, review post ideas, see posted/not-posted status [Source 35] | Must-have |
| R4 | Auto-sync from device to cloud — photos/videos sync automatically; no manual share action required [Source 27] | Must-have |
| R5 | PKM / reference library — store experiment results (filament configs, print settings) as reference for future projects [Source 15, 16] | Must-have |
| R6 | Low cost + simple system — free tier / pennies at low volume; few moving parts [Source 2] | Must-have |
| R7 | Quality floor — not AI slop; reflects user's voice; user writes own captions; AI organizes, doesn't create [Source 4, 18, 19, 23] | Core goal |
| R8 | Start personal (Andrei), design cleanly → product later | Core goal |

---

## Shapes

### A: Third-party mobile sync → R2 → classify → web app

Third-party iOS sync app auto-uploads camera roll to Cloudflare R2. Cron Worker classifies new photos via vision model. Web app shows project groupings + post ideas.

| Part | Mechanism | Flag |
|------|-----------|:----:|
| A1 | Third-party iOS sync app (rclone, Photos+, CloudsLinker) auto-uploads camera roll to Cloudflare R2 | ⚠️ |
| A2 | Cron Worker polls R2 for unclassified items | |
| A3 | Vision model (OpenRouter free) classifies each photo → assigns to project cluster | |
| A4 | Project + classification store (D1/KV): photo → project, project metadata (name, count, date range) | |
| A5 | Post-idea detector: threshold-based surfacing (N+ photos per project → "post idea") | |
| A6 | Web app: project gallery (grouped media), post-idea list (to-be-posted / posted), status tracking | |
| A7 | PKM store: experiment notes + results linked to projects | |
| A8 | Optional AI caption draft (off by default; user can request) | |

### B: iOS Shortcut manual share → Worker → R2 → classify → web app

Same pipeline as A, but ingest via iOS Shortcut (manual share per photo).

| Part | Mechanism | Flag |
|------|-----------|:----:|
| B1 | iOS Shortcut: share photo → POST to Worker endpoint | |
| B2 | Worker stores in R2, then runs A2–A8 (same pipeline) | |

### C: iCloud Drive desktop → R2 → classify → web app

iCloud Photos syncs to Mac → local folder watch pushes to R2. Same pipeline.

| Part | Mechanism | Flag |
|------|-----------|:----:|
| C1 | iCloud Photos → iCloud Drive on Mac → local folder watch daemon → push to R2 | ⚠️ |
| C2–A8 | Same pipeline as A | |

### D: Hybrid — pipeline constant, ingest as spike

Build the classification + organization + web app pipeline now (constant regardless of ingest). Spike ingest alternatives in parallel; pick the best.

| Part | Mechanism | Flag |
|------|-----------|:----:|
| D1 | Ingest mechanism — spike alternatives (see D1-A through D1-D below) | ⚠️ |
| D2 | New-photo detector — cron Worker polls R2 for unclassified items | |
| D3 | Vision classifier — OpenRouter free vision model describes each photo; LLM clusters descriptions into project groups | |
| D4 | Project + classification store (D1/KV): photo → project, project metadata (name, count, date range) | |
| D5 | Post-idea detector: threshold-based surfacing (N+ photos per project → "post idea") | |
| D6 | Web app: project gallery (grouped media), post-idea list (to-be-posted / posted), status tracking | |
| D7 | PKM store: experiment notes + results linked to projects | |
| D8 | Optional AI caption draft (off by default; user can request) | |

#### D1: Ingest mechanism alternatives

| Alt | Mechanism | Flag |
|-----|-----------|:----:|
| D1-A | Third-party mobile sync app → R2 (from A1) | ⚠️ |
| D1-B | iOS Shortcut manual share → Worker → R2 (from B1) | |
| D1-C | iCloud Drive desktop folder watch → R2 (from C1) | ⚠️ |
| D1-D | iOS Shortcuts background automation → R2 | ⚠️ |

---

## Fit Check

| Req | Requirement | Status | A | B | C | D |
|-----|-------------|--------|---|---|---|---|
| R0 | Dead-simple / lowest friction — zero manual effort beyond taking photos | Core goal | ❌ | ❌ | ❌ | ❌ |
| R1 | Auto-classify media by project | Core goal | ✅ | ✅ | ✅ | ✅ |
| R2 | Post-idea detection + status tracking | Must-have | ✅ | ✅ | ✅ | ✅ |
| R3 | Web review app | Must-have | ✅ | ✅ | ✅ | ✅ |
| R4 | Auto-sync from device to cloud | Must-have | ✅ | ❌ | ✅ | ❌ |
| R5 | PKM / reference library | Must-have | ✅ | ✅ | ✅ | ✅ |
| R6 | Low cost + simple system | Must-have | ❌ | ✅ | ❌ | ✅ |
| R7 | Quality floor — not AI slop; user writes own captions | Core goal | ✅ | ✅ | ✅ | ✅ |
| R8 | Start personal, design for product later | Core goal | ✅ | ✅ | ✅ | ✅ |

**Notes:**

- **A fails R0, R6:** Third-party sync app adds friction (install, configure, subscription) and cost. A1 is flagged ⚠️ — we don't know which apps support R2/S3-compatible sync on iOS.
- **B fails R0, R4:** Manual share action contradicts "everything happens by itself." B1 is understood (no flag) but doesn't meet the auto-sync requirement.
- **C fails R0, R6:** Requires Mac running + iCloud Drive setup — extra friction and moving parts. C1 is flagged ⚠️ — iCloud Photos auto-export to iCloud Drive may not work as assumed.
- **D fails R0, R4:** Ingest mechanism (D1) is flagged ⚠️ — we don't yet know how to achieve auto-sync from iOS without a third-party app. **Spike to resolve** (see `docs/spike-ingest.md`). D1-B (Shortcut) is understood but doesn't meet R0/R4 — it's the fallback if the spike finds no viable auto-sync.

### Why D

D is the recommended shape because:

1. **The pipeline (D2–D8) is the core value** — classification, organization, post-idea detection, web app, PKM. It's fully understood and identical across all shapes.
2. **The ingest mechanism (D1) is the main unknown** — the spike de-risks it without blocking pipeline development.
3. **Building the pipeline doesn't depend on which ingest wins** — the Worker receives photos from R2 regardless of how they got there.
4. **Fallback exists** — if the spike finds no viable auto-sync, D falls back to D1-B (Shortcut manual share) and accepts R0/R4 as partially met. The pipeline still delivers value.

Per the shaping skill: "A selected shape should have no flags (all ❌ resolved), or explicit spikes to resolve them." D has an explicit spike to resolve D1.

---

## Deferred / Future

Features explicitly out of MVP scope, captured for later:

| Feature | Source | Status |
|---------|--------|--------|
| Instagram auto-posting | Original R9; Andrei creates videos himself | Deferred — MVP shows posted/not-posted status, doesn't post |
| iOS Photo album per project | [Source 39] | Investigate — might defer if no viable mechanism without native app |
| Content gap detection — AI suggests missing photos | [Source 47] | Deferred — later enhancement |
| Trend analysis + content suggestions (audience-aware) | [Source 21, 23] | Deferred — future nice-to-have |
| Feedback on LLM classification — steer/tweak grouping | User decision | Nice-to-have — later enhancement |
| Video draft generation on autopilot | User decision | Future — select from alternatives, approve, post |
| Stats-based self-learning — analyze engagement → steer content | Original R15 | Deferred — needs post history first |

---

## Detail D: Web Review App UX (provisional)

> To be breadboarded after shape confirmation. Provisional UX from interview + user decisions.

| Part | Decision |
|------|----------|
| **Project gallery** | Grid of project cards, each showing: project name (auto-generated or user-named), photo count, date range, thumbnail collage |
| **Project detail view** | All media for a project, grouped by type (photos, videos). "Select all" affordance for moving to external editor [Source 39] |
| **Post-idea list** | Projects that have crossed the post-idea threshold. Status badge: "to be posted" / "posted." Tap to view the grouped media [Source 35] |
| **Status tracking** | Mark a post idea as "posted" → moves to posted list. Timeline view: what was posted when |
| **PKM reference** | Per-project notes: experiment results, filament configs, lessons learned [Source 15, 16] |
| **Optional caption draft** | Off by default. User can request an AI draft suggestion for a post idea. Andrei writes his own [Source 4] |
