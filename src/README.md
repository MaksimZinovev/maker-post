# Maker Post — Worker (src/)

Cloudflare Worker that handles:

- **Ingest** — receives photos from the iOS Shortcut (E1)
- **AI pipeline (E4)** — two-stage: vision describe → LLM writes caption (style guide + RAG)
- **Feedback loop (E5)** — one-tap tweaks + explicit feedback → style guide
- **Publish (R9)** — Instagram Graph API (official) + semi-manual fallback

> Scaffolding placeholder. Implementation begins with slice V1.