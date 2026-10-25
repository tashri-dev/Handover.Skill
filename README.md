# Skills

Claude Code skills for documenting software systems. Two skills share one set of conventions for diagrams, drift protection and output.

| Skill | Audience | Goal |
|---|---|---|
| [`handover`](handover/SKILL.md) | Incoming owner | Operate, change and debug the system without the outgoing owner |
| [`onboarding`](onboarding/SKILL.md) | Newcomer | Understand the system's shape, navigate the code, ship a first safe change |

## Why

- **Knowledge leaves with people.** A handover written from memory misses things. These skills read the code and config, and cite `path:line` for every claim. Anything unverifiable goes to *Open questions* and is never presented as fact.
- **Newcomers and successors need different docs.** A successor needs a runbook and a risk list. A newcomer needs a reading order and a first-week plan. Each skill targets one reader.
- **Long runs lose context.** An optional canary protocol detects when the agent drifts and writes a resumable checkpoint.
- **Notes should live where you read them.** Output is plain Markdown with Mermaid diagrams, written straight into an Obsidian vault.

## What each skill produces

Both write the same folder layout into your vault:

```
<vault>/<Handovers|Onboarding>/<system-slug>/
  00 Index.md                     # map of content, status, date
  01 Overview.md
  02 Architecture.md              # topology diagram
  03 Flows.md                     # sequence diagrams + flowcharts
  04 Domain Model.md              # class diagrams
  05 Runbook.md                   # handover
  05 First Week.md                # onboarding
  06 Risks & Open Questions.md
  Glossary.md
```

**handover** covers purpose, entry points, dependencies, config (names only), deploy pipeline, observability and ownership signals. The runbook explains how to run, deploy, roll back and rotate secrets, plus the first three checks for each common alert. The risks note ranks tech debt, cliffs (certs, quotas, EOL) and questions the outgoing owner must still answer.

**onboarding** covers the same discovery, explained for the newcomer's background. Each diagram is followed by a plain-language walkthrough. The First Week note gives a day-by-day path: set up, run, test, trace one request, read named files in order, take a starter task, and self-check questions. If a handover for the system already exists in the vault, it builds on it and links to it.

Every note has frontmatter (`type`, `system`, `date`, `tags`, `status`) and `[[wikilinks]]` back to the Index. Status starts as `draft` and becomes `reviewed` once you confirm.

## How to use

### Install

Skills load from `~/.claude/skills/`. The skills reference `~/.claude/skills/_shared/`, so keep that folder alongside them.

```bash
git clone <this-repo> ~/.claude/skills
```

Or symlink the skill folders and `_shared` into `~/.claude/skills/`.

### Invoke

Describe the task, or call the skill by name:

```text
/handover                       # hand a service over to someone else
/onboarding                     # learn a system you've just inherited
```

Phrases that trigger them:

- **handover**: "handover", "I'm leaving / transferring this service", "document this for the next owner"
- **onboarding**: "onboard me", "help a new joiner understand this", "explain this system to a newcomer"

Give a repo path or service name (one or many) and a system slug. Each skill asks only for what is missing. Onboarding also asks about the newcomer's role and languages, and defaults to a generalist backend engineer.

### Workflow

1. **Setup**: detect the canary, confirm scope, resolve the output folder.
2. **Discover**: read code and config, recording `path:line` evidence.
3. **Write notes**: one phase per note, with a drift check after each.
4. **Review**: diagrams are re-checked against code. For handover, you review *Risks & Open Questions*, because only you know the tribal knowledge.
5. **Export**: files are listed with the path to `00 Index.md`.

### Vault configuration

On first run the skill asks for your vault path, checks that the folder contains `.obsidian/`, and saves it to `~/.claude/skills/_shared/obsidian.json`:

```json
{ "vault_path": "/path/to/vault", "handover_dir": "Handovers", "onboarding_dir": "Onboarding" }
```

If the output folder already exists, the skill asks whether to update in place or create a dated copy. It never overwrites silently.

### Canary and resume (optional)

If your `AGENTS.md` or `CLAUDE.md` requires a marker in every reply (for example `begin every response with [On Track] Taha,`), drift protection turns on. If a reply loses the marker, the skill writes `CHECKPOINT.md` to the output folder and gives you a one-line resume prompt:

```text
Resume handover from <path-to-CHECKPOINT.md>
```

With no canary configured, protection is off and the skill runs straight through. `CHECKPOINT.md` is deleted after a fully successful run.

## Repository layout

```
handover/SKILL.md          # handover skill
onboarding/SKILL.md        # onboarding skill
_shared/
  canary.md                # drift detection, checkpoint, resume
  diagrams.md              # Mermaid rules and minimum diagram set
  obsidian-export.md       # vault location, folder layout, note conventions
```

## Rules both skills follow

- Facts come from code and config that was read. Inference is labelled as inference.
- Secrets and credentials are never written to notes. Notes name where they live, not their values.
- Minimum diagrams per system: a topology flowchart, a happy-path sequence, a failure-path sequence or flowchart, and a core-domain class diagram. Every diagram is derived from code, never invented.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for scope, skill structure, testing and PR guidelines.

## License

Released under the [MIT License](LICENSE).
