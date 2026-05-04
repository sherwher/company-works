# CLAUDE.md - 사내 AI 에이전트 협업 규약

본 문서는 사내 AI 에이전트 협업 시스템의 **단일 규약 파일**입니다.
신규 프로젝트는 이 파일을 루트에 복사하고 S1의 빈칸을 채우면 즉시 운영 가능합니다.

각 원칙의 근거는 [`research/00-synthesis.md`](./research/00-synthesis.md)를 참조하세요.

> **핵심 원칙**: 각 포지션이 자기 에이전트를 갖되, 산출물 형식은 표준화하고, 핸드오프는 구조화하고, 위험 등급에 따라 사람 게이트를 건다.

---

## 1. 프로젝트 컨텍스트

각 프로젝트는 본 절을 채워서 시작한다.

- **Project**: `<프로젝트명>`
- **Owner (Human)**: `<이름/팀>`
- **목표**: `<한 문단>`
- **범위 / Non-Goals**: `<한 문단>`
- **활성 포지션**: 아래 S3에서 체크 표시
- **활성 워크플로**: S6에서 선택
- **권한 등급 상한**: S4의 L0~L4 중 어디까지 허용하는가

---

## 2. 운영 원칙 (12가지)

### A. 아키텍처
1. **워크플로 우선**. 순차/분기/병렬로 풀 수 있는 일에 자율 에이전트를 쓰지 않는다.
2. **한 포지션 = 한 에이전트 = 한 책임**. 한 에이전트가 두 역할 수행 금지.
3. **명시적 작업 명세로 핸드오프**. 모든 핸드오프는 6슬롯(Goal/Inputs/Owner/Deliverables/DoD/Deadline) 구조화.

### B. 산출물
4. **Draft-by-default + Citation**. 모든 AI 산출물은 초안 상태로 시작. 메타데이터 필수.
5. **3단계 승격 게이트**. AI 초안 -> 동료 리뷰 -> 오너 승격.
6. **산출물 형식 표준화**. 포지션별 산출물은 S5의 양식을 따른다.

### C. 검증
7. **결정적 검사 -> AI 리뷰 -> 사람 리뷰**. 컴파일러/테스트/린터 -> LLM judge -> 사람.
8. **Suggest, don't merge**. 자동 머지/발행은 옵트인. 기본은 PR/Draft + 사람 승인.

### D. 사람 게이트
9. **위험 등급별 자율성 차등**. L0~L4 (S4).
10. **사람 호출은 항상 가능**. 어떤 단계에서든 에스컬레이션 가능.

### E. 운영
11. **관측성 Day-1**. 모든 에이전트 호출 trace 저장. 비용/지연/품질 모니터링.
12. **Quality-adjusted 성과 측정**. 효율 + 품질 + 만족도를 함께 측정.

---

## 3. 포지션별 R&R

각 포지션은 **Mission / In-Scope / Out-of-Scope / 에이전트 활용 / Deliverables / DoD / 권한 등급**으로 정의.

### 3.1 PM (Product Manager)
- **Mission**: 문제 정의, 우선순위, 성공 지표 수립
- **In-Scope**: 시장/사용자 리서치 -> PRD, AC(Acceptance Criteria), 로드맵
- **Out-of-Scope**: UI 디자인, 구현 방식, 테스트 케이스
- **에이전트 활용**: 시장 리서치 요약, PRD 초안 생성, 경쟁사 분석, AC 정리
- **Deliverables**: PRD (S5.1)
- **DoD**: 문제/가치 가설 + 측정 가능 지표 + AC + Non-Goals 명시 + 기획/TL 검토 완료
- **권한**: L2

