# Skill Collection Structure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert the repository root from a duplicate single-skill directory into a clean multi-skill collection.

**Architecture:** The repository root becomes documentation and container metadata only. Each skill remains a direct child directory with its own `SKILL.md`, so discovery and installation can target individual skill folders.

**Tech Stack:** Markdown files, Git file structure, Codex/Claude skill folder conventions.

---

### Task 1: Add Collection README

**Files:**
- Create: `README.md`

- [ ] **Step 1: Create the README**

Create `README.md` with this exact content:

```markdown
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
```

- [ ] **Step 2: Verify README exists**

Run: `test -f README.md && sed -n '1,120p' README.md`

Expected: command exits with status 0 and prints the README content above.

- [ ] **Step 3: Commit checkpoint if executing one task per commit**

```bash
git add README.md docs/superpowers/plans/2026-04-30-skill-collection-structure.md
git commit -m "docs: document skill collection layout"
```

### Task 2: Remove Duplicate Root Skill

**Files:**
- Delete: `SKILL.md`
- Keep: `dev-workflow/SKILL.md`
- Keep: `laravel-standards/SKILL.md`

- [ ] **Step 1: Delete the duplicate root skill file**

Remove `SKILL.md` from the repository root. Do not edit `dev-workflow/SKILL.md` or `laravel-standards/SKILL.md`.

- [ ] **Step 2: Verify child skills remain**

Run: `test -f dev-workflow/SKILL.md && test -f laravel-standards/SKILL.md`

Expected: command exits with status 0.

- [ ] **Step 3: Verify root skill is gone**

Run: `test ! -f SKILL.md`

Expected: command exits with status 0.

- [ ] **Step 4: Commit checkpoint if executing one task per commit**

```bash
git add -A
git commit -m "refactor: make repository a skill collection"
```

### Task 3: Final Structure Validation

**Files:**
- Inspect: repository root

- [ ] **Step 1: List tracked files**

Run: `rg --files`

Expected output includes:

```text
README.md
docs/superpowers/plans/2026-04-30-skill-collection-structure.md
docs/superpowers/specs/2026-04-30-skill-collection-structure-design.md
dev-workflow/SKILL.md
laravel-standards/SKILL.md
```

Expected output does not include:

```text
SKILL.md
```

- [ ] **Step 2: Check branch status**

Run: `git status --short --branch`

Expected: branch is `codex/skill-collection-structure`; working tree is clean after commits, or only intentional unstaged changes are visible before the final commit.

- [ ] **Step 3: Inspect diff against main**

Run: `git diff main...HEAD --stat`

Expected: diff shows the design doc, implementation plan, README addition, and root `SKILL.md` deletion.
