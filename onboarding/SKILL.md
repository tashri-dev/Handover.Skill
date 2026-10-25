---
name: onboarding
description: Produce an onboarding guide for someone joining a system or group of services: explanation of the system, topology/sequence/class/flow diagrams, glossary, guided code tour and a first-week path, exported to Obsidian. Use when the user says "onboarding", "onboard me/someone", "help a new joiner understand this", "explain this system to a newcomer", or was just handed a system and needs to learn it. Not for the outgoing owner documenting for successors (use handover).
---

# Onboarding

Audience: the **newcomer**. Goal: understand the system's shape, find their way in the code, and ship a first safe change.

Shared references (read before starting):
- `~/.claude/skills/_shared/canary.md` (opt-in drift protection)
- `~/.claude/skills/_shared/diagrams.md` (Mermaid rules)
- `~/.claude/skills/_shared/obsidian-export.md` (output)

## 0. Setup
1. Run canary detection per `canary.md`. State the mode (ON with the detected canary / OFF) in one line.
2. If a `CHECKPOINT.md` is given or found, follow Resume in `canary.md` and continue at the first remaining phase.
3. Confirm scope (repo paths and/or services, system slug) and the newcomer's background (role, languages known) in one question; default to a generalist backend engineer.
4. Look for an existing handover for this system in the vault (`handover_dir/<slug>/`). If it exists, build on it and link to it instead of re-deriving; still verify key claims against code.
5. Resolve the output folder from `obsidian-export.md`.

## 1. Discover
Read code and config: purpose, entry points, dependencies, data stores, how to run and test locally, conventions (structure, naming, error handling), where the sharp edges are. Record `path:line` evidence. Drift check after this phase (if canary ON).

## 2. Write notes (drift check after each)
1. **Overview**: what and why, users, the system in five sentences, service list.
2. **Architecture**: topology flowchart, layers, data flow at a glance, "how to read this codebase".
3. **Flows**: sequence diagrams for the 2-3 flows a newcomer will meet first; flowcharts for key logic. Each diagram followed by a short "walk through it" paragraph.
4. **Domain Model**: class diagram of core concepts, explained in plain language, linked to Glossary.
5. **First Week**: day-by-day path: set up environment, run the system, run tests, trace one request end to end, read named files in order (`path` + why), pick a starter task (low-risk, with where to change and how to verify), who/what to ask (from ownership signals), self-check questions.
6. **Risks & Open Questions**: gotchas, traps, confusing parts, things still unclear.
7. **Glossary**: every domain term and acronym used.
8. **Index** (MOC), status `draft`.

## 3. Review
Verify diagrams against code. Offer to quiz the newcomer with the self-check questions. Set `status: reviewed` once confirmed.

## 4. Export
Finish via `obsidian-export.md`: list created files and the Index path. If canary ON and the run is complete, delete `CHECKPOINT.md`.

## Rules
- Explain for the stated background; define terms on first use.
- Facts come from code you read; label inference.
- Never write secrets into notes.
