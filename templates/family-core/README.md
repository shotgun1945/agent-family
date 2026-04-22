# agent-family-core

> 나만의 AI 패밀리 시스템의 **본체(Family Core)** 템플릿입니다.

[![Use this template](https://img.shields.io/badge/Use%20this%20template-2ea44f?style=for-the-badge)](../../generate)

---

## 이게 뭔가요?

AI 에이전트에게 **정체성·기억·행동 방식**을 부여하는 마크다운 전용 프로젝트입니다.

- 코드 없음 — 모든 설정은 `.md` 파일
- DB 없음 — 파일이 곧 에이전트의 메모리
- 플러그인 구조 — 자식 프로젝트를 붙였다 뗐다 가능

---

## 빠른 시작

### 1. 템플릿 복제

이 레포 우상단 **"Use this template"** → **"Create a new repository"** 클릭.

레포 이름은 `{username}` 형식 권장 (예: `darlin`, `alice`).

### 2. 로컬 클론

```bash
git clone https://github.com/{your-github-id}/{username}.git
cd {username}
```

### 3. Claude Code 열기

```bash
claude
```

### 4. 온보딩 프롬프트 실행

Claude Code에 아래 프롬프트를 입력하세요:

```
agent-family-core 초기 설정 진행해줘.

아래 값들로 플레이스홀더를 치환하고, 
페르소나 파일 작성을 위해 나에게 질문해줘.

- username: [내 이름]
- AI 이름: [에이전트 이름]
- 백로그 접두어: [2~3자리 영문 대문자, 예: MY]
- 프로젝트 목적: [한 줄 설명]
```

---

## 폴더 구조

```
{username}/
├── CLAUDE.md                  # 에이전트 거버넌스 헌법
├── .claude/
│   ├── settings.json          # Claude Code 설정
│   ├── MEMORY.md              # 에이전트 메모리 인덱스
│   └── skills/                # 재사용 가능한 스킬
├── data/persona/              # 유저·AI 페르소나
└── docs/                      # 백로그 및 프로젝트 문서
```

---

## 자식 플러그인 추가

도메인별 기능이 필요할 때 [agent-family-plugin](../agent-family-plugin) 템플릿으로 자식 프로젝트를 추가하세요.

```
{username}_family/
├── {username}/                # 이 레포 (본체)
└── {username}_children/
    ├── financial-planner/     # 재정 관리 플러그인
    └── telegram-bot/          # 텔레그램 연동 플러그인
```

---

## 라이선스

MIT
