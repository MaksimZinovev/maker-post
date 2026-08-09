# AGENTS.md

Maker Post — an AI organizational assistant for solo makers (3D printing, engineering) that auto-classifies camera-roll photos by project, detects post-ready content, and suggests missing media. Designed for Andrei, a part-time 3D-printing entrepreneur who works 5–7 projects in parallel and wants minimal friction.

## Startup Workflow

Before writing code:

1. **Confirm working directory** with `pwd`
2. **Read this file** completely
3. **Read project docs if present** (`docs/progress.md`, `docs/shaping.md`, `docs/mindmap.md`, `docs/slices.md`, README)
4. **Run `./init.sh`** to verify environment is healthy (if present — not yet created for this repo)
5. **Read `feature_list.json`** to see current feature state (if present — not yet created for this repo)
6. **Review recent commits** with `git log --oneline -5`

If baseline verification is failing, repair that first before adding new scope.

## Working Rules

- **One feature at a time**: Pick exactly one unfinished feature from `feature_list.json` (or from `docs/progress.md` during design phase)
- **Verification required**: Don't claim done without running verification commands
- **Update artifacts**: Before ending session, update `docs/progress.md` and `feature_list.json` (once created)
- **Stay in scope**: Don't modify files unrelated to the current feature
- **Leave clean state**: Next session must be able to run `./init.sh` immediately (once created)
- **Design phase rule**: While in design revision, the "feature" is a design-doc update. Verify consistency across all docs before marking done.

## Required Artifacts

- `feature_list.json` — Feature state tracker (source of truth) — **not yet created; create when entering implementation phase**
- `docs/progress.md` — Session continuity log ✅
- `init.sh` — Standard startup and verification path — **not yet created; create when entering implementation phase**
- `session-handoff.md` — Optional, for larger sessions

## Definition of Done

A feature is done only when ALL of the following are true:

- [ ] Target behavior is implemented (or design doc updated and internally consistent)
- [ ] Required verification actually ran (tests / lint / type-check — or doc-consistency check during design phase)
- [ ] Evidence recorded in `feature_list.json` or `docs/progress.md`
- [ ] Repository remains restartable from standard startup path

## End of Session

Before ending a session:

1. Update `docs/progress.md` with current state
2. Update `feature_list.json` with new feature status (once created)
3. Record any unresolved risks or blockers
4. Commit with descriptive message once work is in safe state
5. Leave repo clean enough for next session to run `./init.sh` immediately (once created)

## Verification Commands

```bash
# Full verification (recommended) — implementation phase
npm run test && npm run typecheck

# Design-phase verification — check doc consistency
# Ensure shaping.md, mindmap.md, slices.md, README.md are aligned
# No automated command yet; manual review required
```

Required checks:

- **Implementation phase**: `npm run test`, `npm run typecheck`, `npm run lint`
- **Design phase**: Manual review — all design docs reference each other consistently (requirements ↔ shapes ↔ slices ↔ mindmap)

## Escalation

If you encounter:

- **Architecture decisions**: Consult `docs/architecture-options.md` and `docs/shaping.md` if present, otherwise ask user
- **Unclear requirements**: Check `docs/shaping.md` and `docs/maker-post-app-andrei-interview.md` if present, otherwise ask user
- **Repeated test failures**: Update `docs/progress.md`, flag for human review
- **Scope ambiguity**: Re-read `docs/progress.md` "What's In Progress" section for current scope
- **Design-doc ripple**: When editing a `.md` file with `shaping: true` frontmatter, check for ripple effects across all related docs (affordance tables, fit checks, work streams)

## Code Search & Exploration Discipline

Use **cx** (semantic code navigation) and **ck** (semantic grep by embedding) as the primary tools for code search and exploration:

- `cx overview .` — table of contents for a file or directory
- `cx symbols` — search symbol definitions across the project
- `cx definition` — read a function/type body without reading the whole file
- `cx references` — find all usages of a symbol
- `ck "<single-word>" .` — semantic code search (single word, not phrases)

Do **NOT** use grep/find/rg/ack/ag/fd when cx or ck can do the job. Reserve raw-grep tools for cases where semantic navigation genuinely can't help (exact-string matching, non-code text, or when cx/ck return no matches and a fallback is needed).
