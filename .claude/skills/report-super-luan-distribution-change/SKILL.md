---
name: report-super-luan-distribution-change
description: In a child project, detect edits to shared (distributed) skills/docs and safely sync them back to super_luan (source of truth), then optionally trigger propagation guidance.
---

# Report Super Luan Distribution Change

## Purpose

When you edit a shared (distributed) skill/doc (e.g. `complete-backlog-item`) inside a **child project**,
this skill defines a safe workflow to sync those changes back into **super_luan (the source of truth)**.

> Principle: the source of truth for distributed files is **super_luan**.  
> Editing in a child first is tolerated as an exception, but you must run this skill to **report/apply** the change.

## When To Use

- You edited any of the following in a child project:
  - A shared skill under `.claude/skills/**/SKILL.md` that is distributed from `super_luan`
  - A shared doc/template (e.g. `docs/00_backlog/*.md`) that is distributed from `super_luan`
- You want the change to become the new baseline for future syncs and/or other children.

## Run Context

This skill is meant to run **inside a child project** located under:

- `../super_luan_children/<child>/`

## Source Of Truth (Mandatory)

Always read, in order:

1. `../../super_luan/data/children_propagation_manifest.md`
2. The table: `자식 로컬 복사 매핑 (sync 기준)`

If a file is not listed in that table, **never** copy it back into `super_luan`.

## What This Skill Does

1. Parse the manifest table `자식 로컬 복사 매핑 (sync 기준)` and build 1:1 pairs of:
   - child local file (destination path in the table)
   - super_luan source file (source path in the table)
2. Check whether each pair differs (diff).
3. Collect only the changed candidates and ask the user **per file**:
   - overwrite `super_luan` with the child version? (y/n)
4. Apply only the approved ones by copying the child file into the matching `super_luan` source path.
5. Report what was applied and what was skipped.

## Safety Rules

- Never guess paths: operate only via `../../super_luan/` and the manifest mappings.
- Even if multiple files changed, always confirm **file-by-file** before overwriting `super_luan`.
- If something changed outside the manifest scope, report it as **out of scope** and do not apply.
- Never include local-only files such as `.claude/settings.local.json`.

## Optional Follow-up

After applying changes into `super_luan`, typically do one of:

- (Recommended) In other child projects, run `/sync-super-luan-distribution` to pull the latest baseline.
- If child `CLAUDE.md` wiring needs adjustment, consider `/propagate-super-luan-rules`.

## Output Format

When finished, respond with:

```markdown
Shared distribution change applied
- child: <child name>
- candidates:
  - <file> (changed)
- applied_to_super_luan:
  - <super_luan path>
- skipped:
  - <file> (reason)
- next:
  - <recommendation>
```