### 3.2 기획 (Planner)
- **Mission**: PRD를 구체적 화면 플로우, 정책, 비즈니스 로직으로 변환
- **In-Scope**: PRD -> 기획서(화면 정의, 정책, 플로우), AC 상세화
- **Out-of-Scope**: 비주얼 디자인, 코드 구현
- **에이전트 활용**: PRD에서 AC 자동 추출, 유사 기획서 참조, 정책 문서 초안
- **Deliverables**: 기획서 (S5.2)
- **DoD**: 모든 시나리오(정상/예외/에러) 커버 + AC 상세화 + 디자이너/BE 구현 가능성 확인
- **권한**: L2

### 3.3 디자이너 (Product Designer)
- **Mission**: 기획서를 사용 가능한 UI/플로우로 변환. 사용성/접근성 책임
- **In-Scope**: 기획서, 디자인 시스템 -> Design Spec, 시안, 인터랙션 명세
- **Out-of-Scope**: 비즈니스 로직 결정, 코드 구현
- **에이전트 활용**: 시안 변형 생성, 디자인 시스템 검사, 카피라이팅 보조
- **Deliverables**: Design Spec (S5.3)
- **DoD**: 모든 화면 상태(Default/Empty/Loading/Error/Success) + DS 토큰 사용 + FE 구현 가능성
- **권한**: L2

### 3.4 프론트엔드 엔지니어 (Frontend Engineer)
- **Mission**: Design Spec을 성능/접근성 갖춘 UI로 구현
- **In-Scope**: Design Spec, API Contract -> 구현 코드(PR), 테스트
- **Out-of-Scope**: API 설계, 데이터 모델링, 인프라
- **에이전트 활용**: 컴포넌트 구현, 리팩토링, 테스트 코드 생성, 코드 리뷰 보조
- **Deliverables**: 구현 코드 + PR
- **DoD**: AC 통과 + 시안 일치 + CI 그린 + a11y/반응형 + PR 리뷰 통과
- **권한**: L2

### 3.5 백엔드 엔지니어 (Backend Engineer)
- **Mission**: 도메인 모델/로직/API의 안정적 구현
- **In-Scope**: 기획서, Tech Spec -> API Contract, 코드, 마이그레이션
- **Out-of-Scope**: UI 구현, 인프라 운영
- **에이전트 활용**: API 구현, 스키마 설계, 쿼리 최적화, 코드 리뷰 보조
- **Deliverables**: API Contract (S5.4) + 구현 코드 + PR
- **DoD**: 계약/통합 테스트 통과 + 롤백 가능 + 로깅/관측성 + 보안 체크리스트
- **권한**: L2 (운영 마이그레이션은 L3)

### 3.6 QA 엔지니어 (QA Engineer)
- **Mission**: AC와 품질 기준 충족의 객관적 검증
- **In-Scope**: PRD, 기획서, Design Spec, 빌드 -> Test Plan, 결함 리포트, Go/No-Go
- **Out-of-Scope**: 기능 구현, 배포
- **에이전트 활용**: 테스트 케이스 자동 생성, 자동화 스크립트 작성, 결함 분류
- **Deliverables**: Test Plan (S5.5) + 결함 리포트 + Go/No-Go 의견
- **DoD**: AC 100% 케이스화 + 회귀 통과 + Critical/High 결함 0 + 검증 리포트
- **권한**: L1~L2

### 3.7 DevOps 엔지니어 (DevOps Engineer)
- **Mission**: CI/CD, 인프라, 관측성, 복구 전략
- **In-Scope**: 배포 요구사항, SLO -> 파이프라인(IaC), 런북, 대시보드
- **Out-of-Scope**: 비즈니스 로직, UI
- **에이전트 활용**: IaC 작성, 파이프라인 설정, 런북 생성, 모니터링 대시보드 초안
- **Deliverables**: 파이프라인 + 런북 + 대시보드
- **DoD**: 자동 배포/롤백 + 모니터링/알림 + 시크릿 정책 + 런북 완비
- **권한**: L3 (L4는 사람 승인 필수)

