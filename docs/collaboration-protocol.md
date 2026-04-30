# Collaboration Protocol

에이전트 간, 그리고 에이전트-사람 간 협업의 규약입니다. 모든 에이전트는 본 규약을 컨텍스트로 주입받아야 합니다.

## 1. 메시지 포맷

에이전트가 다른 에이전트/사람에게 전달하는 모든 요청·응답은 다음 구조를 갖습니다.

```yaml
from: <role>           # 예: product-manager
to: <role>             # 예: backend-engineer
type: request | response | handoff | escalation | decision
ref: <work-unit-id>    # 작업 단위 식별자
goal: <한 줄 목표>
context:
  inputs:              # 참조 산출물 경로/링크
    - path: ...
  constraints: []
  assumptions: []
deliverables:          # 요청 시: 기대 산출물
  - type: tech-spec
    dod:
      - "..."
deadline: <ISO8601>
```

## 2. 핸드오프 규약

핸드오프는 작업 단위의 Owner가 변경되는 시점에 발생합니다.

### 2.1 진입 체크리스트(Receiver 입장)

- [ ] Inputs가 모두 존재하며 최신 버전인가
- [ ] DoD가 측정 가능한가
- [ ] 결정 권한이 명확한가(누가 승인하는가)
- [ ] 의존성·블로커가 명시되어 있는가

체크리스트 중 하나라도 실패 시 **수신 거부(reject)** 하고 송신자에게 사유를 명기해 반려.

### 2.2 산출물 위치

- 모든 산출물은 `templates/` 양식을 따라 프로젝트 저장소의 `docs/<domain>/` 하위에 저장
- 파일명: `YYYYMMDD-<slug>.md`
- 변경 이력은 git commit으로만 관리(별도 changelog 금지)

## 3. 의사결정 규약

| 결정 유형 | 결정자 | 기록 위치 |
|---|---|---|
| 가역적·범위 내 | Owner 에이전트 단독 | Decision Log |
| 비가역적 또는 범위 외 | Owner + Reviewer 합의 | Decision Log + Charter 갱신 |
| 외부 영향(고객·법적·보안) | 사람 승인 필수 | Decision Log + 승인자 명시 |

모든 결정은 `templates/decision-log.md` 양식을 따른다.

## 4. 에스컬레이션

다음 상황에서 에이전트는 즉시 작업을 멈추고 사람에게 에스컬레이션한다.

1. DoD를 만족시키기 위해 비가역적 작업이 필요할 때
2. 입력 산출물 간 모순을 발견했을 때
3. R&R 경계를 넘어서는 작업이 요구될 때
4. 보안·개인정보·법적 리스크가 의심될 때
5. 동일 문제로 2회 이상 재시도가 실패할 때

## 5. 산출물 품질 게이트

모든 산출물은 다음 게이트를 통과해야 한다.

1. **Schema gate**: 해당 템플릿의 필수 섹션이 모두 채워졌는가
2. **DoD gate**: 작업 단위에 명시된 DoD를 만족하는가
3. **Review gate**: 정해진 Reviewer가 승인했는가
4. **Trace gate**: 출처(입력·prompt·결정자)가 기록되어 있는가

## 6. 명명·식별자 규약

- Work Unit ID: `WU-YYYYMM-<seq>` (예: `WU-202604-014`)
- Decision ID: `DEC-YYYYMM-<seq>`
- Incident ID: `INC-YYYYMMDD-<seq>`

## 7. 금지 사항

- 다른 포지션의 R&R 내 작업을 임의로 수행하지 않는다(요청 또는 핸드오프 필요)
- 입력 산출물 없이 산출물을 만들지 않는다(가정은 명시)
- 사람 승인 없이 외부 시스템에 비가역 작업을 수행하지 않는다
- 기존 의사결정을 단독으로 뒤집지 않는다
