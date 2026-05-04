# 공통 산출물 양식

본 문서는 모든 포지션이 공유하는 산출물 메타데이터, 핸드오프 메시지, Decision Log 양식을 정의한다.
포지션별 산출물 양식은 [`agents/<role>.md`](../agents/) 참조.

## 메타데이터 (모든 산출물 필수)

```yaml
---
id: <type>-<slug>
work_unit: WU-...
author: <role>
status: draft | reviewed | approved
generated_by: human | agent | hybrid
trust_level: L1 | L2 | L3   # AI 산출물 신뢰 등급 (아래 §AI 산출물 신뢰 등급)
human_reviewer: <name>      # generated_by != human일 때 필수
prompt_intent: <한 줄>       # generated_by != human일 때 필수, AI에 내린 핵심 지시
source_refs:
  - <path-or-url>
created: YYYY-MM-DD
---
```

## AI 산출물 신뢰 등급

AI/하이브리드로 만들어진 모든 산출물은 다음 3등급 중 하나를 단다. 등급은 메타데이터 `trust_level`에 명시한다.

| 등급 | 의미 | 다음 단계 가능 여부 |
|---|---|---|
| **L1 (Draft)** | AI 초안, 사람 검증 없음. 레이아웃/방향성 참고용. | 동료 리뷰 ❌ / 개발 착수 ❌ |
| **L2 (Verified Logic)** | 작성자가 모든 인터랙션·예외(empty/error/permission)·정책을 직접 검증. 비즈니스 로직 역추적 설명 가능. | 동료 리뷰 ✅ / 개발 착수 ❌ |
| **L3 (Dev-ready)** | 데이터 모델·API 계약·도메인 정합성까지 확인 완료. Dev-ready 체크리스트 통과. | 개발 착수 ✅ |

**핵심 규칙**:
- 개발 착수 조건은 "산출물 완성"이 아니라 **`trust_level: L3`** 이다.
- L1을 다음 포지션으로 핸드오프하지 않는다. 본인이 L2까지 끌어올린 뒤 핸드오프한다.
- "AI가 짜줬어요"는 리뷰 자리에서 금지. 작성자는 산출물의 모든 요소에 대해 **역추적 설명**(Back-tracing) 의무를 진다 — "이 UI 요소는 비즈니스 로직 X를 충족하기 위해 존재한다"는 형식.
- Atomic Planning: AI에 한 번에 큰 화면을 만들게 하지 않고, 작성자가 한눈에 파악 가능한 기능 단위로 나눠 호출한다.

## Dev-ready 체크리스트 (L2 → L3 승격 기준)

기획·디자인·AI 엔지니어 산출물이 개발 착수 가능 상태로 가려면 다음을 모두 충족해야 한다.

- [ ] 문제 정의가 있다
- [ ] 주요 사용자 플로우가 있다
- [ ] 전체 화면 목록이 있다 (특정 화면만 잘려 있지 않다)
- [ ] 각 화면의 액션 결과가 정의되어 있다
- [ ] 주요 엔티티가 정의되어 있다
- [ ] 필수 데이터 필드(이름/타입/필수 여부)가 정의되어 있다
- [ ] 상태값과 상태 전이가 정의되어 있다
- [ ] 권한 기준이 있다
- [ ] API 초안이 있다
- [ ] DB 영향이 검토되었다
- [ ] 예외/오류/빈 상태(empty/error/loading)가 정의되어 있다
- [ ] 미확정 사항(known_missing)이 명시되어 있다
- [ ] PM/디자인/개발 리뷰 상태가 표시되어 있다

승격은 [`workflows/dev-ready-review.md`](../workflows/dev-ready-review.md)의 30분 회의에서 판정한다. 판정값은 `Ready` / `Ready with constraints` / `Not ready` 3가지뿐이다.

## SSOT 섹션 (UI/HTML 산출물 필수)

UI/HTML 시안을 핸드오프할 때는 시안 본문(Visual Prototype)만으로 끝내지 않고, 다음 6섹션을 함께 둔다. AI 산출물의 결과가 아닌 **작성자의 사고**를 분리해서 보존하기 위함이다.

| 섹션 | 내용 | 담당 |
|---|---|---|
| Visual Prototype | AI가 만든 HTML/Figma (UI 흐름 참고용) | AI + 기획/디자인 |
| Data Requirements | 화면에서 필요한 데이터 항목 (필드명, 타입, 필수 여부) | **기획 (필수)** |
| Business Logic | AI 코드에 숨겨진 게 아닌, 작성자가 정의한 정책 (예: 재고 0일 때 버튼 비활성화) | **기획 (필수)** |
| Edge Cases | 네트워크 오류, 권한 없음, 데이터 없음 시 대응 | 기획 + 개발 |
| Prompt Intent | AI에 내린 핵심 지시어와 우선순위 | 기획 |
| Dev Notes | 구현 시 기술 제약, 데이터 모델링 결정 | 개발 |

## 산출물 커버리지/공백 표시

AI 또는 하이브리드 산출물은 메타데이터에 다음 블록을 권장한다. 받는 쪽이 "어디까지 검증되었나"를 즉시 알 수 있어야 한다.

```yaml
coverage:
  screens: full | partial | primary_only
  user_flows: full | primary_only
  error_states: defined | missing
  data_model: defined | partial | not_defined
  api_contract: defined | partial | not_defined
known_missing:
  - <미확정 항목 1>
assumptions:
  - <작성자가 둔 가정 1>
validation:
  planning_review: passed | pending | failed
  design_review: passed | pending | failed
  domain_review: passed | pending | failed
  engineering_review: passed | pending | failed
  qa_review: not_started | passed | failed
```

## 핸드오프 메시지

```yaml
from: <role>
to: <role>
type: request | response | handoff | escalation
ref: <WU-id>
context:
  assumptions: []
  constraints: []
  open_questions: []
```

**송신자 의무**: `trust_level >= L2`가 아닌 산출물은 핸드오프 금지. L2 이하는 본인 단계에서 보완한다.

**수신자 진입 체크리스트** ([`CLAUDE.md` §7.2](../CLAUDE.md))의 항목 외에, AI/하이브리드 산출물에 대해서는 다음을 추가 검증한다:
- [ ] `trust_level`이 명시되어 있다
- [ ] `prompt_intent` 또는 작성자의 의도 설명이 있다
- [ ] `known_missing` / `assumptions` 블록이 있다
- [ ] 작성자가 역추적 설명을 할 수 있다

## Decision Log

```
# DEC-YYYYMM-XXX
1. Context
2. Options (2개 이상, 각각 장단/비용/리스크)
3. Decision
4. Consequences
5. Reversibility
6. References
```

## Work Unit (작업 단위)

```yaml
id: WU-202605-001
goal: <한 줄>
inputs:
  - path: ...
    version: ...
owner: <role>
deliverables:
  - type: <prd | plan | design-spec | api-contract | ...>
    dod:
      - "..."
reviewers: [<role>, ...]
deadline: <ISO8601>
```
