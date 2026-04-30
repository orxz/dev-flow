# claude-skills

Skill collection for reusable Claude/Codex workflows.

## Skills

- `dev-workflow` - Routes non-trivial development work through planning, execution, review, documentation, and shipping workflows.
- `laravel-standards` - Provides Laravel engineering standards for layered architecture, API design, error handling, caching, performance, and task lifecycle discipline.

## Layout

Each skill lives in its own directory:

```text
skill-name/
└── SKILL.md
```

The repository root is a collection container, not an installable skill.

## Local Installation

From the repository root, install or link individual skill directories into the
skill root used by your agent:

```bash
ln -s "$(pwd)/dev-workflow" ~/.claude/skills/dev-workflow
ln -s "$(pwd)/laravel-standards" ~/.claude/skills/laravel-standards
```

The `$(pwd)` form writes absolute symlink targets so they keep working from any
shell location.
