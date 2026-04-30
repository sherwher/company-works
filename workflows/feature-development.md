# Workflow — Feature Development

기능 단위 개발의 표준 흐름.

```
PRD → Design Spec → API Contract → Build (FE/BE) → Test (QA) → Deploy (DevOps) → Monitor
 PM     Designer       Backend         Engineers       QA          DevOps          All
```

## 단계 상세

### 1. 요구사항 확정 (PM)
- 인풋: 사용자 요청, 데이터 분석
- 산출물: PRD (`templates/prd.md`)
- 핸드오프 → Designer / Tech Lead

### 2. 디자인 (Designer)
- 인풋: PRD
- 산출물: Design Spec
- 핸드오프 → Frontend

### 3. API 계약 (Backend + Frontend 합의)
- 인풋: PRD, Design Spec
- 산출물: API Contract (`templates/api-contract.md`)
- 합의 후 양쪽 병렬 개발 가능

### 4. 구현 (FE/BE)
- 작은 PR로 분리, 셀프 리뷰 → 코드 리뷰 → 머지
- DoD: 각 R&R 문서의 DoD

### 5. 검증 (QA)
- 인풋: 빌드, AC, Design Spec, API Contract
- 산출물: Test Plan, 검증 리포트
- Go/No-Go 의견 제출

### 6. 배포 (DevOps)
- 카나리/단계 배포 권장
- 롤백 플랜 사전 합의

### 7. 모니터링·회고 (All)
- 출시 후 지표 모니터링(PM, Data)
- 1주 내 회고(`workflows/retrospective.md`)

## 핸드오프 게이트 요약

| 게이트 | 통과 조건 |
|---|---|
| PRD → Design | AC 명시, 성공 지표 정의, 검토 완료 |
| Design → Build | 모든 시나리오 커버, 에셋 제공 |
| Build → QA | CI 그린, AC 자가 검증 |
| QA → Deploy | Critical/High 결함 0, Go 의견 |
| Deploy → Done | 모니터링 정상, SLO 충족 |
