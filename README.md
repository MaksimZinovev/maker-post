# Maker Post

> Photo → engaging Instagram post, on autopilot, in your voice — not AI slop.

Maker Post turns rough phone photos into social media posts that build community and followership for solo makers. Designed for Andrei, a part-time 3D-printing entrepreneur who wants minimal friction: take a photo, get a post.

## Why

Solo builders don't have a marketing team. They have limited time and don't want complex setups, multi-step flows, or generic AI-generated slop. They want their voice, their vision, minimal effort — and content that actually performs.

## Core principles

- **Dead-simple** — grandma could install and use it
- **AI-powered** — generates captions + suggests improvements from your feedback
- **Compounding feedback** — your steering shapes every future post; human feedback compounds
- **Lean & visual** — everything you need, nothing you don't; visual over textual
- **Not AI slop** — quality floor; reflects your voice and vision

## Architecture (selected: E — Hybrid + Shortcut MVP)

| Part | Mechanism |
|------|-----------|
| **E1** | MVP ingest via iOS Shortcut (no native app) + hosted web review app |
| **E2** | Later: auto-sync only tagged/favorite photos (avoids AI slop) |
| **E3** | Guided one-time setup/onboarding |
| **E4** | Two-stage AI pipeline: vision describes → LLM writes caption (style guide + RAG over approved posts). OpenRouter free models primary, Ollama Cloud fallback |
| **E5** | Feedback loop: one-tap tweaks + explicit feedback → persistent style guide |
| **E6** | Reels/video slot in later |
| **E7** | Stats-based self-learning later (IG insights → steer future content) |

## Repository layout

```
maker-post/
├── README.md           # this file
├── docs/               # design documents (shaping work)
│   ├── idea.md                 # original app idea
│   ├── architecture-options.md # A/B/C/D architecture comparison
│   ├── mindmap.md              # living design mindmap
│   ├── shaping.md              # requirements, shapes, fit check, UX
│   └── slices.md               # vertical implementation slices (V1–V6)
├── src/                # Cloudflare Worker (API + AI pipeline)
└── web/                # hosted web review app
```

## Implementation slices

| # | Slice | Demo |
|---|-------|------|
| V1 | Photo → AI draft | Share a photo → see an AI caption draft on the web page |
| V2 | Approve & Post | Approve a draft → it posts to Instagram |
| V3 | Edit + Regenerate | Edit the caption, or regenerate a new draft |
| V4 | Feedback loop | Tap "shorter" → the next draft is shorter |
| V5 | Bulk drafts | Queue several photos, review from a drafts list |
| V6 | Seed style + onboarding | Answer 2 questions → future drafts match your style |

See [`docs/slices.md`](docs/slices.md) for the full breadboard.

## Status

Design fully shaped. Next step: slice V1 into implementation.

## Tech stack (planned)

- **Cloudflare Workers** — serverless API + AI pipeline
- **OpenRouter** — free vision + text models (primary), Ollama Cloud (fallback)
- **Instagram Graph API** — publishing (official API + semi-manual fallback)
- **iOS Shortcuts** — zero-app MVP ingest
- **Web app** — review/approve/publish UI