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
source_refs:
  - <path-or-url>
created: YYYY-MM-DD
---
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
