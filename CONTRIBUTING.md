# Contributing

Thanks for helping improve these skills. This guide explains what belongs here, how a skill is structured, and how to get a change merged.

## Scope

This repo holds Claude Code skills for documenting software systems, plus the shared references they use.

In scope:
- Fixes and improvements to `handover` and `onboarding`.
- New skills that fit the same family (read code, write verified notes, export to Obsidian).
- Improvements to the shared references in `_shared/`.
- README and docs fixes.

Out of scope: skills unrelated to documenting systems, and anything that requires secrets or private data to work.

## Repository layout

```
<skill-name>/SKILL.md      # one folder per skill
_shared/
  canary.md                # drift detection, checkpoint, resume
  diagrams.md              # Mermaid rules and minimum diagram set
  obsidian-export.md       # vault location, folder layout, note conventions
```

A change to `_shared/` affects every skill. Check each skill that references the file before opening a PR.

## Adding or editing a skill

### Frontmatter

Every `SKILL.md` starts with:

```yaml
---
name: <folder-name>
description: <what it produces, when to use it, trigger phrases, and when NOT to use it>
---
```

The `description` decides when Claude picks the skill. List the phrases users actually say, and point to the neighbouring skill for cases it doesn't cover (as `handover` and `onboarding` do for each other).

### Structure

Follow the existing skills:

1. A one-line **audience** and **goal**.
2. **Shared references** the skill reads first.
3. Numbered phases: **Setup → Discover → Write notes → Review → Export**.
4. A short **Rules** section.

Keep it imperative and scannable: headings, numbered steps, short paragraphs. Don't duplicate content that lives in `_shared/`; link to it.

### Conventions all skills must keep

- **Evidence over guessing.** Facts come from code and config that was read, recorded as `path:line`. Inference is labelled as inference. Unverifiable items go to *Open questions*.
- **No secrets.** Notes name where credentials live, never their values.
- **Diagrams** follow `_shared/diagrams.md`: Mermaid, derived from code, one idea per diagram, and the minimum set per system.
- **Output** follows `_shared/obsidian-export.md`: the folder layout, the frontmatter fields, `[[wikilinks]]` to the Index, and no silent overwrites.
- **Drift protection** stays opt-in and follows `_shared/canary.md`.

## Testing a change

There is no automated test suite. Verify by running the skill:

1. Install your branch into `~/.claude/skills/` (or symlink it).
2. Run the skill against a small real repo, using a throwaway vault.
3. Check the output:
   - All expected notes exist with the right names and frontmatter.
   - Every diagram renders in Obsidian and matches the code.
   - Claims carry `path:line` evidence and no secrets appear.
   - With a canary configured, a forced drift writes a usable `CHECKPOINT.md` and resume works.
4. Note what you ran in the PR description.

## Submitting changes

1. Open an issue first for new skills or large changes, so scope is agreed before you build.
2. Fork the repo and create a branch from `main`.
3. Make small, focused commits with clear messages (for example `handover: add rollback checks to runbook`).
4. Open a pull request with:
   - **What** changed.
   - **Why** it is needed.
   - **How** you tested it.
5. Respond to review comments by pushing new commits to the same branch.

### Review checklist

- [ ] Frontmatter `name` matches the folder and `description` states when to use and when not to.
- [ ] Skill follows the phase structure and the Rules section.
- [ ] No duplicated content that belongs in `_shared/`.
- [ ] Every skill affected by a `_shared/` change still works.
- [ ] No secrets, personal paths or private data in examples.
- [ ] `README.md` updated if the change adds a skill or alters behaviour.

## Reporting bugs

Open an issue with the skill name, the command or phrase you used, what you expected, what happened, and the relevant output (with secrets removed).

## Code of conduct

Be respectful and constructive. Assume good intent, critique the work and not the person, and keep discussion on topic. Maintainers may remove comments or contributions that don't meet this standard.

## License

By contributing, you agree your contributions are licensed under the repository's [MIT License](LICENSE).
