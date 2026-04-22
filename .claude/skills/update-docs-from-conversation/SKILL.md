---
name: update-docs-from-conversation
description: Updates super_luan documents from the current conversation context when explicitly requested. Use when the user says to sync, reflect, or batch-update docs based on this chat/session.
---

# Update Docs From Conversation

## Purpose
Run a deliberate, explicit document sync from the current chat/session into `super_luan` docs.

## Explicit Invocation
Only run this skill when the user explicitly asks, for example:
- "지금까지 대화 기준으로 문서 업데이트해줘"
- "이번 세션 내용 반영해줘"
- "대화 요약해서 super_luan 문서에 반영해줘"

## Source Scope
Use only:
1. Current conversation content
2. Files edited/read during this session
3. Existing project rules in `CLAUDE.md` and `data/persona/update_rules.md`

Do not infer facts not supported by this session.

## Update Policy
- Prefer batch update over per-message writes.
- Skip duplicates already present in target files.
- If confidence is low, write to `data/notes/misc.md` as pending note.

## Target Mapping
- Persona/work style/value/decision pattern -> `data/persona/personality.md`
- Preference/interest/like/dislike -> `data/persona/preferences.md`
- Profile/base info changes -> `data/persona/profile.md`
- Raw ideas from conversation -> `data/ideas/inbox.md`
- Work/project state decisions -> `data/work/overview.md`, `data/work/projects.md`
- Unclear category -> `data/notes/misc.md`

## Execution Steps
1. Extract candidate items from session (3-10 bullets max)
2. Classify each item with target file
3. Remove duplicates by reading target files
4. Apply minimal edits only
5. Report what was updated and what was skipped

## Safety Checks
- Never rewrite entire documents for small updates.
- Keep existing frontmatter format unchanged.
- Respect current file style and language.

## Output Format
When done, respond with:

```markdown
대화 기반 문서 업데이트 완료
- 반영 기준: 현재 세션
- 업데이트 파일:
  - <file>
- 추가된 내용:
  - <item>
- 스킵한 내용:
  - <reason>
```

