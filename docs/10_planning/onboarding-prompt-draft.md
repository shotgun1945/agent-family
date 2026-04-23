---
updated: 2026-04-23
status: draft
---

# 온보딩 프롬프트 초안

새 유저가 `agent-family` 시스템을 처음 설치할 때 AI 에이전트에게 입력하는 프롬프트.

---

## 설계 원칙

- **복붙 즉시 실행**: 유저는 프롬프트를 수정 없이 그대로 붙여넣는다. 에이전트가 대화로 값을 수집.
- **언어 우선**: 첫 번째 질문은 항상 언어. 이후 모든 출력을 해당 언어로.
- **최소 질문**: 필수 값만 인터렉티브하게 수집. 페르소나 등 부가 설정은 옵션으로.
- **치환 파일 명시**: 에이전트가 빠뜨리지 않도록 치환 대상 파일을 프롬프트에 직접 나열.
- **완료 안내**: 셋업 완료 후 코어 폴더에서 다시 에이전트를 열도록 유도.

---

## 프롬프트 A — 자동 설치 (Option A, 빈 폴더에서 시작)

README.md Option A에 게재되는 프롬프트.

```
Set up my agent-family system.

Step 1: Ask me which language to use (e.g. English / 한국어 / 日本語). Use that language for all output from this point on.

Step 2: Once I answer, show me this form and wait for me to fill it in and send it back:

---
Family name (becomes the folder and repo name):
AI name:
Backlog prefix (2–3 uppercase letters, e.g. MY):
---

Step 3: After I submit the form, do the following:
1. Create the folder structure:
   - {family-name}_family/
   - {family-name}_family/{family-name}/        ← Family Core lives here
   - {family-name}_family/{family-name}_children/
2. Download the Family Core template (no git history attached):
   git clone --depth 1 https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
   rm -rf {family-name}_family/{family-name}/.git
3. Replace all placeholders in the cloned files. Use today's date for {SETUP_DATE} (YYYY-MM-DD).
   Files to update:
   - README.md → {family-name}
   - CLAUDE.md → {SETUP_DATE}, {username}, {LANGUAGE}, {AI_NAME}, {BACKLOG_PREFIX}
   - data/persona/assistant_persona.md → {SETUP_DATE}, {AI_NAME}, {username}
   - data/persona/profile.md → {SETUP_DATE}
   - data/persona/preferences.md → {SETUP_DATE}
   - data/persona/personality.md → {SETUP_DATE}
   - data/persona/update_rules.md → {SETUP_DATE}
   - data/children_manifest.md → {SETUP_DATE}, {username}
   - data/children_registry.md → {SETUP_DATE}, {username}
   - docs/README.md → {SETUP_DATE}
   - docs/00_backlog/backlog.md → {SETUP_DATE}, {BACKLOG_PREFIX}
   - docs/00_backlog/backlog_done.md → {SETUP_DATE}
   Note: {username} and {family-name} = family name from the form.
4. Fetch the plugin registry and ask which plugins I want to install:
   https://raw.githubusercontent.com/shotgun1945/agent-family/main/plugins/registry.md
5. Tell me to open {family-name}_family/{family-name}/ in my AI agent and start fresh from there.
```

---

## 프롬프트 B — 수동 설치 후 코어 셋업 (Option B, 코어 폴더에서 시작)

README.md Option B Step 4에 게재되는 프롬프트.

