---
name: handover
description: Produce a handover package for a system or group of services being passed to someone else: system explanation, topology/sequence/class/flow diagrams, runbook, risks and open questions, exported to Obsidian. Use when the user says "handover", "hand over", "I'm leaving/transferring this service", "document this for the next owner", or passes repos/services to be handed off. Not for helping a newcomer learn a system (use onboarding).
---

# Handover

Audience: the **incoming owner**. Goal: they can operate, change and debug the system without the outgoing owner.

Shared references (read before starting):
- `~/.claude/skills/_shared/canary.md` (opt-in drift protection)
- `~/.claude/skills/_shared/diagrams.md` (Mermaid rules)
- `~/.claude/skills/_shared/obsidian-export.md` (output)

## 0. Setup
1. Run canary detection per `canary.md`. State the mode (ON with the detected canary / OFF) in one line.
2. If a `CHECKPOINT.md` is given or found in the target output folder, follow the Resume section of `canary.md` and skip to the first remaining phase.
3. Confirm scope: repo paths and/or service names (one or many), and a system slug. Ask only for what is missing.
4. Resolve the output folder from `obsidian-export.md` (ask for the vault path if not saved).

## 1. Discover (read code, don't guess)
For each service: purpose, entry points (APIs, consumers, jobs, CLIs), dependencies (services, DBs, queues, third parties), config and env vars (names only), build/deploy pipeline, tests, observability (logs, metrics, alerts, dashboards), ownership signals (CODEOWNERS, docs, recent git authors).
Record `path:line` evidence for every claim. Unverifiable items go to Open questions.
Drift check after this phase (if canary ON).

## 2. Write notes (one phase per note; drift check after each)
1. **Overview**: what the system does, who uses it, business criticality, service list with one-line roles, glossary terms.
2. **Architecture**: topology flowchart (services, stores, queues, externals, trust boundaries), tech stack, data ownership.
3. **Flows**: sequence diagrams for main happy path(s) and key failure paths; flowcharts for non-trivial business or deploy logic.
4. **Domain Model**: class diagram(s) of core entities with key fields and relationships; invariants worth knowing.
5. **Runbook**: how to run locally, deploy, roll back, rotate secrets (where they live, never values), common alerts and the first three checks for each, dashboards/log queries, on-call and escalation if discoverable.
6. **Risks & Open Questions**: tech debt, fragile spots, known bugs, cliffs (certs/quotas/EOL), in-flight work, undocumented behaviour, questions the outgoing owner must still answer. Rank by severity.
7. **Glossary**.
8. **Index** (MOC) linking everything, with status `draft`.

## 3. Review
Re-check every diagram against code. Remove anything unverified or mark it. Ask the user to review Risks & Open Questions, since only they know the tribal knowledge. Set `status: reviewed` once confirmed.

## 4. Export
Finish via `obsidian-export.md`: list created files and the Index path. If canary ON and the run is complete, delete `CHECKPOINT.md`.

## Rules
- Facts come from code and config you read; label inference as inference.
- Never write secrets or credentials into notes.
- Keep each note scannable: headings, tables, short paragraphs.
