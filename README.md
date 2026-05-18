# Knowledge Publishing Skills

Portable coding-agent skills for turning engineering work into polished HTML
knowledge artifacts, then publishing them to a static knowledge site.

The skills are plain Markdown instructions plus small Python helpers. They are
designed to be usable by Codex, OpenClaw, pi, opencode, or any other coding
agent that can read files, edit a repository, run shell commands, and commit
changes.

## What This Gives An Agent

- Capture what happened in a coding session, PR, incident, experiment, or
  technical discussion.
- Turn that knowledge into a self-contained HTML slide deck or page with inline
  SVG/CSS visuals.
- Validate that the artifact is publishable without localhost, local paths, or
  remote JavaScript dependencies.
- Publish the finished HTML into `erain/knowledge-pages`, commit it, push it,
  and return a live URL on `https://knowledge.11tech.xyz`.

## Skills

| Skill | Purpose | Entry Point |
| --- | --- | --- |
| `knowledge-to-slides` | Create a polished, self-contained HTML slide deck. | `skills/knowledge-to-slides/SKILL.md` |
| `publish-html-artifact` | Publish a finished HTML artifact to the knowledge site. | `skills/publish-html-artifact/SKILL.md` |

Each skill directory is self-contained:

- `SKILL.md` is the agent-readable workflow.
- `scripts/` contains deterministic helper scripts.
- `assets/` contains reusable templates or static inputs.
- `agents/` contains optional adapter metadata for platforms that support it.

## Use With Any Coding Agent

For agents with native skill support, install or symlink the skill folders into
the agent's skill directory.

For agents without native skill support, point the agent at this repository and
ask it to read the relevant `SKILL.md` file before working. The skill files are
written as direct operating instructions, so they do not require a Codex-specific
runtime.

Example prompt:

```text
Use knowledge-publishing-skills.
First read skills/knowledge-to-slides/SKILL.md and turn what we just learned
into a polished HTML slide deck. Then read skills/publish-html-artifact/SKILL.md,
publish the deck, commit the hosting repo changes, push, and give me the URL.
```

See [docs/agent-adapters.md](docs/agent-adapters.md) for Codex, OpenClaw, pi,
opencode, and generic-agent setup patterns.

## Codex Install

```sh
git clone https://github.com/erain/knowledge-publishing-skills
cd knowledge-publishing-skills
mkdir -p ~/.codex/skills
ln -s "$(pwd)/skills/knowledge-to-slides" ~/.codex/skills/knowledge-to-slides
ln -s "$(pwd)/skills/publish-html-artifact" ~/.codex/skills/publish-html-artifact
```

## Requirements

- Python 3.11+
- Git
- GitHub authentication for pushing published artifacts
- A local checkout of `erain/knowledge-pages` when using
  `publish-html-artifact`

The default publishing target is:

- Repository: `/home/ubuntu/src/knowledge-pages`
- Public base URL: `https://knowledge.11tech.xyz`

Both can be overridden with `publish.py` arguments.

## Visibility

`publish-html-artifact` supports two publication modes:

- `public`: appears in the knowledge site gallery.
- `unlisted`: deploys under `/u/<slug-or-token>/` and does not appear in the
  gallery.

Unlisted URLs are convenience links, not access control. Publish only content
that is acceptable to be visible to anyone who obtains the URL.

## Maintainer Notes

Keep this repository portable:

- Avoid agent-specific assumptions in `SKILL.md`.
- Keep helper scripts deterministic and shell-friendly.
- Prefer inline HTML/CSS/SVG artifacts over external runtime dependencies.
- Validate skills and scripts before pushing changes.
