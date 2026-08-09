---
shaping: true
---

# Maker Post — Shaping

## Requirements (R)

| ID | Requirement | Status |
|----|-------------|--------|
| R0 | Dead-simple for a non-technical user (grandma could do it) — lowest friction | Core goal |
| R1 | Photo → engaging IG post with minimal effort | Core goal |
| R2 | Generate engaging posts/reels that perform (grow followers/likes), steered by human feedback + compounding analysis of past engagement | Core goal |
| R2b | Content doesn't read as generic AI slop (quality floor) | Core goal |
| R3 | Feedback loop: human steering + AI self-learning, compounding | Must-have |
| R4 | Low cost (free tier / pennies at 1 post/day) | Must-have |
| R5 | Simple system — few moving parts | Must-have |
| R6 | Start personal (Andrei), design cleanly → product later | Core goal |
| R7 | iOS / Photos share sheet (lowest friction) | Must-have |
| R8 | Reviewed workflow — approve before post | Must-have |
| R9 | Official IG API + semi-manual fallback | Must-have |
| R10 | Photos-first for MVP; design pipeline so Reels/video slot in later | Leaning yes |
| R11 | Bulk drafts — queue several, review later | Nice-to-have |
| R12 | MVP without a native iOS app (Shortcut + web) | Leaning yes |
| R13 | Auto-sync later, only tagged/favorite photos feed the pipeline (avoid AI slop) | Leaning yes |
| R14 | Guided setup/onboarding to make one-time setup easy | Nice-to-have |
| R15 | Stats-based self-learning: analyze past engagement → steer future content | Deferred (later) |

## Shapes

### A: On-demand, synchronous

Share → upload to Worker → AI runs → draft in seconds → approve → publish. No storage, no scheduling.

### B: On-demand, async queue

Share → upload → enqueue → AI in background → notification when draft ready. User never waits.

### C: Auto-sync + scheduled pipeline

Photos auto-upload to cloud → scheduled job polls → AI → drafts queue. Zero-effort capture, most moving parts.

### D: Hybrid

Start with A, add C's auto-sync later.

### E: Hybrid + Shortcut MVP + tagged-photo auto-sync (selected direction)

| Part | Mechanism |
|------|-----------|
| **E1** | MVP ingest via iOS Shortcut (no native app) + hosted web review app; Worker + web backend carry over to a real app later |
| **E2** | Later auto-sync: only photos Andrei tags (special album/folder) feed the pipeline → avoids AI slop |
| **E3** | Guided setup/onboarding: one-time setup automated where possible |
| **E4** | AI pipeline: two-stage — vision model describes photo factually, then LLM writes caption using style guide + RAG over past approved posts. **Provider: OpenRouter free models primary** (Stage 1 `llama-3.2-11b-vision-instruct:free`, Stage 2 `llama-3.3-70b-instruct:free`), **Ollama Cloud fallback** (glm-5.1). Worker calls external OpenAI-compatible API; OpenRouter handles failover. |
| **E5** | Feedback loop: one-tap tweaks + explicit feedback write into a persistent style guide; pairwise (A) + auto-rewrite (D) added later |
| **E6** | Reels/video slot in later: pipeline designed so video assembly (e.g. hyperframes) can be added without rework |
| **E7** | Stats-based self-learning later: pull IG insights, correlate content features with engagement, steer future content |

## Fit check (R × E)

| Req | Requirement | Status | E |
|-----|-------------|--------|---|
| R0 | Dead-simple / lowest friction | Core goal | ✅ |
| R1 | Photo → engaging post, minimal effort | Core goal | ✅ |
| R2 | Content performs (grow followers/likes) | Core goal | ⚠️ |
| R2b | Not generic AI slop | Core goal | ✅ |
| R3 | Feedback loop (steer + compound) | Must-have | ⚠️ |
| R4 | Low cost | Must-have | ✅ |
| R5 | Simple system | Must-have | ✅ |
| R6 | Personal → product | Core goal | ✅ |
| R7 | iOS / share sheet | Must-have | ✅ |
| R8 | Reviewed workflow | Must-have | ✅ |
| R9 | Official IG API + fallback | Must-have | ✅ |
| R10 | Photos-first, Reels later | Leaning yes | ✅ |
| R11 | Bulk drafts | Nice-to-have | ✅ |
| R12 | No native app for MVP (Shortcut + web) | Leaning yes | ✅ |
| R13 | Tagged-photo auto-sync later | Leaning yes | ✅ |
| R14 | Guided setup/onboarding | Nice-to-have | ✅ |
| R15 | Stats-based self-learning | Deferred | ❌ |

**Gaps:**

- **R2 partial (⚠️):** MVP steers toward engagement via human feedback (one-tap + explicit) + RAG over approved posts. The "compounding analysis of past engagement" (stats) is deferred to E7/R15 — so R2's full goal is only partially met in MVP.
- **R3 partial (⚠️):** MVP has human steering + compounding via style guide. "AI self-learning from engagement stats" is deferred (E7).
- **R15 (❌):** stats-based self-learning is explicitly deferred — not in MVP. Accepted.
- **R13 (E2) flagged unknown ⚠️:** exact iOS "tagging" mechanism not yet pinned down — later scope, not a blocker.

## Detail E: Web review app UX (approved)

| Part | Decision |
|------|----------|
| **Review screen** | Single card: photo on top, caption below, Approve & Post + one-tap tweak row. One thing at a time. |
| **One-tap tweaks** | ~8 curated, each maps to a style-guide rule: length (shorter/longer), tone (casual/professional/playful), structure (add CTA / hashtags / question), format (more emoji / no emoji) |
| **Drafts list (bulk)** | Simple queue: photo thumb + caption preview + Approve / Edit / Delete; tap → single-card review |
| **Setup/onboarding** | 3 guided steps: (1) connect Instagram OAuth, (2) seed style guide (~2 min, questions or example posts), (3) install the Shortcut |
