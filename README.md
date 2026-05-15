# dev-workflow

A Claude skill that routes non-trivial development tasks through a decision tree of 7 workflow paths, from evaluation through TDD, review, documentation, and shipping.

## What It Does

Receives a task and routes it through the correct flow:

| Flow | When |
|------|------|
| A: New feature | Brand new functionality |
| B: Bug fix | Expected behavior exists but output is wrong |
| C: Feature rewrite | Existing code needs to be replaced |
| D: Refactor | Internal implementation change, behavior unchanged |
| E: Evaluation | Analysis only, no code changes |
| F: Emergency fix | Production is down, fix first / test later |
| G: Dependency upgrade | Package/framework version bump |

Each flow enforces: plan before coding, TDD, per-task review, documentation sync, and progress persistence.

## Installation

This repository is the skill. Link it into your Claude skills directory:

```bash
ln -s "$(pwd)" ~/.claude/skills/dev-workflow
```

Or copy the directory directly if you prefer:

```bash
cp -r . ~/.claude/skills/dev-workflow
```

## Optional Tool Suites

The skill works standalone. Three optional tool suites provide enhanced capabilities:

- **gstack** — Architecture review, code review, root cause investigation, shipping. Skills: `/plan-eng-review`, `/review`, `/investigate`, `/ship`, `/document-release`, `/office-hours`, `/plan-ceo-review`, `/freeze`
- **Superpowers** — Structured implementation planning, TDD workflow. Skills: `writing-plans`, `executing-plans`, `subagent-driven-development`, `requesting-code-review`, `test-driven-development`
- **planning-with-files** — Progress persistence across sessions. Skills: `planning-with-files-zh`

When a tool suite is not installed, the skill falls back to manual equivalents at each step.
