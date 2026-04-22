---
name: lets-commit
description: Applies reusable commit conventions for software and documentation projects. Use when preparing commits, grouping changes, or drafting commit messages in super_luan and child projects.
---

# Let's Commit

## Purpose
Provide a reusable commit policy for both super_luan and child projects (including real coding repositories).

## Message Format
- Use: `<타입>: <한 줄 요약>`
- Example: `feat: add OAuth login callback handler`
- Keep the summary focused on why the change exists.

## Preferred Types (General)
- `feat`: new user-facing feature
- `fix`: bug fix
- `refactor`: internal restructuring without behavior change
- `test`: test addition/update
- `docs`: documentation change
- `chore`: maintenance/tooling/dependency updates
- `perf`: performance improvement
- `build`: build system or CI/CD changes

## Optional Domain Types
When a repository already uses domain-specific types, follow local conventions.
Examples used in super_luan:
- `idea`, `persona`, `script`, `init`, `tree`

## Commit Policy
1. Split by task as much as practical.
2. One commit = one reason.
3. Do not mix unrelated purposes.

## Repository scope (default)

- When the user runs `/lets-commit` in **super_luan**, **only commit inside the `super_luan` git repository**.
- **Do not** stage, commit, or otherwise modify child projects under `../super_luan_children/` unless the user **explicitly** asks to commit a specific child repo in the same message.
- Child worktrees are separate repositories: treat them as out of scope for default lets-commit runs.

## Grouping Heuristic
- If files changed for the same reason, one commit.
- If reasons differ (e.g., docs + refactor + feature), split commits.
- If one change enables another, keep them together only when separation breaks traceability.

## Pre-Commit Checklist
- commit scope is single-purpose
- type matches dominant change reason
- summary line explains why this change exists
- related references/docs/tests are updated when needed

