# Canary protocol (opt-in drift protection)

A canary is a marker the user's own config requires in every agent reply. If a reply lacks it, the agent has lost the instructions (context drift). Skills use it as a drift signal. It is **optional**: no canary configured means no drift protection.

## 1. Detect (run first, every invocation)

Read, in order: project `AGENTS.md`, project `CLAUDE.md`, `~/.claude/AGENTS.md`, `~/.claude/CLAUDE.md`.
Find any rule that asks for a marker in responses (look for "canary", "begin every response with", "end every response with", "sign off with", "tag", or similar).

Do not assume a format. A canary may be a prefix, suffix, emoji, tag, name, phrase, or a combination, with any value. Interpret the rule as written and record:

- `canary_text`: exact text the user wants (as literal as the rule gives it; if the rule gives a template, record the template and the concrete value you have been using)
- `position`: prefix | suffix | anywhere | other
- `source`: file the rule came from

If several rules conflict, the project file wins over the global file; tell the user which one you picked.

## 2. Modes

- **Canary found → drift protection ON.** Follow sections 3-4.
- **No canary found → OFF.** Run the skill straight through. No checkpoint file, no drift checks. Mention once, in one line, that a canary can be added to AGENTS.md to enable resumable runs. Do not repeat.

Always emit the canary in your own replies when one exists (that is the user's rule, independent of this skill).

## 3. Drift check (ON only)

After finishing each section/phase, before starting the next, re-read your last reply. Drift = the canary is missing, malformed, or in the wrong position. Also treat as drift: forgetting the skill's own steps, scope, or output paths.

On drift:
1. Write/overwrite `CHECKPOINT.md` (format below) in the output folder.
2. Tell the user, using the canary, that context drifted and recommend either compacting the session or starting a new one.
3. Give a one-line resume prompt: `Resume <skill-name> from <path-to-CHECKPOINT.md>`.
4. If a session-spawning tool is available (e.g. the desktop app's start_session), offer to open the continuation session; otherwise stop.

Also write a checkpoint proactively before any very long phase (many repos/services), so a crash is recoverable.

## 4. Resume

When invoked with a `CHECKPOINT.md` (path given or found in the output folder):
1. Re-run detection (section 1). Do not trust `canary_text` in the file; the live config wins. If the canary is now absent, switch to OFF mode and continue without protection.
2. Read the checkpoint, skip sections marked done, continue at `next_action`.
3. Do not redo done work unless the user asks.

## Checkpoint format

```markdown
---
skill: handover | onboarding
canary_text: "<text>"
position: prefix
source: <file>
scope: <repos/services covered>
output_dir: <absolute path>
updated: <ISO date>
---
## Done
- [x] <section> → <file>
## Remaining
- [ ] <section>
## Next action
<one concrete step>
## Key findings so far
<facts that must survive a context reset: service names, entry points, decisions, open questions>
```
