---
updated: 2026-04-22
status: draft
---

# 온보딩 프롬프트 초안

새 유저가 `agent-family-core` 템플릿을 복제한 직후, AI 에이전트에게 처음 입력하는 프롬프트.

---

## 설계 원칙

- **복붙 즉시 실행**: 유저는 프롬프트를 수정 없이 그대로 붙여넣는다. 에이전트가 대화로 값을 수집.
- **한 번에 하나씩**: 에이전트가 값을 하나씩 질문한다. 한꺼번에 요구하지 않는다.
- **즉시 작동**: 온보딩이 끝나면 바로 실제 작업에 쓸 수 있는 상태.
- **되돌릴 수 있음**: 모든 설정은 마크다운 파일이므로 언제든 수정 가능.

---

## 프롬프트 (Family Core 온보딩)

README.md에 게재되는 최종 프롬프트.

```
Set up my agent-family-core.

Ask me each of the following values one by one, then replace all placeholders and fill in the persona files interactively.

Required:
- username
- AI name
- Backlog prefix (2–3 uppercase letters, e.g. MY)
- Project purpose (one-line description)
- Language (default language for all responses and documents, e.g. English / 한국어 / 日本語)
```

---

## 단계별 에이전트 행동 기대값

### 1단계: 값 수집 (대화)

에이전트가 하나씩 질문:
1. "What should I call you? (username for this project)"
2. "What's my name? (AI agent name)"
3. "Backlog prefix? (2–3 uppercase letters, e.g. MY)"
4. "One-line purpose of this project?"
5. "Preferred language for all responses and documents? (e.g. English / 한국어)"

### 2단계: 플레이스홀더 치환

수집된 값으로 아래 파일 일괄 치환:
- `CLAUDE.md` — `{username}`, `{AI_NAME}`, `{BACKLOG_PREFIX}`, `{PROJECT_PURPOSE}`, `{LANGUAGE}`, `{SETUP_DATE}`
- `data/persona/assistant_persona.md` — `{AI_NAME}`, `{username}`
- `docs/00_backlog/backlog.md` — `{BACKLOG_PREFIX}`, `{SETUP_DATE}`
- 전체 파일의 `{SETUP_DATE}`

### 3단계: 페르소나 파일 작성

에이전트가 추가 질문으로 페르소나 파일 채움:
- "What do you do? (job, role)"
- "What will we mainly work on together?"
- "Any preferences for how I respond? (length, style, language mix)"
- "Any rules I should always follow or avoid?"

수집된 답변 → `profile.md`, `preferences.md`, `assistant_persona.md` 작성

### 4단계: 완료 확인

에이전트 출력 예시:
```
Setup complete!

Configured files:
- CLAUDE.md (language: English, AI name: Aria)
- data/persona/profile.md ✓
- data/persona/preferences.md ✓
- data/persona/assistant_persona.md ✓
- docs/00_backlog/backlog.md (MY-001 ready)

What you can do next:
- Create a child project: just ask "Create a new child project"
- Add skills: place SKILL.md files under .claude/skills/
- Build knowledge base: create data/knowledge/ and add documents
```

---

## 프롬프트 (Child 온보딩)

자식 프로젝트 README.md에 게재되는 프롬프트.

```
Set up my agent-family-child.

Ask me each of the following values one by one, then replace all placeholders and initialize the project.

Required:
- Child name
- Backlog prefix (2–3 uppercase letters, e.g. FP)
- Purpose (one-line description)
- Core path (relative path to the Family Core repo, e.g. ../../{username})
```

---

## 미결 사항

- [ ] `update_rules.md` 기본값 내용 정의
- [ ] 페르소나 질문 언어를 {LANGUAGE} 설정값에 맞게 자동 전환하는 방법
