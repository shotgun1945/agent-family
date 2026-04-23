---
updated: 2026-04-16
---

# agent-family

## 프로젝트 목적
**agent-family**는 `super_luan`의 패밀리 구조(본체-자식 계층)를 일반 사용자에게 배포하기 위한 **공개 템플릿 프레임워크** 프로젝트다.

super_luan에서 개인화된 내용을 제거하고, 누구나 자신만의 AI 패밀리 시스템을 구축할 수 있도록 범용 템플릿과 온보딩 가이드를 제공한다.

## 범위
- 포함:
  - 배포용 본체(family) 템플릿 설계 및 문서화
  - 플러그인(children) 구조 정의 및 가이드
  - GitHub Template Repository 설정
  - 온보딩 프롬프트 설계
  - agent-family 프레임워크 개념 문서
- 제외:
  - 개인 데이터 (페르소나, 재정, 일기 등)
  - super_luan 전용 스킬 (turtle-trading 등)
  - 실제 자식 프로젝트 코드 (teleluan, financial-planner2 등)

## 아키텍처 개념

### agent-family 패턴
```
{username}_family/
├── {username}/              # 본체 (Family Core)
│   ├── CLAUDE.md            # 에이전트 거버넌스 헌법
│   ├── data/                # 페르소나·기억·지식 (마크다운)
│   └── .claude/skills/      # 재사용 가능한 에이전트 행동 단위
└── {username}_children/     # 자식 플러그인들
    ├── {plugin-name}/       # Core Plugin / Connector Plugin 등
    └── ...
```

### 플러그인 타입
| 타입 | 역할 | 예시 |
|------|------|------|
| Core Plugin | 본체 기능 확장 | financial-planner |
| Connector Plugin | 외부 서비스 연동 | telegram-bot |
| Distribution Plugin | 배포·공개 관리 | agent-family (이 프로젝트) |

### 설계 원칙
- **본체 코드 제로**: 본체는 마크다운(CLAUDE.md + data/)만으로 구성
- **플러그인 탈부착**: 자식 프로젝트는 독립적으로 추가/제거 가능
- **파일 = 메모리**: DB 없이 마크다운 파일이 에이전트의 상태/기억
- **스킬 = 행동 단위**: .claude/skills/ 안의 SKILL.md가 재사용 가능한 워크플로

## 스킬·룰 추가 규칙 (구조 준수)

### 스킬 추가
- **원본(단일 진실 원천)**: `.claude/skills/<skill-name>/SKILL.md`
- **에이전트 호환 wrapper**: `.agents/skills/<skill-name>/SKILL.md`
- wrapper의 `name`/`description`은 원본과 **동일하게 유지**한다
- wrapper 본문은 **최소 내용**만 두고, 항상 원본 `.claude/skills/.../SKILL.md`를 읽고 따르도록 연결한다 (중복 작성 금지)

### 룰 추가
- 프로젝트의 거버넌스/작업 규칙은 항상 `CLAUDE.md`를 **단일 진실 원천**으로 유지한다
- Cursor 룰을 추가/변경하는 경우:
  - `.cursor/rules/project-context.mdc`만 `alwaysApply: true`로 유지한다
  - 나머지는 `alwaysApply: false`로 별도 파일에 작성하고, 이 문서의 `## Cursor Rules 참조` 테이블에 등록한다

## super_luan 연동
- **전파 목록(매니페스트)** — 자식으로 복사하지 않음. 항상 super_luan만 참조: `../../super_luan/data/children_propagation_manifest.md`
- 로컬 복사된 스킬(매니페스트 `자식 로컬 복사 매핑`):
  - `lets-commit` → `.claude/skills/lets-commit/SKILL.md`
  - `update-docs-from-conversation` → `.claude/skills/update-docs-from-conversation/SKILL.md`
  - `sync-super-luan-distribution` → `.claude/skills/sync-super-luan-distribution/SKILL.md`
  - `complete-backlog-item` → `.claude/skills/complete-backlog-item/SKILL.md`
  - `report-super-luan-distribution-change` → `.claude/skills/report-super-luan-distribution-change/SKILL.md`
  - `wiki-query` → `.claude/skills/wiki-query/SKILL.md`
  - `wiki-ingest` → `.claude/skills/wiki-ingest/SKILL.md`
- super_luan 참조 문서(상대 경로):
  - `../../super_luan/data/persona/`
  - `../../super_luan/data/persona/update_rules.md`

## 유저 페르소나
- `../../super_luan/data/persona/profile.md`
- `../../super_luan/data/persona/preferences.md`
- `../../super_luan/data/persona/personality.md`

## AI 페르소나 (루안)
- `../../super_luan/data/persona/assistant_persona.md`

## 페르소나 업데이트
- 저장 기준: `../../super_luan/data/persona/update_rules.md`
- 조건에 맞는 정보는 `../../super_luan/data/persona/`에 직접 반영

## 백로그
- 활성 백로그: `docs/00_backlog/backlog.md` — **진행** / **대기** 섹션
- 완료 아카이브: `docs/00_backlog/backlog_done.md`
- 백로그 기획문서: `docs/10_planning/backlog/` 폴더에 생성
- 완료 처리: `/complete-backlog-item` 스킬 사용
- 백로그 ID 접두어: **`AF`** — 형식 `AF-###`

### 항목 필드 정의
- `ID` — 형식: `AF-###` (세 자리 숫자). 신규 항목은 기존 최대 번호 + 1
- `태그` — 작업 유형 (`#feature`, `#fix`, `#chore`, `#docs`, `#refactor`)
- `제목` — 항목을 한 줄로 요약
- `우선순위` — `critical` / `high` / `medium` / `low`
- `설명` — 작업 내용 및 목적
- `추가 문서` — 관련 문서 경로 (없으면 `-`)
- `등록일` — YYYY-MM-DD

## Cursor Rules 참조

| 파일 | 적용 상황 |
|------|-----------|
| `.cursor/rules/project-context.mdc` | 항상 적용 — CLAUDE.md를 단일 진실 원천으로 참조 |

- `CLAUDE.md`가 항상 적용되는 단일 진실 원천이다
- `.cursor/rules/project-context.mdc`만 `alwaysApply: true`로 설정하고 CLAUDE.md를 가리킨다
- 상황별 룰은 `alwaysApply: false`로 별도 파일에 작성하고 위 테이블에 등록한다

## 개발 완료 후·커밋 전 문서화
- 기능·수정 작업이 끝나면 커밋 전에 관련 문서를 갱신한다
- 커밋 규칙: `.claude/skills/lets-commit/SKILL.md` 준수