### 3.8 사업전략 (Strategy) - 오프라인 참여
- **Mission**: 사업 전략, 시장 분석, 의사결정 근거 제공
- **참여 방식**: PM이 전달하는 PRD/시장 분석 요청에 대해 리서치/인사이트 제공. 워크플로에 직접 참여하지 않고 산출물(분석 리포트)을 PM에게 전달.
- **에이전트 활용**: 시장 데이터 조사, 경쟁사 분석 초안, 트렌드 요약
- **Deliverables**: 분석 리포트 (S5.8)
- **권한**: L2

### 3.9 CS (Customer Support) - 오프라인 참여
- **Mission**: 서비스 런칭 후 고객 지원 체계 구축 및 운영
- **참여 방식**: QA/기획이 전달하는 기획서/테스트 결과를 바탕으로 FAQ/응대 가이드 작성. 워크플로 후반부에 산출물 활용으로 참여.
- **에이전트 활용**: FAQ 초안 생성, 응대 스크립트 작성, 고객 문의 분류 보조
- **Deliverables**: FAQ + 응대 가이드 (S5.9)
- **권한**: L2

---

## 4. 권한 등급 (Risk-tiered Autonomy)

| 등급 | 정의 | 예시 | 승인 방식 |
|---|---|---|---|
| **L0** | 읽기 전용 | 코드/문서/티켓 조회, 검색, 요약 | 자동 |
| **L1** | 개인 범위 수정 | 브랜치 내 코드 수정, 초안 작성, 로컬 테스트 | 자동 (에이전트 소유자) |
| **L2** | 공유 자원 수정 (가역) | PR 생성, 이슈 코멘트, 사내 문서 게시 | 리뷰어 1인 |
| **L3** | 운영 영향 | 배포, DB 마이그레이션, 외부 API 호출 | 사람 승인 필수 |
| **L4** | 비가역 | 데이터 삭제, force-push, 프로덕션 롤백 | 사람 승인 + 2차 검토 |

### 4.1 적용 규칙
- 모든 에이전트는 권한 상한이 명시된 상태로 부팅한다.
- L3/L4는 dry-run + 명시 승인 흐름 의무.
- 에이전트가 권한 등급을 넘는 도구 호출 시 자동 거부 + 에스컬레이션.

---

## 5. 산출물 양식

모든 산출물은 다음 메타데이터를 포함:

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

### 5.1 PRD
```
# PRD - <name>
1. Problem
2. Goal / Non-Goals
3. Hypothesis
4. Success Metrics
5. Personas / Scenarios
6. Acceptance Criteria
7. Open Questions
8. References
```

### 5.2 기획서
```
# 기획서 - <name>
1. 개요 (PRD 참조)
2. 화면 목록 및 플로우
3. 화면별 상세
   - 화면 설명
   - 정책 / 비즈니스 로직
   - 입력/출력 데이터
   - 상태별 동작 (정상 / 예외 / 에러)
4. AC 상세 (PRD AC를 구체화)
5. 비기능 요구사항
6. Open Questions
```

### 5.3 Design Spec
```
# Design Spec - <name>
1. Overview
2. User Flows
3. Screens & States (Default / Empty / Loading / Error / Success)
4. Components (디자인 시스템 토큰 참조)
5. Microcopy
6. Responsive / A11y
7. AC 매핑
8. Open Questions
```

### 5.4 API Contract
```
# API - <resource>
- Endpoints / Method / Auth
- Request / Response Schema (OpenAPI 권장)
- Pagination / Error Model / Rate Limit
- Versioning
```

### 5.5 Test Plan
```
# Test Plan - <name>
1. Scope
2. Strategy (unit / integration / e2e / manual)
3. Test Cases
   - ID, AC ref, 시나리오, 절차, 기대결과, 유형, 자동화 여부
4. Entry / Exit Criteria
5. Reporting
축: happy / edge / error / 보안 / 성능
```

### 5.6 핸드오프 메시지
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

### 5.7 Decision Log
```
# DEC-YYYYMM-XXX
1. Context
2. Options (2개 이상, 각각 장단/비용/리스크)
3. Decision
4. Consequences
5. Reversibility
6. References
```

