---
updated: 2026-04-21
status: draft
---

# Family Core 템플릿 폴더 구조 스펙

## 개요

`{username}_family/`는 두 폴더로 구성된다.

- `{username}/` — **본체(Family Core)**: 에이전트의 신원·기억·행동을 정의하는 마크다운 전용 프로젝트
- `{username}_children/` — **자식 플러그인**: 도메인별 기능 확장 프로젝트 집합

이 문서는 본체(`{username}/`)의 템플릿 구조를 정의한다.

---

## 전체 폴더 트리

```
{username}/
├── CLAUDE.md                          # [필수] 에이전트 거버넌스 헌법
├── .claude/
│   ├── settings.json                  # [필수] Claude Code 설정 (훅, 권한)
│   ├── MEMORY.md                      # [필수] 에이전트 메모리 인덱스
│   └── skills/                        # [필수] 스킬 모듈
│       ├── lets-commit/SKILL.md
│       └── complete-backlog-item/SKILL.md
├── data/
│   └── persona/
│       ├── profile.md                 # [필수] 유저 프로필
│       ├── preferences.md             # [필수] 작업 방식·도구 선호
│       ├── personality.md             # [선택] 성격 특성
│       ├── assistant_persona.md       # [필수] AI 페르소나 정의
│       └── update_rules.md            # [필수] 페르소나 갱신 기준
└── docs/
    ├── README.md                      # [필수] 폴더 구조 설명
    ├── 00_backlog/
    │   ├── backlog.md                 # [필수] 활성 백로그
    │   └── backlog_done.md            # [필수] 완료 아카이브
    ├── 10_planning/                   # 기획서, 요구사항
    ├── 20_spec/                       # 스펙, 설계 문서
    ├── 50_review/                     # 리뷰 기록, 결정 사항
    └── 90_logs/                       # 운영 로그, 릴리즈 노트
```

---

## 파일별 역할

### `CLAUDE.md` [필수]

에이전트의 **헌법**. Claude Code / Cursor가 프로젝트를 열 때 가장 먼저 로드.

포함 섹션:
- 프로젝트 목적 · 에이전트 정체성
- 페르소나 파일 경로 (`data/persona/` 참조)
- 백로그 관리 규칙 (ID 접두어, 필드 정의)
- 스킬 참조 목록
- 배치 문서 업데이트 규칙

---

### `.claude/settings.json` [필수]

Claude Code 실행 환경 설정. 최소 구성:

```json
{
  "permissions": {
    "allow": [],
    "deny": []
  }
}
```

---

### `.claude/MEMORY.md` [필수]

에이전트의 **영구 메모리 인덱스**. 대화 간 맥락을 유지.

- 형식: `- [제목](memory/file.md) — 한 줄 설명`
- 200줄 초과 시 잘림 → 간결하게 유지
- 메모리 파일 실체는 `.claude/memory/` 하위에 저장

---

### `.claude/skills/` [필수]

재사용 가능한 에이전트 행동 단위. 각 스킬은 `{skill-name}/SKILL.md` 형태.

**기본 포함:**

| 스킬 | 역할 |
|------|------|
| `lets-commit` | 커밋 메시지 컨벤션 및 스테이징 |
| `complete-backlog-item` | 백로그 항목 완료 처리 + 아카이브 이동 |

**선택 (자식 프로젝트 구성 후 추가):**

| 스킬 | 역할 |
|------|------|
| `sync-children-distribution` | 스킬/문서를 자식으로 전파 |
| `wiki-ingest` | 외부 소스를 지식 베이스에 수집 |
| `wiki-query` | 지식 베이스 검색 |

---

### `data/persona/` [필수]

에이전트가 유저와 자신을 이해하기 위한 마크다운 메모리 계층.

| 파일 | 내용 | 갱신 주체 |
|------|------|-----------|
| `profile.md` | 직업, 도메인, 현재 목표, 프로젝트 맥락 | 에이전트 (자동) |
| `preferences.md` | 선호 도구, 작업 스타일, 금기 사항 | 에이전트 (자동) |
| `personality.md` | 성격 특성, 커뮤니케이션 패턴 | 에이전트 (자동) |
| `assistant_persona.md` | AI의 이름, 말투, 역할 정의 | 유저 (초기 설정) |
| `update_rules.md` | 위 파일들을 언제/어떻게 갱신할지 기준 | 유저 (초기 설정) |

---

### `docs/` [필수]

번호 접두어로 워크플로우 순서 정렬. 번호 사이 여백은 추후 삽입을 위해 의도적으로 비워둠.

| 폴더 | 용도 |
|------|------|
| `00_backlog/` | 작업 대기·진행·완료 관리 |
| `10_planning/` | 기획서, 요구사항 |
| `20_spec/` | 스펙, 아키텍처, 설계 문서 |
| `50_review/` | 리뷰 기록, 결정 사항 |
| `90_logs/` | 운영 로그, 릴리즈 노트 |

---

## 자식 플러그인 구조

```
{username}_children/{plugin-name}/
├── CLAUDE.md                          # [필수] 플러그인 거버넌스
├── .claude/
│   ├── settings.json                  # [필수] Claude Code 설정
│   └── skills/                        # [선택] 본체에서 로컬 복사된 스킬
│       └── lets-commit/SKILL.md
└── docs/
    ├── README.md
    └── 00_backlog/
        ├── backlog.md
        └── backlog_done.md
```

자식 `CLAUDE.md` 필수 참조:
- 본체 페르소나 경로: `../../{username}/data/persona/`
- 플러그인 타입 명시: Core / Connector / Distribution

---

## 설계 원칙

| 원칙 | 적용 방식 |
|------|-----------|
| 코드 제로 | 본체는 `.md`만. 실행 코드는 자식에만 존재 |
| 파일 = 메모리 | DB 없이 마크다운이 에이전트 상태 저장 |
| 스킬 = 행동 단위 | `SKILL.md` 하나 = 재사용 가능한 워크플로 하나 |
| 플러그인 탈부착 | 자식 추가/제거가 본체에 영향 없음 |
| 단일 진실 원천 | `CLAUDE.md`가 유일한 거버넌스 문서 |
