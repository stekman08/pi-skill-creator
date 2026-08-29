---
name: skill-creator
description: Create, review, improve, evaluate, and package Agent Skills for the Pi coding agent. Use when the user wants a new SKILL.md, wants to improve an existing skill, needs help with skill prose or trigger descriptions, wants evals or qualitative review, or wants to package and install a reusable capability. Also use when a skill feels vague, brittle, overlong, surprising, or is not triggering reliably.
compatibility: Requires Pi and Python 3.10+ for the optional evaluation scripts. Browser review is optional and can use the generated static HTML file.
---

# Skill Creator

Help the user create skills that are useful, unsurprising, maintainable, and pleasant for an agent to follow. Treat the skill as a small product: first understand the behavior it should enable, then write a lean instruction set, test it with realistic requests, review the results, and iterate.

Do not optimize for length alone. Remove instructions that do not change behavior, explain important reasons instead of piling up rigid commands, and keep details close to the point where they are needed.

## Choose the right workflow

Identify where the user is:

- **New skill**: interview first, then draft the skill and propose test prompts.
- **Existing skill**: inspect it and its supporting files before suggesting changes. Preserve its purpose and name unless the user asks otherwise.
- **Prose or trigger review**: review the description and body separately. The description controls discovery; the body controls behavior after loading.
- **Evaluation**: use realistic cases and assertions only where the outcome is objectively checkable. Use human review for judgment-heavy writing or design.
- **Packaging**: validate the skill, then package or install it only after the user accepts the result.

When the request is already specific, extract answers from the conversation instead of asking questions that have already been answered. Ask only for decisions that affect the result.

## Capture intent

Before writing a new skill, establish:

1. What capability should the skill provide?
2. Which user requests and contexts should load it?
3. Which similar requests should not load it?
4. What should the agent produce or change?
5. What dependencies, tools, files, or safety boundaries matter?
6. How will we know it worked, and does the user want formal evals?

For an existing skill, begin with an inventory of its files, current behavior, known failures, and user corrections.

## Research and draft

Research relevant documentation or examples when the task depends on a framework, format, or external tool. Prefer authoritative sources. Do not copy incidental assumptions from another harness; verify that they apply to Pi.

Write the smallest complete skill:

- Use valid frontmatter. `name` must match the parent directory and contain only lowercase letters, numbers, and hyphens. Keep `description` under 1024 characters.
- Put both purpose and trigger context in `description`. Make it specific enough to distinguish near-misses, but do not turn it into a keyword dump.
- Put operational instructions in the body, not in the description.
- Mention required tools and dependencies accurately.
- Link to bundled references and scripts, and say when to read or run them.
- Prefer progressive disclosure: metadata for discovery, the body for the normal workflow, and references/scripts for detail or deterministic work.

### Prose and instruction quality

Read the draft with fresh eyes before presenting it. Check that:

- Each section earns its context cost and changes a decision or behavior.
- The sequence follows the user's real workflow rather than the author's implementation history.
- Instructions explain why when the reason helps the agent generalize.
- Strong wording is reserved for safety, correctness, or an explicit user requirement. Avoid reflexive `MUST`, `ALWAYS`, and `NEVER` when a rationale is clearer.
- Examples illuminate a pattern and do not accidentally narrow the skill to one case.
- Output formats are explicit when consistency matters.
- The skill is honest about uncertainty, dependencies, and what it does not cover.
- The contents would not surprise a user or enable harmful, deceptive, or unauthorized behavior.
- The skill is general enough to work beyond the initial examples and does not encode a brittle workaround as a universal rule.

## Test cases and evaluation

After drafting, propose 2-3 realistic prompts and let the user correct them when practical. Include positive cases, a near-miss, and an edge case when they reveal different behavior. Save accepted cases as `evals/evals.json` using the schema in `references/schemas.md`.

Run cases inline in the current Pi session unless the user explicitly provides another runner. For each case:

1. Read the candidate skill and follow it as the agent would.
2. Record the output, decisions, tool use, and any user-visible result under `~/.pi/skill-workspaces/<skill-name>/iteration-N/`.
3. Run a baseline without the skill when comparison is useful.
4. Draft a small number of objective assertions. Do not force subjective prose or design judgments into fake numeric scores.
5. Review the actual outputs and, when useful, generate the static HTML review with `eval-viewer/generate_review.py` so the user can assess them.
6. Improve the skill based on evidence, not on imagined failures, then repeat with a new iteration directory.

Always separate three questions:

- **Behavior**: did the skill produce the intended result?
- **Quality**: was the result clear, robust, and unsurprising?
- **Discovery**: did the description load the skill for the right requests and avoid near-misses?

Treat trigger optimization as a separate step after the body is sound. Build a reviewed set of realistic positive and negative queries before optimizing a description. Report both precision and recall and watch for overfitting.

## Tools and resources

This skill may use the bundled Python scripts for validation, aggregation, packaging, and static review. Inspect a script before relying on it. If a bundled script assumes another harness, do not silently use it: adapt it or explain the limitation.

Use these resources as needed:

- `references/schemas.md` for eval and result formats.
- `eval-viewer/generate_review.py` for a human-reviewable static report.
- `scripts/quick_validate.py` for frontmatter and structure checks.
- `scripts/package_skill.py` for packaging a validated skill.
- `scripts/aggregate_benchmark.py` for aggregating compatible result directories.

Do not require external agent orchestration or another coding-agent CLI. Pi can complete the normal workflow in the main session. If a separate agent or compatible runner is available, treat it as an optional enhancement and preserve an inline fallback.

## Finish and install

Before calling a skill finished:

1. Re-read the complete `SKILL.md` and its README.
2. Validate the name, description length, references, scripts, and directory structure.
3. Check for stale harness-specific terms, commands, paths, or promises.
4. Run the relevant tests or validation commands and report what was not tested.
5. Show the user the important behavior and prose changes.
6. Install only after acceptance. For this repository, the canonical skill lives at `skills/skill-creator/`; a Pi installation may symlink that directory to `~/.pi/agent/skills/skill-creator`.

A skill is finished when it is useful in practice, its limitations are documented, and the user can reproduce its installation and evaluation.
