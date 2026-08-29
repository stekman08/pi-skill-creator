# Pi Skill Creator

A Pi-native skill for creating, reviewing, improving, evaluating, and packaging Agent Skills.

This repository is a fork of [Anthropic's skills repository](https://github.com/anthropics/skills). The `skills/skill-creator/` directory is adapted for Pi. Claude Code-specific runners, commands, paths, and authentication are intentionally not part of the Pi workflow.

## Install in Pi

From this checkout, install the canonical skill with a symlink:

```bash
mkdir -p ~/.pi/agent/skills
ln -sfn "$HOME/.agent-config/pi/skill-creator/skills/skill-creator" \
  "$HOME/.pi/agent/skills/skill-creator"
```

Reload Pi after installing. The source of truth is the checkout in `~/.agent-config/pi/skill-creator`; the link is only Pi's discovery path.

## Use

After reload, ask Pi to create or improve a skill. Examples:

- `Create a skill for reviewing database migrations.`
- `Review the prose and trigger description in ~/.pi/agent/skills/my-skill.`
- `Help me evaluate this skill with realistic test prompts.`
- `Package the finished skill for distribution.`

The skill first clarifies intent when needed, then drafts or inspects the skill, reviews its prose, proposes realistic eval cases, and iterates from evidence. Formal assertions are useful for objectively checkable behavior; subjective writing quality remains a human judgment.

## Evaluation workflow

The skill supports an inline Pi workflow:

1. Save accepted cases in `evals/evals.json`.
2. Run with the skill and, where useful, a baseline without it.
3. Store outputs under `~/.pi/skill-workspaces/<skill-name>/iteration-N/`.
4. Add only meaningful objective assertions.
5. Generate a static review with `eval-viewer/generate_review.py` when visual comparison helps.
6. Iterate only after reviewing the outputs and feedback.

The bundled Python utilities validate structure, aggregate compatible result directories, generate a review, and package skills. Inspect a script before use and do not assume an external runner is available.

## Upstream updates

The fork tracks Anthropics as `upstream`:

```bash
git fetch upstream
git diff HEAD..upstream/main -- skills/skill-creator
git merge upstream/main
```

Review conflicts manually. Preserve Pi-specific adaptations, update this README and `skills/skill-creator/SKILL.md` when upstream changes affect them, then run validation. Do not push changes to `upstream` or create an upstream PR unless explicitly requested.

## Layout

```text
skills/skill-creator/
  SKILL.md
  references/       eval schemas
  eval-viewer/      static human-review page generator
  scripts/          validation, aggregation, packaging
```

## Limitations

Pi does not provide the Claude Code `claude -p` runner used by the upstream implementation. This port does not require Claude Code, Claude authentication, tmux, or Pi's tmux-based subagent plugin. Runs are performed inline unless the user explicitly supplies another compatible runner.
