---
shaping: true
---

# Maker Post — Slices

> Vertical implementation slices from the breadboard. Each slice is demo-able (visible UI). Max 9.

## Slice Summary

| # | Slice | Mechanism | Demo |
|---|-------|-----------|------|
| V1 | Photo → AI draft | E1 (Shortcut ingest), E4 (two-stage + RAG) | "Share a photo from Photos → see an AI caption draft on the web page" |
| V2 | Approve & Post | R9 (IG API), E3 (connect IG) | "Approve a draft → it posts to Instagram" |
| V3 | Edit + Regenerate | review UX | "Edit the caption, or regenerate a new draft" |
| V4 | Feedback loop | E5 (one-tap + explicit → style guide) | "Tap 'shorter' → the next draft is shorter" |
| V5 | Bulk drafts | R11 (queue several) | "Queue several photos, review them from a drafts list" |
| V6 | Seed style + onboarding | E3 (seed style guide) | "Answer 2 questions → future drafts match your style" |

## V1: Photo → AI draft

**Demo:** Share a photo from Photos → see an AI caption draft on the web page.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U1 | P1 | shortcut | "Maker Post" action | trigger | → N1 |
| N1 | P1 | worker | ingest handler (receive photo) | call | → N2 |
| N2 | P1 | worker | temp image store | write | → store |
| N3 | P1 | worker | vision describe — stage 1 (OpenRouter) | call | → N4 |
| N4 | P1 | worker | RAG: retrieve past approved posts | call | → N5 |
| N5 | P1 | worker | LLM write caption — stage 2 (OpenRouter) | call | → N6 |
| N6 | P1 | worker | draft store | write | → store |
| N7 | P1 | worker | render draft → web page | call | → P2 |
| U2 | P2 | review | photo preview | render | — |
| U3 | P2 | review | caption draft | render | — |

*Uses a default style guide (no seed yet).*

## V2: Approve & Post

**Demo:** Approve a draft → it posts to Instagram.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U10 | P4 | setup | connect Instagram | click | → N13 |
| N13 | P4 | worker | Instagram OAuth connect | call | → IG |
| U5 | P2 | review | Approve & Post | click | → N9 |
| N9 | P2 | worker | publish to IG API (or fallback) | call | → IG |
| N12 | P2 | worker | mark photo as approved (RAG source) | write | → approved store |

## V3: Edit + Regenerate

**Demo:** Edit the caption text, or regenerate a new draft.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U4 | P2 | review | edit caption | type | → N8 |
| N8 | P2 | worker | save edited caption | write | → store |
| U6 | P2 | review | Regenerate | click | → N10 |
| N10 | P2 | worker | regenerate caption | call | → N3 |

## V4: Feedback loop

**Demo:** Tap "shorter" → the next draft is shorter.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U7 | P2 | review | one-tap tweaks (shorter, casual…) | click | → N11 |
| U8 | P2 | review | free feedback field | type | → N11 |
| N11 | P2 | worker | apply tweak/feedback → style guide | write | → style guide |

## V5: Bulk drafts

**Demo:** Queue several photos, review them from a drafts list.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U9 | P3 | drafts | draft row | click | → P2 |

## V6: Seed style + onboarding

**Demo:** Answer 2 questions → future drafts match your style.

| # | Place | Component | Affordance | Control | Wires Out |
|---|-------|-----------|------------|---------|-----------|
| U11 | P4 | setup | seed style guide | type | → N14 |
| N14 | P4 | worker | save seed style guide | write | → style guide |
