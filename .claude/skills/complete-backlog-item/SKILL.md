---
name: complete-backlog-item
description: Marks a backlog item as complete in a child project. Moves the item from backlog.md (진행) to backlog_done.md with a completion date; moves linked planning docs from docs/10_planning/backlog/ to docs/10_planning/backlog/complete/ when applicable; updates related docs if needed. Use when a task is finished in a child project.
---

# Complete Backlog Item

## Purpose
Consistently execute the backlog item completion workflow within a child project.

## Workflow

### 1. Identify the item to complete
- If no item is specified: read the **진행 (In Progress)** section of `docs/00_backlog/backlog.md`, show the list, and ask the user to select one.
- If an item is specified: locate it in the In Progress section.

### 2. Update backlog.md
- Remove the item from the In Progress section of `docs/00_backlog/backlog.md`.

### 3. Update backlog_done.md
- Add the item to the Done section of `docs/00_backlog/backlog_done.md`.
- Preserve the item’s `ID` field unchanged.
- Format: `- [x] <item content> (YYYY-MM-DD)`

### 4. Move completed planning documents (this project)
- Per `CLAUDE.md`: backlog planning docs live under `docs/10_planning/backlog/`; when the item is done, move the corresponding file(s) to `docs/10_planning/backlog/complete/`.
- Resolve which file(s) to move from the item’s **추가 문서** field, from filenames matching the item `ID` (e.g. `FP2-013_*.md`), or by asking the user if unclear.
- Create `docs/10_planning/backlog/complete/` if it does not exist.
- Prefer `git mv` so history is preserved.
- If there is no planning doc under `docs/10_planning/backlog/`, skip this step.

### 5. Update related documents (optional)
- If there are other documents related to the completed item, ask the user whether they need to be updated.

## Priority Levels
- `critical` — must be done immediately, blocks other work
- `high` — important, do soon
- `medium` — normal priority
- `low` — nice to have, no urgency

## Backlog File Paths
- Active backlog: `docs/00_backlog/backlog.md`
- Done archive: `docs/00_backlog/backlog_done.md`
- Planning backlog (active): `docs/10_planning/backlog/`
- Planning backlog (completed): `docs/10_planning/backlog/complete/`

## Notes
- Backlog is managed entirely within the child project. Do not reflect completions back to super_luan.
- If completing an item that is still in the 대기 (Waiting) section, move it directly to done without staging it as In Progress first.
- Other child projects may use different planning paths; follow that project’s `CLAUDE.md` if it differs from the paths above.