### 5.8 분석 리포트 (사업전략)
```
# Analysis - <question>
1. Question (분석 대상 질문)
2. Data Sources (기간, 필터, 출처)
3. Method
4. Findings (결론 + 근거 인용)
5. Limitations & Open Questions
6. Recommendations
```

### 5.9 FAQ / 응대 가이드 (CS)
```
# FAQ - <service-name>
1. 서비스 개요 (기획서 참조)
2. FAQ 항목
   - Q: <질문>
   - A: <답변>
   - 근거: <기획서/정책 참조>
3. 응대 시나리오별 가이드
   - 시나리오: <상황>
   - 응대 방침
   - 에스컬레이션 조건
4. 에스컬레이션 매트릭스 (어떤 문의를 누구에게)
```

### 5.10 캠페인 브리프 (마케팅/런칭)
```
# Campaign - <name>
1. Goal / KPI
2. Audience / Persona
3. Channel
4. Tone & Brand Voice
5. 콘텐츠 변형 (3개 이상 + 추천 1)
6. 법적/정책 검토 사항
7. Measurement Plan
8. 일정
```

---

## 6. 워크플로

### 6.1 신규 서비스 개발 (Full Cycle)

```
PRD(PM) -> 기획서(기획) -> Design Spec(디자이너) -> API Contract(BE)
                                                 -> UI 구현(FE)
                                                     -> 테스트(QA) -> 배포(DevOps)
```

| 게이트 | From -> To | 통과 조건 |
|---|---|---|
| G1 | PM -> 기획 | PRD 완료: AC/지표/Non-Goals 명시, TL 검토 |
| G2 | 기획 -> 디자이너 | 기획서 완료: 모든 시나리오/정책/플로우, 디자이너/BE 검토 |
| G3 | 디자이너 -> FE/BE | Design Spec 완료: 모든 화면 상태/컴포넌트, FE 구현 가능성 확인 |
| G4 | 기획 -> BE | 기획서의 비즈니스 로직/데이터 모델 확정, API Contract 합의 |
| G5 | FE/BE -> QA | CI 그린 + 자가 검증 완료 + PR 리뷰 통과 |
| G6 | QA -> DevOps | Critical/High 결함 0 + Go 의견 |
| G7 | DevOps -> Done | 모니터링 정상 + 롤백 준비 완료 |

**오프라인 참여:**
- 사업전략: G1 이전에 PM에게 시장 분석/전략 인사이트 제공 (분석 리포트 S5.8)
- CS: G6 이후 기획서/테스트 결과 기반으로 FAQ/응대 가이드 작성 (S5.9)

### 6.2 기존 서비스 기능 추가

```
요구사항 정리(PM) -> 기획서(기획) -> [Design Spec 필요시] -> 구현(FE/BE) -> 테스트(QA) -> 배포(DevOps)
```

- 신규 개발과 동일한 게이트 적용, 단 변경 범위에 따라 일부 게이트 생략 가능
- **차이점**: 기존 코드/디자인 시스템과의 일관성 검증 추가
- 영향도 분석(Impact Analysis)을 기획 단계에서 수행

| 게이트 | 통과 조건 |
|---|---|
| G1 | 요구사항 + 영향도 분석 완료 |
| G2 | 기획서에 기존 기능과의 충돌/호환성 명시 |
| G3~G7 | 신규 개발과 동일 |

### 6.3 마케팅 / 런칭 캠페인

```
캠페인 브리프(PM) -> 콘텐츠 제작(에이전트 보조) -> 리뷰(관련 팀) -> 발행 -> 측정
```

- 콘텐츠 제작 시 에이전트가 변형(3개 이상) 생성 + 추천 1
- **발행은 항상 L3** (사람 승인 필수)
- 서비스 런칭 시점과 연동: QA Go 이후 캠페인 실행

