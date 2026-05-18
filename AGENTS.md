# Agent Notes

This repository is a portable skill pack, not an application runtime. Treat each
`skills/<name>/SKILL.md` file as the primary instruction entry point for that
skill, and treat bundled scripts/assets as implementation aids.

## Editing Guidelines

- Keep skill instructions agent-neutral. Mention Codex, OpenClaw, pi, opencode,
  or other agents only in adapter documentation.
- Do not duplicate large workflow text across files. Put core behavior in
  `SKILL.md` and installation/adapter guidance in `README.md` or `docs/`.
- Keep skill directories small and purposeful: `SKILL.md`, optional `scripts/`,
  optional `assets/`, optional `agents/`.
- Use deterministic Python or shell helpers when the workflow benefits from
  repeatable validation or publishing.
- When changing the knowledge site URL, default hosting repo, or visibility
  semantics, update `README.md`, the relevant `SKILL.md`, and publishing scripts
  together.

## Validation

Before committing, run:

```sh
python3 -m py_compile \
  skills/knowledge-to-slides/scripts/validate_deck.py \
  skills/publish-html-artifact/scripts/publish.py

python3 /home/ubuntu/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  skills/knowledge-to-slides

python3 /home/ubuntu/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  skills/publish-html-artifact

git diff --check
```

If the Codex skill validator is unavailable, still verify frontmatter, script
syntax, and repository diffs manually.