```
Set up my Family Core.

Step 1: Ask me which language to use (e.g. English / 한국어 / 日本語). Use that language for all output from this point on.

Step 2: Once I answer, show me this form and wait for me to fill it in and send it back:

---
Family name (becomes {username} in all files):
AI name:
Backlog prefix (2–3 uppercase letters, e.g. MY):
---

Step 3: After I submit the form, replace all placeholders. Use today's date for {SETUP_DATE} (YYYY-MM-DD).
Files to update:
- README.md → {family-name}
- CLAUDE.md → {SETUP_DATE}, {username}, {LANGUAGE}, {AI_NAME}, {BACKLOG_PREFIX}
- data/persona/assistant_persona.md → {SETUP_DATE}, {AI_NAME}, {username}
- data/persona/profile.md → {SETUP_DATE}
- data/persona/preferences.md → {SETUP_DATE}
- data/persona/personality.md → {SETUP_DATE}
- data/persona/update_rules.md → {SETUP_DATE}
- data/children_manifest.md → {SETUP_DATE}, {username}
- data/children_registry.md → {SETUP_DATE}, {username}
- docs/README.md → {SETUP_DATE}
- docs/00_backlog/backlog.md → {SETUP_DATE}, {BACKLOG_PREFIX}
- docs/00_backlog/backlog_done.md → {SETUP_DATE}
Note: {username} and {family-name} = family name from the form.

After setup, optionally ask if I want to set up my persona (profile, preferences, personality).
```

---

## 단계별 에이전트 행동 기대값 (Option A 기준)

### 1단계: 언어 + 필수 값 수집 (대화)

에이전트가 순서대로 질문:
1. "What language would you like to use?" → 이후 해당 언어로 전환
2. "What's the family name?" (폴더명이자 프로젝트 이름, `{username}` 자리에 들어감)
3. "What should I call myself? (AI agent name)"
4. "Backlog prefix? (2–3 uppercase letters, e.g. MY)"

### 2단계: 폴더 구조 생성 + 코어 클론

```bash
mkdir -p {family-name}_family/{family-name}
mkdir -p {family-name}_family/{family-name}_children
git clone --depth 1 https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
rm -rf {family-name}_family/{family-name}/.git
```

### 3단계: 플레이스홀더 치환

수집된 값으로 아래 파일 전부 치환 (누락 없이):

| 파일 | 치환 대상 |
|------|-----------|
| `CLAUDE.md` | `{SETUP_DATE}`, `{username}`, `{LANGUAGE}`, `{AI_NAME}`, `{BACKLOG_PREFIX}` |
| `data/persona/assistant_persona.md` | `{SETUP_DATE}`, `{AI_NAME}`, `{username}` |
| `data/persona/profile.md` | `{SETUP_DATE}` |
| `data/persona/preferences.md` | `{SETUP_DATE}` |
| `data/persona/personality.md` | `{SETUP_DATE}` |
| `data/persona/update_rules.md` | `{SETUP_DATE}` |
| `data/children_manifest.md` | `{SETUP_DATE}`, `{username}` |
| `data/children_registry.md` | `{SETUP_DATE}`, `{username}` |
| `docs/README.md` | `{SETUP_DATE}` |
| `docs/00_backlog/backlog.md` | `{SETUP_DATE}`, `{BACKLOG_PREFIX}` |
| `docs/00_backlog/backlog_done.md` | `{SETUP_DATE}` |

### 4단계: 플러그인 선택

레지스트리 raw URL fetch:
```
https://raw.githubusercontent.com/shotgun1945/agent-family/main/plugins/registry.md
```
사용 가능한 플러그인 목록을 보여주고 원하는 것을 선택하게 함.

### 5단계: 완료 안내

에이전트 출력 예시:
```
Setup complete!

Configured:
- CLAUDE.md (language: 한국어, AI name: 루안, prefix: MY)
- data/persona/ (all placeholder dates filled)
- data/children_manifest.md ✓
- data/children_registry.md ✓
- docs/00_backlog/backlog.md (MY-001 ready)

Next step:
→ Open {family-name}_family/{family-name}/ in your AI agent and start fresh from there.

Optional: ask me to set up your persona (profile, preferences, personality).
```

---

## 미결 사항

- [ ] `update_rules.md` 기본값 내용 정의
- [ ] 페르소나 질문 언어를 {LANGUAGE} 설정값에 맞게 자동 전환하는 방법