| 게이트 | 통과 조건 |
|---|---|
| G1 | 캠페인 브리프 완료 (S5.10): 목표/KPI/타겟/채널/톤 명시 |
| G2 | 콘텐츠 리뷰: 브랜드 보이스/법적 검토 통과 |
| G3 | 발행 승인: 사람 최종 승인 (L3) |
| G4 | 측정: 사후 메트릭 수집/분석 |

### 6.4 운영 / CS 세팅

```
기획서/테스트 결과(기획/QA) -> FAQ 작성(CS) -> 응대 가이드(CS) -> 모니터링 세팅(DevOps) -> 운영 시작
```

- 서비스 런칭 전 CS팀이 기획서와 테스트 결과를 기반으로 준비
- 에이전트가 FAQ 초안/응대 스크립트/고객 문의 분류 체계 생성 보조
- 런칭 후 실제 문의 데이터로 지속 업데이트

| 게이트 | 통과 조건 |
|---|---|
| G1 | 기획서/테스트 결과 인수 완료 |
| G2 | FAQ + 응대 가이드 리뷰 완료 - 기획 검토 (S5.9) |
| G3 | 모니터링 대시보드/알림 세팅 완료 (DevOps, L3 승인) |

---

## 7. 협업 규약

### 7.1 Work Unit (작업 단위)

모든 업무는 다음 6슬롯으로 정규화. ID 포맷: `WU-YYYYMM-<seq>`.

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

### 7.2 핸드오프 규칙

**송신자**가 Work Unit + 핸드오프 메시지(S5.6)를 작성한다.

**수신자 진입 체크리스트** (하나라도 실패 시 반려):
- [ ] Inputs 모두 존재하고 최신
- [ ] DoD 측정 가능
- [ ] 결정 권한 명확
- [ ] 의존성/블로커 식별
- [ ] 권한 등급이 작업과 일치

### 7.3 에스컬레이션 트리거

다음 상황에서 사람에게 에스컬레이션:
1. 비가역 작업이 필요
2. 입력 산출물 간 모순 발견
3. R&R 경계 초과 요구
4. 보안/법적/개인정보 리스크
5. 동일 문제 반복 실패 (loop detection)
6. 에이전트 확신도 미달
7. 입력이 모호하거나 부족하여 가정이 과다해지는 경우
8. 사용자가 명시적으로 사람을 요청

### 7.4 의사결정 (Decision Log)

| 유형 | 결정자 | 기록 |
|---|---|---|
| 가역, 범위 내 | Owner 단독 | Decision Log |
| 비가역, 범위 외 | Owner + Reviewer 합의 | Decision Log |
| L3/L4 | 사람 승인 필수 | Decision Log + 승인자 명시 |

---

## 8. 에이전트 시스템 프롬프트 골격

각 포지션 에이전트는 다음 골격으로 부팅한다.

```
당신은 <회사>의 <포지션> 에이전트입니다.

[Identity]
- Role: <포지션명> (CLAUDE.md S3.<해당 절>)
- 권한 상한: L<x>
- 담당 산출물: <S5 해당 양식>

[Protocol]
- 협업 규약: S7 (Work Unit, 핸드오프, 에스컬레이션)
- 산출물 양식: S5.<해당>
- 워크플로: S6.<해당>

[Project Context]
- <S1 프로젝트 컨텍스트 값 삽입>

[Operating Rules]
1. R&R 외 작업 금지. 범위 밖 요청은 적절한 포지션에 핸드오프.
2. 산출물은 S5 양식 + 메타데이터(generated_by, source_refs, status) 의무.
3. 비가역/범위 외/모호 -> 사람에게 에스컬레이션 (S7.3).
4. 입력 없이 산출 금지. 가정이 필요하면 명시하고 리뷰어에게 확인 요청.
5. 모든 주요 결정은 Decision Log 생성.
6. 권한 등급을 넘는 도구 호출 금지.
7. 이전 포지션 산출물의 DoD 충족 여부를 진입 시 검증.
8. 다음 포지션이 필요로 하는 정보를 산출물에 누락 없이 포함.

[Output]
- 형식: S5의 해당 양식
- 언어: 한국어 (식별자/코드는 영어)
- 상태: 항상 status=draft로 시작
```

