---
name: wiki-query
description: >-
  Answers investing, stock, and concept questions using the super_luan wiki layer
  (index → concepts/synthesis, raw sources only when needed). Resolves paths for
  super_luan or child-project workspaces. Use when the user asks about investments,
  tickers, or learned concepts, or when navigating data/wiki before raw sources.
---

# Wiki Query

## When to use
- 투자·주식·개념 관련 질문이 들어왔을 때
- 위키를 우선 탐색해야 할 때 (토큰 절약)

## Wiki root (경로 해석)

워크스페이스 루트에 따라 아래처럼 **위키 루트**를 정한다.

| 컨텍스트 | 조건 | 위키 루트 (`index.md` = `{WIKI}/index.md`) |
|----------|------|---------------------------------------------|
| **super_luan** | 현재 루트에 `data/wiki/index.md`가 있음 | `data/wiki` |
| **자식 프로젝트** | `super_luan_children/...` 등, 위가 아님 | `../../super_luan/data/wiki` |

- 이후 절차의 `index.md`, `concepts/`, `synthesis/`, `log.md`는 모두 `{WIKI}/...`로 읽는다.

**raw 소스**는 위키로 부족할 때만 읽는다. 경로도 동일 규칙으로 맞춘다.

| 컨텍스트 | raw 예시 (`data/learning/...`, `data/finance/...`) |
|----------|---------------------------------------------------|
| super_luan | `data/learning/...`, `data/finance/...` |
| 자식 | `../../super_luan/data/learning/...`, `../../super_luan/data/finance/...` |

## Procedure

1. `{WIKI}/index.md`를 읽는다
   - **FAQ 매핑 섹션**을 먼저 확인해 질문 키워드와 매핑되는 페이지를 찾는다
   - 매핑이 없으면 테이블에서 관련 concept 페이지를 찾는다
2. 해당 concept 페이지를 읽는다 (**raw 소스는 읽지 않는다** — 토큰 절약)
3. concept 페이지가 없거나 불충분하면: raw 소스를 읽고 답변 후 **"위키에 없는 내용입니다. ingest할까요?"** 라고 묻는다
4. synthesis 페이지가 있으면 **concept보다 synthesis를 우선** 참조한다

## Filed Back — 대화 분석 결과를 위키에 저장

**대화에서 나온 분석·계산·판단**은 사라지지 않도록 wiki에 filed back한다.

### 저장 대상 (자동 감지)
아래 중 하나에 해당하면 **답변 직후** "이 분석을 위키에 저장할까요?" 라고 제안한다:
- 특정 종목의 손익비·PBR·목표가를 직접 계산한 경우
- 두 개념/종목을 비교·대조한 경우 (예: "싸이클 vs 영구성장 손익비 룰 차이")
- 기존 위키에 없는 새 관점·연결 고리를 발견한 경우
- 유저가 "저장해줘", "기록해줘"라고 요청한 경우 (즉시 저장)

### 저장 위치 결정
| 내용 | 저장 위치 |
|------|---------|
| 특정 종목 분석 (PBR 계산, 손익비) | 해당 entity 페이지 `## 분석 이력` 섹션에 날짜·수치 추가 |
| 개념 간 비교·연결 | 관련 concept 페이지 `## 관련 질문 & 답` 섹션 추가 또는 신규 페이지 |
| 독립적 분석 (여러 개념 통합) | `{WIKI}/analysis/YYYY-MM-DD-[제목].md` 신규 생성 |

### 저장 포맷 (`analysis/` 파일)
```markdown
---
updated: YYYY-MM-DD
type: analysis
query: "유저가 물어본 질문 요약"
concepts:
  - data/wiki/concepts/investing/[관련개념].md
stocks:
  - data/wiki/concepts/stocks/[관련종목].md
---

# [분석 제목]

## 질문
(유저의 원래 질문)

## 분석
(계산·비교·판단 내용)

## 결론
(핵심 takeaway — 1~3줄)
```

### 저장 후 처리
- `{WIKI}/log.md`에 한 줄 추가: `[YYYY-MM-DD] ANALYSIS [파일명] -- query:"질문 요약"`
- `{WIKI}/index.md`의 `## 분석 이력` 섹션에 링크 추가 (없으면 섹션 생성)

## Constraints
- raw는 위키로 답이 안 될 때만 읽는다
- 위키 레이어 역할 분리는 `CLAUDE.md`의 `## Wiki 레이어` 참고
- filed back은 **제안**이 기본 — 유저가 "저장해줘"라고 하면 즉시, 그 외에는 제안 후 확인
