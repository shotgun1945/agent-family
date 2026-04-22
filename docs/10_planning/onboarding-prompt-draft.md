---
updated: 2026-04-21
status: draft
---

# 온보딩 프롬프트 초안

새 유저가 `agent-family-core` 템플릿을 복제한 직후, AI 에이전트에게 처음 입력하는 프롬프트.

---

## 설계 원칙

- **한 번에 하나씩**: 모든 설정을 한꺼번에 요구하지 않는다. 단계별로 진행.
- **질문으로 이끌기**: 에이전트가 먼저 물어보고, 유저가 답하면 파일에 기록.
- **즉시 작동**: 온보딩이 끝나면 바로 실제 작업에 쓸 수 있는 상태여야 함.
- **되돌릴 수 있음**: 모든 설정은 마크다운 파일이므로 언제든 수정 가능.

---

## 프롬프트 초안

```
안녕! 나는 agent-family-core 템플릿을 방금 복제했어.
이 에이전트 시스템을 내 것으로 만들기 위한 초기 설정을 같이 진행해줘.

아래 순서로 진행해줘:

1. **플레이스홀더 치환**
   CLAUDE.md와 관련 파일에서 아래 값들을 내가 알려주는 값으로 바꿔줘:
   - {username} → [내 이름 또는 프로젝트 식별자]
   - {AI_NAME} → [에이전트 이름]
   - {BACKLOG_PREFIX} → [백로그 ID 접두어, 예: AF]
   - {PROJECT_PURPOSE} → [이 에이전트의 한 줄 목적]

2. **페르소나 파일 작성**
   나에 대해 몇 가지 물어봐줘. 내 답변을 바탕으로 아래 파일을 채워줘:
   - data/persona/profile.md
   - data/persona/preferences.md
   - data/persona/assistant_persona.md

3. **백로그 초기화**
   docs/00_backlog/backlog.md의 예시 항목을 지우고,
   첫 번째 실제 작업이 있으면 항목으로 등록해줘.

4. **완료 확인**
   설정이 끝나면 현재 파일 구조를 요약해서 보여줘.
   뭔가 빠졌거나 더 설정할 게 있으면 알려줘.

시작해줘.
```

---

## 단계별 에이전트 행동 기대값

### 1단계: 플레이스홀더 치환

에이전트가 수행할 것:
- 유저에게 4개 값 요청
- `CLAUDE.md`, `data/persona/assistant_persona.md`, `docs/00_backlog/backlog.md` 일괄 치환
- 변경된 파일 목록 보고

### 2단계: 페르소나 파일 작성

에이전트가 물어볼 질문 (예시):
- "어떤 일을 하시나요? (직업, 역할)"
- "이 에이전트와 주로 어떤 작업을 함께 하고 싶으세요?"
- "선호하는 작업 스타일이 있나요? (짧은 답변 vs 상세한 설명, 한국어 vs 영어 등)"
- "에이전트 이름 외에 성격이나 말투에 대한 선호가 있나요?"

수집된 답변 → `profile.md`, `preferences.md`, `assistant_persona.md` 작성

### 3단계: 백로그 초기화

- placeholder 항목 제거
- 유저가 첫 작업을 말하면 `{BACKLOG_PREFIX}-001` 항목으로 등록

### 4단계: 완료 확인

에이전트 출력 예시:
```
초기 설정 완료!

설정된 파일:
- CLAUDE.md ({username} → darlin, {AI_NAME} → 루안)
- data/persona/profile.md ✓
- data/persona/preferences.md ✓
- data/persona/assistant_persona.md ✓
- docs/00_backlog/backlog.md (AF-001 등록)

다음에 할 수 있는 것:
- 자식 플러그인 추가: agent-family-plugin 템플릿 복제
- 스킬 추가: .claude/skills/ 에 SKILL.md 추가
- 지식 베이스 구성: data/knowledge/ 폴더 생성 후 문서 추가
```

---

## 변형 프롬프트 — 빠른 시작

플레이스홀더를 미리 다 알고 있는 경우 한 번에 입력:

```
agent-family-core 초기 설정 진행해줘.

- username: darlin
- AI 이름: 루안
- 백로그 접두어: AF
- 프로젝트 목적: 개인 AI 패밀리 시스템 본체

페르소나는 나중에 채울게. 지금은 플레이스홀더 치환과 백로그 초기화만 해줘.
```

---

## 미결 사항

- [ ] 페르소나 질문 목록 확정 (몇 개가 적당한지)
- [ ] `update_rules.md` 기본값 내용 정의
- [ ] 온보딩 완료 후 자식 플러그인 추가 흐름 연결 방법
