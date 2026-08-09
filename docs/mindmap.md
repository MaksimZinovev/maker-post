# Maker Post — Design Mindmap

> Living document. Updated as we explore. Each node is short; details live in the draft.

## Maker Post

- **Problem**: solo maker (Andrei) → rough phone photos → engaging IG posts, minimal effort
- **Core promise**: photo → post on autopilot, with his voice, not AI slop
- **Key tension**: dead-simple vs. not-AI-slop vs. reflects his vision
- **Reframe (R2)**: differentiator = engagement performance, not text voice
  - goal: content that grows followers/likes, steered by feedback + compounding stats
  - guardrail (R2b): not generic AI slop

## Settled so far

### Round 1 — foundations

- **Scope**: start personal (Andrei), design cleanly → could become a product later
- **Platform**: iOS native (Photos share sheet = lowest friction)
- **MVP workflow**: reviewed — photo → AI draft → user approves → post

### Round 2 — heart of the product

- **Voice**: seed style guide now + refined by feedback loop later
- **Feedback loop**: full loop — AI suggests improvements, feedback compounds, steers future runs
- **Instagram**: official API now, semi-manual fallback if app review denied
  - fact: needs Creator/Business account + Meta app review; 1/day well within 50/day cap

### Round 3 — content, storage, scope

- **Content**: photos first, design so Reels can be added later
- **MVP scope**: core + bulk drafts (queue several, review later) — may simplify
- **Architecture**: D (Hybrid) → refined into **E** (see below)

### Round 4 — AI pipeline + reframe

- **AI pipeline (E4)**: two-stage — vision describes photo factually → LLM writes caption using style guide + RAG over past approved posts
- **Feedback loop (E5)**: one-tap tweaks + explicit feedback → persistent style guide; pairwise + auto-rewrite later
- **R2 reframe**: engagement performance (not text voice); keep "not AI slop" as guardrail
- **Reels/video (E6)**: later, pipeline designed to slot in (e.g. hyperframes)
- **Stats self-learning (E7/R15)**: later, once enough post history — analyze engagement → steer future

## Architecture E (selected direction)

- **E1 — MVP ingest via iOS Shortcut** (no native app) + hosted web review app
  - share photo → Shortcut POSTs to Worker → AI runs → opens web page → approve → publish
  - why: avoids building a native iOS app for MVP; Worker + web backend carry over to a real app later
- **E2 — Auto-sync later, modified**: only photos Andrei tags (special album/folder) feed the pipeline
  - avoids AI slop by not auto-processing every photo
- **E3 — Guided setup/onboarding**: one-time setup made easy, automated where possible
  - tradeoff accepted: setup once, then near-zero-effort capture daily (pays off)
- **E4 — AI pipeline**: two-stage (vision describe → LLM write) + RAG over approved posts
  - provider: OpenRouter free primary (vision `llama-3.2-11b-vision:free`, text `llama-3.3-70b:free`), Ollama Cloud fallback (glm-5.1)
- **E5 — Feedback loop**: one-tap tweaks + explicit feedback → style guide; pairwise + auto-rewrite later
- **E6 — Reels/video later**: pipeline designed so video assembly slots in without rework
- **E7 — Stats self-learning later**: IG insights → correlate features with engagement → steer future

## Status

- **Design fully shaped** — requirements, shape E, fit check, breadboard, UX, AI pipeline all recorded
- **Next step**: slice into implementation (V1 plan) — ask user
