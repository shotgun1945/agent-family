# agent-family-plugin

> AI 패밀리 시스템의 **자식 플러그인** 템플릿입니다.

[![Use this template](https://img.shields.io/badge/Use%20this%20template-2ea44f?style=for-the-badge)](../../generate)

---

## 이게 뭔가요?

본체([agent-family-core](../agent-family-core))에 기능을 추가하는 플러그인 프로젝트입니다.

| 플러그인 타입 | 역할 | 예시 |
|--------------|------|------|
| Core Plugin | 본체 기능 확장 | financial-planner, journal |
| Connector Plugin | 외부 서비스 연동 | telegram-bot, notion-sync |
| Distribution Plugin | 배포·공개 관리 | agent-family (이 템플릿의 출처) |

---

## 빠른 시작

### 1. 템플릿 복제

**"Use this template"** 클릭 → 레포 이름은 플러그인 역할로 (예: `financial-planner`, `telegram-bot`).

### 2. 로컬 배치

본체와 형제 관계로 배치하는 것을 권장:

```bash
{username}_family/
├── {username}/                # 본체 (agent-family-core)
└── {username}_children/
    └── {plugin-name}/         # 이 레포
```

### 3. Claude Code 열기

```bash
cd {username}_children/{plugin-name}
claude
```

### 4. 온보딩 프롬프트 실행

```
agent-family-plugin 초기 설정 진행해줘.

- 플러그인 이름: [plugin-name]
- 플러그인 타입: Core | Connector | Distribution
- 백로그 접두어: [2~3자리 영문 대문자, 예: FP]
- 목적: [한 줄 설명]
- 본체 경로: ../../{username}
```

---

## 폴더 구조

```
{plugin-name}/
├── CLAUDE.md                  # 플러그인 거버넌스
├── .claude/
│   ├── settings.json
│   └── skills/
│       └── lets-commit/SKILL.md
└── docs/
    ├── README.md
    └── 00_backlog/
        ├── backlog.md
        └── backlog_done.md
```

---

## 라이선스

MIT
