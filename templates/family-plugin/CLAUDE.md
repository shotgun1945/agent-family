---
updated: {SETUP_DATE}
---

# {plugin-name}

## 프로젝트 목적
{PROJECT_PURPOSE}

## 플러그인 타입
{PLUGIN_TYPE}  <!-- Core | Connector | Distribution -->

## 본체 연동
- 유저 페르소나: `../../{username}/data/persona/`
- 본체 CLAUDE.md: `../../{username}/CLAUDE.md`

## 스킬
- `lets-commit` → `.claude/skills/lets-commit/SKILL.md`

## 백로그
- 활성 백로그: `docs/00_backlog/backlog.md` — **진행** / **대기** 섹션
- 완료 아카이브: `docs/00_backlog/backlog_done.md`
- 백로그 ID 접두어: **`{BACKLOG_PREFIX}`** — 형식 `{BACKLOG_PREFIX}-###`

### 항목 필드 정의
- `ID` — 형식: `{BACKLOG_PREFIX}-###`. 신규 항목은 기존 최대 번호 + 1
- `태그` — `#feature`, `#fix`, `#chore`, `#docs`, `#refactor`
- `제목` — 항목을 한 줄로 요약
- `우선순위` — `critical` / `high` / `medium` / `low`
- `설명` — 작업 내용 및 목적
- `추가 문서` — 관련 문서 경로 (없으면 `-`)
- `등록일` — YYYY-MM-DD

## 커밋 규칙
`.claude/skills/lets-commit/SKILL.md` 준수
