# Pi Skill Creator

A Pi-native skill for creating, reviewing, improving, evaluating, and packaging [Agent Skills](https://agentskills.io/).

This repository is a Pi adaptation of the [Anthropic skills repository](https://github.com/anthropics/skills). The adaptation keeps the upstream skill-writing guidance while providing a workflow that fits Pi's session and tool model.

## Install

Clone this repository, then link the skill into Pi's global skill directory:

```bash
git clone https://github.com/stekman08/pi-skill-creator.git ~/.agent-config/pi/skill-creator
mkdir -p ~/.pi/agent/skills
ln -sfn "$HOME/.agent-config/pi/skill-creator/skills/skill-creator" \
  "$HOME/.pi/agent/skills/skill-creator"
```

If the repository is already checked out, only the symlink command is needed. Reload Pi after installation.

## Use

Ask Pi to create or improve a skill. For example:

- `Create a skill for reviewing database migrations.`
- `Review the prose and trigger description in ~/.pi/agent/skills/my-skill.`
- `Help me evaluate this skill with realistic test prompts.`
- `Package the finished skill for distribution.`

The workflow clarifies intent when needed, drafts or inspects the skill, reviews its prose and trigger description, proposes realistic evaluation cases, and iterates from evidence.

## Evaluation workflow

1. Save accepted cases in `evals/evals.json`.
2. Run the cases with the skill and, when useful, a baseline without it.
3. Store outputs under `~/.pi/skill-workspaces/<skill-name>/iteration-N/`.
4. Add only meaningful assertions for objectively checkable outcomes.
5. Generate a static review with `eval-viewer/generate_review.py` when visual comparison helps.
6. Review the outputs and feedback before changing the skill.
7. Repeat with a new iteration directory when the change is substantial.

Qualitative review remains important for prose, usability, and judgment-heavy results. Quantitative scores should support that review, not replace it.

## Repository layout

```text
skills/skill-creator/
  SKILL.md
  references/       evaluation schemas
  eval-viewer/      static human-review page generator
  scripts/          validation, aggregation, and packaging tools
```

## Updating from upstream

The local checkout has the upstream repository configured as the `upstream` remote. To review and integrate new upstream changes:

```bash
git fetch upstream
git diff HEAD..upstream/main -- skills/skill-creator
git merge upstream/main
```

Review the resulting diff carefully. Preserve intentional Pi adaptations, update the README and `SKILL.md` when needed, run validation, and commit the result to this fork. The upstream remote is read-only for this project; upstream changes are not published from this repository.

## Scope

This repository retains the upstream skill collection needed for comparison and future selective updates, while `skills/skill-creator/` is the maintained Pi adaptation. Only that directory is installed by the symlink above.
