# Maker Post — Design Mindmap

> Living document. Updated as we explore. Each node is short; details live in the shaping doc.

## Maker Post

- **Problem**: solo maker (Andrei) → loses "brain fuel" organizing photos across 5–7 parallel projects → wants to focus on creative work, not organization
- **Core promise**: take a photo, everything else happens by itself — photos auto-grouped by project, post ideas surfaced, reference library compounds
- **Key reframe (v2)**: from "AI caption generator" → "AI organizational assistant for makers." Andrei writes his own captions [Source 4, 18, 19]. The pain is organization, not content creation [Source 7, 34]
- **Guardrail (R7)**: not AI slop; AI organizes, doesn't create; user writes own captions

## Settled so far

### v1 — original shaping (superseded)

- Scope: start personal (Andrei), design cleanly → product later
- Platform: iOS (Photos share sheet)
- MVP workflow: photo → AI draft → approve → post to Instagram
- Architecture E: Shortcut ingest + web review + Worker + two-stage AI pipeline

### v2 — reframe from interview

- **Reframe trigger**: Andrei interview revealed the core pain is organization, not caption generation [Source 7, 34, 27]
- **New core flow**: sync photos → auto-classify by project → surface post ideas → web review app → track posted/not-posted
- **No Instagram posting in MVP**: Andrei creates videos himself (VN editor — "pleasant work" [Source 9]). Posting is a future enhancement.
- **PKM is in MVP**: reference library for experiment results (filament configs, print settings) [Source 15, 16]
- **AI caption generation**: optional, off by default [Source 4, 18, 19]
- **Auto-sync is the goal**: investigate feasibility for MVP [Source 27]. Spike created (`docs/spike-ingest.md`).
- **Shape D selected**: pipeline constant (classify → group → web app), ingest as spike

## Shape D (selected direction)

- **D1 — Ingest mechanism** (⚠️ spike): auto-sync from iOS to Cloudflare R2. Alternatives: third-party app, iCloud Drive, Shortcuts automation. Fallback: iOS Shortcut manual share.
- **D2 — New-photo detector**: cron Worker polls R2 for unclassified items
- **D3 — Vision classifier**: OpenRouter free vision model describes each photo; LLM clusters into project groups
- **D4 — Project + classification store**: D1/KV — photo → project, project metadata
- **D5 — Post-idea detector**: threshold-based surfacing (N+ photos per project → "post idea")
- **D6 — Web app**: project gallery (grouped media), post-idea list (to-be-posted / posted), status tracking
- **D7 — PKM store**: experiment notes + results linked to projects
- **D8 — Optional AI caption draft**: off by default; user can request

## Deferred / Future

- Instagram auto-posting (Andrei creates videos himself for MVP)
- iOS Photo album per project (investigate — might defer)
- Content gap detection — AI suggests missing photos [Source 47]
- Trend analysis + content suggestions (audience-aware: engineers/companies [Source 23])
- Feedback on LLM classification — steer/tweak grouping
- Video draft generation on autopilot — select from alternatives, approve, post
- Stats-based self-learning — analyze engagement → steer future content

## Status

- **Frame created** (`docs/frame.md`) — problem, outcome, less/more about
- **Shaping v2 complete** (`docs/shaping.md`) — R0–R8, shapes A–D, fit check, Shape D selected
- **Ingest spike created** (`docs/spike-ingest.md`) — investigate auto-sync feasibility
- **Next step**: Confirm Shape D with user → breadboard D → slice into implementation
