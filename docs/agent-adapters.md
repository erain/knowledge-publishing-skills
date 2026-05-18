# Agent Adapters

These skills are intentionally lightweight: each one is a Markdown instruction
file plus optional scripts and assets. Any coding agent can use them if it can
read repository files and run shell commands.

## Shared Pattern

1. Make this repository available to the agent.
2. Tell the agent which skill to use.
3. Ask the agent to read `skills/<skill-name>/SKILL.md`.
4. Let the agent run bundled scripts from the skill directory when validation or
   publishing is needed.

Recommended prompt:

```text
Use the skill at skills/knowledge-to-slides/SKILL.md to create a self-contained
HTML deck from what we learned. Validate it with the bundled script.

Then use skills/publish-html-artifact/SKILL.md to publish it, commit the hosting
repo changes, push, and return the live URL.
```

## Codex

Codex can load these as native skills by symlinking each skill folder into
`~/.codex/skills`:

```sh
mkdir -p ~/.codex/skills
ln -s "$(pwd)/skills/knowledge-to-slides" ~/.codex/skills/knowledge-to-slides
ln -s "$(pwd)/skills/publish-html-artifact" ~/.codex/skills/publish-html-artifact
```

After that, prompts can refer to `$knowledge-to-slides` and
`$publish-html-artifact`.

## OpenClaw

Use the repository as a skill source or context folder. If the agent supports a
skill path, point it at:

```text
skills/knowledge-to-slides/SKILL.md
skills/publish-html-artifact/SKILL.md
```

If it does not have native skill loading, include the same paths in the task
prompt and ask OpenClaw to follow them as operating instructions.

## pi

Attach or mount this repository in the workspace, then instruct pi to read the
needed `SKILL.md` file before generating artifacts. The bundled scripts use only
standard Python and shell tools, so pi can execute the workflow directly.

## opencode

Add this repository to the working context, or copy the skill directories into
the location where opencode reads project instructions. Use `AGENTS.md` for
repo-level conventions and `SKILL.md` for the concrete workflow.

## Generic Agents

For any other agent, the minimum contract is:

- Read the selected `SKILL.md`.
- Produce or update files in the current workspace.
- Run validation scripts when requested by the skill.
- Use Git to commit and push when publishing is requested.

No agent-specific API is required.
