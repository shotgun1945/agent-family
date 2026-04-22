---
name: sync-super-luan-distribution
description: Syncs distributable rules and skills from super_luan into a child project. Use inside a child project when the user asks to pull, sync, or refresh super_luan rule/skill packages.
---

# Sync Super Luan Distribution

## Purpose
In a child project, pull required rule and skill files from `super_luan` so the child can run with local copies.

## Run Context
This skill is intended to run while the current workspace is a child project under `../super_luan_children/`.

## Source Of Truth
Always read in this order:
1. `../../super_luan/data/children_propagation_manifest.md`
2. `../../super_luan/CLAUDE.md`
3. `자식 로컬 복사 매핑 (sync 기준)` table in the manifest

## Sync Scope
Copy files from `super_luan` to child project local paths using only the manifest table:
- table: `자식 로컬 복사 매핑 (sync 기준)`
- source: `super_luan 소스 경로` column
- destination: `자식 대상 경로` column
- kind: `종류` column (`doc` or `skill`)

Do not copy files not listed in the manifest.  
**`data/children_propagation_manifest.md` is never copied into children** — it is read from `../../super_luan/data/children_propagation_manifest.md` only; it does not appear in `자식 로컬 복사 매핑`.

## Copy Policy

종류(`종류` 컬럼)에 따라 정책이 다르다:

| 종류 | 대상 파일 존재 시 | 대상 파일 없을 시 |
|------|-------------------|-------------------|
| `skill` | 덮어쓰기 (항상 최신 버전 유지) | 복사 |
| `doc` | 유저에게 파일별로 덮어쓸지 확인 후 결정 | 복사 |

공통:
- 대상 폴더가 없으면 생성
- 텍스트는 소스와 동일하게 유지 (내용 재작성 금지)
- 소스 파일이 없으면 에러로 보고하고 나머지 계속 진행

### `doc` 파일 존재 시 확인 방식
각 파일마다 아래 형식으로 유저에게 묻는다:
```
`docs/00_backlog/backlog.md` 이미 존재합니다. 덮어쓰시겠어요? (y/n)
```
- y: 덮어쓰기
- n: skip (기존 내용 보존)

## Optional Metadata Stamp
If child already has a local operation note file, append:
- synced_at date
- source path
- copied file list

If no note file exists, skip this step.

## Execution Checklist
- [ ] Confirm current project is child path
- [ ] Read manifest and parse `자식 로컬 복사 매핑 (sync 기준)` rows
- [ ] Copy all mapped files to child destinations
- [ ] Report copied/skipped/failed files

## Output Format
When finished, respond with:

```markdown
super_luan 배포 동기화 완료
- 대상 프로젝트: <child name>
- copied:
  - <file>
- skipped:
  - <reason>
- failed:
  - <file + reason>
- next:
  - 자식 CLAUDE.md의 `## super_luan 연동` 확인
```

