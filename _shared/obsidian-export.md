# Obsidian export

Run as the last step, after all notes are written and reviewed.

## Vault location
1. Read `~/.claude/skills/_shared/obsidian.json` (`{"vault_path": "...", "handover_dir": "Handovers", "onboarding_dir": "Onboarding"}`).
2. If missing, ask the user once for the vault path (verify the folder exists and contains `.obsidian/`), then save the file. Never guess a path.

## Output
Write plain Markdown files directly into the vault; no plugin required.

```
<vault>/<handover_dir|onboarding_dir>/<system-slug>/
  00 Index.md          # MOC: links to every note, status, date
  01 Overview.md
  02 Architecture.md   # topology diagram
  03 Flows.md          # sequence + flowcharts
  04 Domain Model.md   # class diagrams
  05 Runbook.md        # (handover) / 05 First Week.md (onboarding)
  06 Risks & Open Questions.md
  Glossary.md
```

## Note conventions
- Frontmatter on every note: `type` (handover|onboarding), `system`, `date`, `tags: [<type>, <system-slug>]`, `status` (draft|reviewed).
- Link notes with `[[wikilinks]]`; Index links to all; each note links back to Index.
- Diagrams stay as ```mermaid blocks. Reference code as `repo/path:line` (inline code, not links).
- Never write secrets, tokens or credentials into notes. Name where they live, not their values.
- If the target folder already exists, do not overwrite: ask whether to update in place or create a dated copy.
- Finish by listing the created files and the vault-relative path to `00 Index.md`.

## Source of truth
Work in the vault folder directly when canary mode is OFF. When ON, work in the same folder and keep `CHECKPOINT.md` there; delete `CHECKPOINT.md` after a fully successful run.
