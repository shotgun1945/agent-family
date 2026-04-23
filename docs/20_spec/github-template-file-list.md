---
updated: 2026-04-22
status: draft
---

# GitHub Template Repository 파일 목록

## 배포 구조

GitHub Template Repository는 **두 개의 레포**로 분리 배포한다.

| 레포 | 용도 | 복제 대상 |
|------|------|-----------|
| `agent-family-core` | 부모(Family Core) 템플릿 | `{username}/` |
| `agent-family-child` | 자식 템플릿 | `{username}_children/{child-name}/` |

**이유:** 부모는 한 번만 만들지만 자식은 여러 개를 만든다. 분리하면 각각 "Use this template"으로 독립 생성 가능.

---

## `agent-family-core` 파일 목록

### 포함 — 필수

```
CLAUDE.md                              # 플레이스홀더 포함 템플릿
.gitignore
.claude/
├── settings.json
├── MEMORY.md                          # 빈 인덱스
├── skills/
│   ├── lets-commit/SKILL.md
│   ├── complete-backlog-item/SKILL.md
│   ├── create-child/SKILL.md          # 자식 프로젝트 생성
│   ├── sync-to-children/SKILL.md      # 부모→자식 전파
│   └── sync-to-core/SKILL.md         # 자식→부모 역전파
└── templates/
    └── family-child/                  # create-child 스킬이 사용하는 자식 템플릿
data/
└── persona/
    ├── profile.md                     # 빈 템플릿 (작성 가이드 포함)
    ├── preferences.md                 # 빈 템플릿
    ├── assistant_persona.md           # 빈 템플릿 (예시 포함)
    └── update_rules.md                # 기본값 제공 (수정 가능)
docs/
├── README.md
└── 00_backlog/
    ├── backlog.md                     # placeholder 예시 포함
    └── backlog_done.md                # 빈 파일
```

### 포함 — 선택

```
.github/
└── ISSUE_TEMPLATE/
    └── backlog-item.md                # 백로그 항목 이슈 템플릿
data/
└── persona/
    └── personality.md                 # 빈 템플릿 (작성 안 해도 무방)
```

### 제외

| 파일/폴더 | 제외 이유 |
|-----------|-----------|
| `.claude/settings.local.json` | 로컬 환경 전용 (MCP 서버 설정 등) |
| `.claude/memory/` | 개인 메모리 데이터 |
| `.cursor/` | 에디터 로컬 설정 |
| `.vscode/` | 에디터 로컬 설정 |
| `docs/10_planning/` ~ `docs/90_logs/` | 빈 폴더 (Git은 빈 폴더 추적 불가) |

> 빈 폴더가 필요한 경우 `.gitkeep` 파일을 넣어 추적.

---

## `agent-family-child` 파일 목록

### 포함 — 필수

```
CLAUDE.md                              # 자식 거버넌스 템플릿
.gitignore
.claude/
├── settings.json
└── skills/
    └── lets-commit/SKILL.md           # 부모에서 전파받는 스킬
docs/
├── README.md
└── 00_backlog/
    ├── backlog.md
    └── backlog_done.md
```

### 제외

| 파일/폴더 | 제외 이유 |
|-----------|-----------|
| `.claude/settings.local.json` | 로컬 환경 전용 |
| `.cursor/`, `.vscode/` | 에디터 로컬 설정 |

---

## 플레이스홀더 치환 목록

온보딩 시 에이전트가 수집하여 치환하는 값들.

### `agent-family-core`

| 플레이스홀더 | 설명 | 예시 |
|--------------|------|------|
| `{username}` | 유저/프로젝트 식별자 | `darlin`, `alice` |
| `{AI_NAME}` | AI 에이전트 이름 | `루안`, `Aria` |
| `{BACKLOG_PREFIX}` | 백로그 ID 접두어 | `AF`, `MY` |
| `{PROJECT_PURPOSE}` | 프로젝트 한 줄 설명 | `개인 AI 패밀리 시스템 본체` |
| `{LANGUAGE}` | 기본 응답 언어 | `한국어`, `English` |
| `{SETUP_DATE}` | 초기 설정 날짜 | `2026-04-22` |

치환 대상 파일:
- `CLAUDE.md` — 모든 플레이스홀더
- `data/persona/assistant_persona.md` — `{AI_NAME}`
- `docs/00_backlog/backlog.md` — `{BACKLOG_PREFIX}`
- 전체 파일의 `{SETUP_DATE}`

### `agent-family-child`

| 플레이스홀더 | 설명 | 예시 |
|--------------|------|------|
| `{child-name}` | 자식 프로젝트 이름 | `financial-planner` |
| `{username}` | 부모 레포 식별자 | `darlin` |
| `{BACKLOG_PREFIX}` | 백로그 ID 접두어 | `FP`, `TB` |
| `{PROJECT_PURPOSE}` | 자식 프로젝트 한 줄 목적 | `개인 재정 관리` |
| `{SETUP_DATE}` | 초기 설정 날짜 | `2026-04-22` |

---

## .gitignore 항목

```gitignore
# Claude Code 로컬 설정
.claude/settings.local.json

# 에이전트 메모리 (개인 데이터)
.claude/memory/

# 에디터
.cursor/
.vscode/
.DS_Store
```

---

## GitHub Template 설정 방법

1. 레포 생성 후 **Settings → General → Template repository** 체크
2. `README.md`(루트)에 "Use this template" 뱃지 추가
3. `main` 브랜치 기준으로 템플릿 제공

```markdown
[![Use this template](https://img.shields.io/badge/Use%20this%20template-2ea44f?style=for-the-badge)](https://github.com/{owner}/agent-family-core/generate)
```
