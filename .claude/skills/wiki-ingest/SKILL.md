---
name: wiki-ingest
description: >-
  Ingests learning/finance sources into super_luan data/wiki (concepts, stocks,
  log, index, synthesis). In super_luan, default scan of data/learning and
  data/finance when no source given; user may specify paths or paste content.
  In child projects, explicit source paths or inline content is required. Use when
  the user says ingest, 위키 업데이트, or similar.
---

# Wiki Ingest

## Wiki write root (경로 해석)

위키 파일은 **항상 super_luan 저장소** 아래에 둔다.

| 컨텍스트 | 위키 루트 (`index.md` = `{WIKI}/index.md`) |
|----------|---------------------------------------------|
| **super_luan** | `data/wiki` |
| **자식 프로젝트** | `../../super_luan/data/wiki` |

`log.md`·`index.md`·`concepts/`·`synthesis/` 갱신은 모두 `{WIKI}/...` 기준.

## 입력 — 무엇을 소스로 삼을지

### super_luan에서

1. **유저가 폴더·파일·붙여넣은 본문을 지정한 경우** → 그것만 소스로 삼는다 (여러 경로 가능).
2. **아무 것도 지정하지 않은 경우 (디폴트)** → 아래 **기본 스캔 루트**만 조사하여 ingest 후보를 찾는다.  
   - `data/learning/`  
   - `data/finance/`  
   - `data/wiki/`는 소스가 아니다 (산출물). `data/persona/`, `data/journal/` 등 위키 ingest 대상이 아닌 트리는 디폴트 스캔에 넣지 않는다.

### 자식 프로젝트에서

- **디폴트 스캔 없음.** 아래 **하나 이상**이 있어야 절차를 진행한다.
  - **명시 경로**: 파일 또는 디렉터리 (가능하면 `../../super_luan/data/learning/...`, `../../super_luan/data/finance/...` 형태로 지정)
  - **인라인 본문**: 유저가 채팅에 붙여 넣은 텍스트 + 위키에 반영 요청
- 둘 다 없으면 **중단**하고, 필요한 경로 또는 본문을 요청한다.

### 로그의 소스 표기

- 파일 경로면 그대로 `log.md`에 쓴다.
- 인라인이면 `-- inline:user` 또는 `-- inline:paste` 등 짧게 구분해 남긴다.

## Agent procedure

1. `{WIKI}/index.md`를 읽고 기존 concept 페이지 목록을 파악한다
2. 위 **입력** 규칙에 따라 소스를 확정하고 해당 파일(또는 붙여넣기)을 읽는다
3. **신규 개념** → `{WIKI}/concepts/[domain]/[name].md` 생성, frontmatter에 `sources:` 필드 포함
4. **기존 개념 업데이트** → 해당 concept 페이지를 읽고 병합 (중복 제거)
5. **종목 언급** → `{WIKI}/concepts/stocks/[ticker].md` 생성 또는 업데이트
6. `{WIKI}/log.md`에 한 줄 추가: `[YYYY-MM-DD] CREATED|UPDATED [target] -- [source]`
7. `{WIKI}/index.md` 테이블 업데이트 (행 추가 또는 날짜 갱신)
8. 영향받은 synthesis 페이지가 있으면 함께 업데이트

## Concept page format

신규/갱신 시 아래 포맷을 따른다.

```markdown
---
updated: YYYY-MM-DD
type: concept          # concept | entity | synthesis
domain: investing      # investing | stocks | general
sources:
  - data/learning/fanding/lectures/파일명.md
tags: []
---

# [개념명]

## 한 줄 정의

## 핵심 내용

## 연관 개념
- [개념명](../[파일]) — 설명
```

## Constraints
- **raw 소스 원본 파일은 수정하지 않는다** (위키만 갱신)
- 역할 분리 표는 `CLAUDE.md` `## Wiki 레이어` 참고
