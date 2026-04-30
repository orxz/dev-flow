# Skill Collection Structure Design

## Context

The repository currently behaves like a single installed skill because the git root is
`/Users/rengang/.claude/skills/dev-workflow` and contains a root-level `SKILL.md`.
It also contains nested skill directories:

- `dev-workflow/SKILL.md`
- `laravel-standards/SKILL.md`

The root `SKILL.md` duplicates the nested `dev-workflow/SKILL.md`, which makes the
repository shape ambiguous.

## Decision

Convert the repository into a skill collection repository. The repository root should
be a container, not a skill. Each skill should live in its own direct child directory.

Target structure:

```text
claude-skills/
├── README.md
├── dev-workflow/
│   └── SKILL.md
└── laravel-standards/
    └── SKILL.md
```

## Scope

In scope:

- Remove the duplicate root-level `SKILL.md`.
- Keep `dev-workflow/SKILL.md` as the only `dev-workflow` skill file.
- Keep `laravel-standards/SKILL.md` unchanged.
- Add a root `README.md` that explains this is a multi-skill collection and lists the
  available skills.

Out of scope:

- Rewriting skill content.
- Changing skill names, descriptions, or trigger behavior.
- Renaming the local parent directory from `dev-workflow` to `claude-skills`.
- Creating symlinks for local installation.

## Validation

After the structural change:

- `rg --files` should show no root-level `SKILL.md`.
- `dev-workflow/SKILL.md` and `laravel-standards/SKILL.md` should remain present.
- `git diff` should show only the root duplicate removal and README addition.

## Risks

If another local tool expects the current root path to be a single skill directory,
removing the root `SKILL.md` will stop that specific install path from exposing
`dev-workflow`. That is intentional for the collection layout, but installing or
linking the child skill directories may be needed afterward.