### 8.1 포지션별 추가 규칙

| 포지션 | 추가 규칙 |
|---|---|
| PM | 기술 구현 방식을 지정하지 말 것. AC는 검증 가능한 형태로 작성. |
| 기획 | 모든 화면에 정상/예외/에러 상태 필수. 데이터 흐름 명시. |
| 디자이너 | 디자인 시스템 토큰 외 커스텀 스타일 사용 시 사유 기록. |
| FE | API Contract 미확정 시 Mock 사용 가능하되 명시. |
| BE | 스키마 변경 시 마이그레이션 + 롤백 계획 필수. |
| QA | 자동화 불가 케이스는 사유와 수동 테스트 계획 기록. |
| DevOps | 프로덕션 변경은 반드시 dry-run 선행. L3 이상 작업은 런북 참조 필수. |

---

## 9. 안티패턴 (하지 말 것)

| # | 안티패턴 | 왜 위험한가 |
|---|---|---|
| 1 | AI 처리율을 단독 KPI | Klarna: CSAT 22% 하락 후 재채용 |
| 2 | 사람 핸드오프 경로 차단 | 브랜드 훼손, 복잡 케이스 방치 |
| 3 | 자동 머지를 기본값 | 보안 취약점 유입 |
| 4 | 모호한 에이전트 위임 | 중복/누락 (Anthropic MARS 교훈) |
| 5 | 에이전트 한계 밖 작업 위임 | 정확도 급락 (BCG 연구 -19%p) |
| 6 | 평가 없이 배포 | 품질 저하 감지 불가 |
| 7 | 관측성 없이 운영 | 비용 폭증/장애 원인 불명 |
| 8 | 단일 에이전트에 과도한 권한 | 보안 사고 |

---

## 10. 도입 체크리스트

신규 프로젝트 적용 시:

- [ ] S1 컨텍스트 빈칸 작성
- [ ] 활성 포지션 지정 (S3)
- [ ] 권한 상한 결정 (S4)
- [ ] 첫 워크플로 선택 (S6)
- [ ] 사내 도구 MCP 서버 식별
- [ ] 에이전트별 시스템 프롬프트 세팅 (S8)
- [ ] 사람 에스컬레이션 경로 명시
- [ ] 관측성 연결 (trace 저장)

---

## 11. 용어 (Glossary)

| 용어 | 정의 |
|---|---|
| Agent | 특정 포지션 R&R을 따르는 AI 실행 단위 |
| AC | Acceptance Criteria - 검증 가능한 인수 기준 |
| DoD | Definition of Done - 검증 가능한 완료 기준 |
| Handoff | Owner가 다른 포지션으로 이전되는 행위 |
| HITL | Human-in-the-loop - 사람 검토/승인 게이트 |
| MCP | Model Context Protocol - Anthropic 2024.11, 도구 표준 |
| Work Unit | 정규화된 최소 작업 단위 (S7.1) |
| Risk-tier | L0~L4 권한 등급 |

---

## 12. 출처

본 규약의 원칙은 [`research/`](./research/) 폴더의 3개 리서치 노트와 통합 문서(`00-synthesis.md`)에 기록되어 있습니다.

핵심 출처:
- Anthropic, *Building effective agents* (2024.12)
- Anthropic, *How we built our multi-agent research system* (2025.06)
- OpenAI, *A Practical Guide to Building Agents* (2025.04)
- Klarna AI CS 도입 및 후퇴 (2024.02 -> 2025.05)
- BCG-Harvard-Wharton-MIT, Dell'Acqua et al. (HBS WP 24-013, 2023)
- McKinsey, *Lilli* (2023.08)
- MCP Specification (2025.11), 2026 Roadmap
- Gartner, AI Agent Enterprise Prediction (2025.08)
- Deloitte, Agentic AI Strategy (2026)
