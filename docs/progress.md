# Session Progress Log

## Current State

**Last Updated:** 2026-08-09
**Session ID:** —
**Active Feature:** Design revision — incorporate Andrei interview insights

## Status

### What's Done

- [x] Original app idea captured (`docs/idea.md`)
- [x] Architecture options explored A–D (`docs/architecture-options.md`)
- [x] Design v1 shaped — R0–R15, Shape E, fit check, breadboard, UX (`docs/shaping.md` v1, superseded)
- [x] Living design mindmap maintained (`docs/mindmap.md`)
- [x] Vertical implementation slices V1–V6 defined (`docs/slices.md` v1, to be re-sliced)
- [x] Andrei user interview conducted and transcribed (`docs/maker-post-app-andrei-interview.md`)
- [x] Repo scaffolded — `src/` (Worker placeholder), `web/` (review app placeholder), README
- [x] **Frame created** (`docs/frame.md`) — Source, Problem, Outcome, Less/More about (per framing-doc skill)
- [x] **Shaping v2 complete** (`docs/shaping.md`) — R0–R8, shapes A–D, fit check, Shape D selected (per shaping skill)
- [x] **Ingest spike created** (`docs/spike-ingest.md`) — auto-sync feasibility investigation (per shaping skill spike format)
- [x] **Mindmap updated** (`docs/mindmap.md`) — reflects v2 reframe

### What's In Progress

- [ ] **Shaping review** — Shape D and revised docs presented to user for confirmation (HITL check-in)
  - Details: Per user's "incremental — shaping first, then check in" instruction. Awaiting confirmation before proceeding to breadboard + slices.
  - Blockers: none — awaiting user review

### What's Next

1. **User reviews** shaping v2 (R0–R8, Shape D, fit check) → confirm or adjust
2. **Breadboard Shape D** — detail into UI + Code affordances, wiring diagram (per breadboarding skill)
3. **Re-slice** — vertical implementation slices from the breadboard (replaces v1 slices V1–V6)
4. **Run ingest spike** (`docs/spike-ingest.md`) — investigate auto-sync feasibility in parallel with breadboarding
5. **Update README.md** — reflect reframe, new architecture, deferred features
6. Once breadboard + slices confirmed: begin implementation of revised Slice 1

## Interview Insights → Shaping v2 Mapping

> See `docs/frame.md` for the full problem frame. Key divergences that drove the re-shaping:

| Interview insight | Source | How addressed in shaping v2 |
|---------|--------|---------------------------|
| Andrei writes own captions; AI caption gen not important | [4, 18, 19] | R7: user writes own captions; D8 optional, off by default |
| Real pain = organization ("micro-movements") | [7, 34] | R1: auto-classify by project; R0: zero manual effort |
| Auto-group by project (5–7 in parallel) | [3, 32] | R1: vision model groups by topic/product/activity |
| Post ideas when enough content accumulates | [32, 35] | R2: post-idea detection + status tracking |
| "To be posted" → "posted" lifecycle | [35] | R2: status tracking in web app |
| Everything happens by itself (zero friction) | [27] | R0 + R4: auto-sync from device; D1 spike |
| PKM / reference library | [15, 16] | R5: experiment notes linked to projects |
| Editing in VN is pleasant | [9] | R7: AI organizes, doesn't create; no IG posting in MVP |
| Target audience = engineers/companies | [23] | Deferred: trend analysis (future) |
| Content gap detection | [47] | Deferred: later enhancement |
| iOS Photo albums per project | [39] | Deferred: investigate (might need native app) |

## Blockers / Risks

- [ ] **Ingest mechanism unknown (D1 ⚠️)**: Auto-sync from iOS to Cloudflare R2 — no proven mechanism yet. Spike created (`docs/spike-ingest.md`). Fallback: D1-B (iOS Shortcut manual share) if no auto-sync found.
- [ ] **Vision classification quality**: Can free OpenRouter vision models reliably group photos by project? Needs early validation during implementation. Not a ⚠️ flag (mechanism is understood) but a quality risk.
- [ ] **iOS Photo album per project**: May require native app — deferred, under investigation.
- [ ] **No Instagram posting in MVP**: Andrei creates videos himself. Web app tracks posted/not-posted status only.

## Decisions Made

- **Start with design revision, not V1 implementation**: The interview surfaced enough divergence that implementing the current slices would build the wrong thing.
  - Context: Andrei's interview reveals the core value is organization, not caption generation
  - Alternatives considered: Start V1 implementation and revise later (rejected — would waste effort on the wrong shape)
- **Full reframe to organizational assistant**: AI organizes (classify, group, surface ideas), doesn't create (no caption generation by default).
  - Context: Andrei explicitly says caption writing is not important [Source 4, 18, 19]; editing is pleasant [Source 9]
  - Alternatives: Hybrid (keep AI captions prominent) — rejected; Andrei was clear
- **No Instagram posting in MVP**: Web app shows posted/not-posted status; Andrei creates videos himself.
  - Context: Andrei uses VN editor and finds it pleasant [Source 9]
  - Alternatives: Include IG API in MVP — rejected; over-scopes MVP
- **Auto-sync as goal, spike for feasibility**: Pipeline constant (D2–D8), ingest as spike (D1).
  - Context: Andrei wants zero friction [Source 27]; iCloud has no public API
  - Alternatives: Accept manual share for MVP (D1-B fallback) — held as fallback if spike fails
- **PKM in MVP**: Reference library for experiment results is in scope.
  - Context: Andrei needs to store filament configs, print settings [Source 15, 16]
  - Alternatives: Defer PKM — rejected; user explicitly selected it for MVP

## Files Modified This Session

- `docs/progress.md` — updated (this file)
- `AGENTS.md` — created (agent working-rules file)
- `docs/frame.md` — created (problem frame, per framing-doc skill)
- `docs/shaping.md` — rewritten (shaping v2: R0–R8, shapes A–D, fit check, Shape D, per shaping skill)
- `docs/spike-ingest.md` — created (ingest feasibility spike, per shaping skill)
- `docs/mindmap.md` — updated (reflects v2 reframe)

## Evidence of Completion

- [ ] Shaping v2 reviewed and confirmed by user (HITL check-in)
- [ ] Breadboard complete: Shape D detailed into affordances + wiring
- [ ] Slices re-defined: vertical implementation slices from breadboard
- [ ] README updated to reflect v2
- [ ] No code verification applicable (design phase)

## Notes for Next Session

1. **Shaping v2 is ready for user review.** The key decision point: confirm Shape D (hybrid — pipeline constant, ingest as spike). See `docs/shaping.md` fit check.
2. After confirmation: **breadboard Shape D** using the breadboarding skill — detail D2–D8 into UI + Code affordances with wiring. This replaces the v1 breadboard in `docs/slices.md`.
3. **Run the ingest spike** (`docs/spike-ingest.md`) in parallel — it can happen alongside breadboarding since the pipeline doesn't depend on the ingest mechanism.
4. **Re-slice** the breadboard into vertical implementation slices (max 9). Each slice must be demo-able (visible UI). The v1 slices (V1–V6) are superseded.
5. **Update README.md** after breadboarding + slicing — reflect the reframe, new architecture, deferred features.
6. The reframe in one sentence: **from "AI caption generator" → "AI organizational assistant for makers."** AI classifies, groups, surfaces post ideas, stores reference knowledge. Andrei writes captions and creates videos himself.
7. Watch the D1 spike result — it determines whether MVP achieves R0/R4 (auto-sync) or falls back to D1-B (manual share).
