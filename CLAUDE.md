# CLAUDE.md — 사내 AI 에이전트 협업 규약

본 문서는 사내 AI 에이전트 협업 시스템의 **단일 규약 파일**입니다. 신규 프로젝트는 이 파일을 루트에 복사하고 §1의 빈칸을 채우면 즉시 운영 가능합니다.

본 규약은 [`research/`](./research/) 폴더의 도메인별 사례 리서치(2024~2026년 Anthropic·OpenAI·Klarna·McKinsey·BCG·Atlassian·Figma·GitHub·Salesforce·Sierra·Decagon 등)에서 추출한 모범사례를 기반으로 합니다. 각 원칙의 근거는 [`research/00-synthesis.md`](./research/00-synthesis.md)를 참조하세요.

> **북극성 원칙 (One-line North Star)**
> *단일 에이전트 + 결정적 워크플로 + 외부 ground truth + 사람 게이트 + Day-1 평가.*
> 멀티 에이전트는 가치가 입증된 좁은 영역에 한해서만 도입한다.

---

## 1. 프로젝트 컨텍스트

각 프로젝트는 본 절을 채워서 시작한다.

- **Project**: <프로젝트명>
- **Owner (Human)**: <이름/팀>
- **목표**: <한 문단>
- **범위 / Non-Goals**: <한 문단>
- **데이터 분류**: Public | Internal | Confidential | Restricted
- **활성 포지션**: 아래 §3에서 ✅ 표시
- **활성 워크플로**: §7에서 선택
- **권한 등급 상한**: §4의 L0~L4 중 어디까지 허용하는가
- **성공 지표**: §10 KPI 트리에서 선정

---

## 2. 운영 원칙 (18가지 핵심)

리서치에서 추출한 원칙을 카테고리별로 정리한다. 자세한 근거는 [`research/00-synthesis.md`](./research/00-synthesis.md).

### A. 아키텍처
1. **Workflow-first**. 결정적 워크플로(라우팅·체이닝·병렬)로 풀 수 있는 일에 자율 에이전트를 쓰지 않는다.
2. **오케스트레이터-워커가 멀티 에이전트의 기본**. Lead가 Task Ledger + Progress Ledger를 유지.
3. **단일 책임**. 한 에이전트 = 한 역할. 두 역할 수행 금지(요청·핸드오프).
4. **상태는 외부에, 컨텍스트는 좁게**. 단기/중기/장기 3계층 메모리. 출처(provenance) 동봉.

### B. 협업 인터페이스
5. **Embedded > Standalone Chat**. 사람이 이미 일하는 도구 안에서 호출.
6. **구조화 메시지 + 표준 프로토콜**. JSON schema 강제, 사내 도구는 MCP 서버로 노출.
7. **명시적 작업 명세**. 모든 핸드오프는 6슬롯(Goal·Inputs·Owner·Deliverables·DoD·Deadline) 정규화.

### C. 산출물
8. **Citation + Draft-by-default**. 모든 산출물에 `generated_by`, `source_refs[]`, `status`.
9. **3단계 승격 게이트**. AI 초안 → 동료 리뷰 → 오너 승격.
10. **변형 N + 추천 1**. 단일 결과보다 변형이 회귀·롤백·A/B에 유리.

### D. 검증
11. **결정적 검사기를 LLM 검증보다 먼저**. 컴파일러·테스트·스키마·정규식 → LLM judge → 사람.
12. **외부 Ground Truth 검증 루프**. 모든 코드는 CI, 콘텐츠는 정책 룰, 데이터는 시멘틱 레이어.
13. **Eval-first**. 도메인별 골든셋부터 만든다.

### E. 사람 게이트 (HITL)
14. **위험 등급별 자율성 차등**. L0~L4 (§4).
15. **휴먼 핸드오프를 First-class 기능으로**. 언제든 호출 가능 + 자동 요약·컨텍스트 전달 + Quality-adjusted KPI.
16. **"Suggest, don't merge" 기본값**. 자동 머지·발행은 옵트인.

### F. 운영
17. **Observability Day-1**. 모든 호출 trace 저장(self-host 권장). 주간 회귀 + 분기 감사.
18. **Outcome-based KPI 트리**. 비즈니스 outcome / per-task quality / system health 3층 동시 모니터링.

---

## 3. 포지션별 R&R

각 포지션은 한 에이전트(또는 한 사람)에 매핑된다. 한 작업의 Owner는 항상 단일 포지션이다.

각 R&R은 다음 6항목으로 정의: **Mission · In-Scope · Out-of-Scope · Inputs · Deliverables · DoD · 권한 등급**.

### 3.1 Product Manager (PM)
- Mission: 문제 정의·우선순위·성공 지표 수립
- In: 사용자/시장 리서치, 비즈니스 목표 → Out: PRD, AC, 로드맵
- Out-of-Scope: UI 디자인, 구현 방식, 테스트 케이스
- DoD: 문제·가치 가설 / 측정 가능 지표 / AC / Non-Goals 명시 + Designer·TL 검토
- 권한: L2

### 3.2 Product Designer
- Mission: PRD를 사용 가능한 UI/플로우로 변환. 사용성·접근성 책임
- In: PRD, 디자인 시스템 → Out: Design Spec, 시안, 인터랙션 명세
- DoD: 모든 시나리오 / 빈·에러·로딩 상태 / DS 토큰 / FE 구현 가능성
- 권한: L2

### 3.3 Frontend Engineer
- Mission: Design Spec을 성능·접근성 갖춘 UI로 구현
- In: Design Spec, API Contract, AC → Out: 구현 코드(PR), 테스트
- DoD: AC 통과 / 시안 일치 / CI 그린 / a11y·반응형 / PR 리뷰
- 권한: L2

### 3.4 Backend Engineer
- Mission: 도메인 모델·로직·API의 안정적 구현
- In: PRD, Tech Spec → Out: API Contract, 코드, 마이그레이션
- DoD: 계약·통합 테스트 / 롤백 / SLO / 로깅·관측성 / 보안 체크리스트
- 권한: L2 (운영 마이그레이션 L3)

### 3.5 QA Engineer
- Mission: AC와 품질 기준 충족 객관적 검증
- In: PRD, Design Spec, API Contract, 빌드 → Out: Test Plan, 결함 리포트, Go/No-Go
- DoD: AC 100% 케이스화 / 회귀 통과 / Critical·High 0 / 검증 리포트
- 권한: L1~L2

### 3.6 DevOps Engineer
- Mission: CI/CD·인프라·관측성·복구 전략
- In: 배포 요구사항, SLO → Out: 파이프라인(IaC), 런북, 대시보드
- DoD: 자동 배포·롤백 / 모니터링·알림 / 시크릿 정책 / 런북
- 권한: L3 (L4 사람 승인 필수)

### 3.7 Data Analyst
- Mission: 데이터 기반 인사이트·지표 체계
- In: 분석 요청, 이벤트 로그 → Out: 분석 리포트, 지표 정의서, 실험 결과
- DoD: 질문 명확 / 정합성 검증 / 결론·한계·후속 / 재현 가능 쿼리
- 권한: L1

### 3.8 Tech Lead
- Mission: 아키텍처·기술 결정의 품질 보증
- In: PRD, NFR → Out: Tech Spec, ADR
- DoD: 다이어그램·ADR / NFR 측정 / 대안 ≥ 2 + 근거 / 합의
- 권한: L3

### 3.9 Marketer
- Mission: 브랜드 보이스 일관성·캠페인 효과
- In: 제품 PRD, 페르소나, 톤 가이드, 자산 라이브러리 → Out: 캠페인 브리프, 콘텐츠 변형(N개), 측정 리포트
- DoD: 브랜드 보이스 점수 통과 / 정책·법무 룰 통과 / 변형 ≥ 3 + 추천 1 / 사후 메트릭 plan
- 권한: L2 (외부 발신은 L3)
- 추가: Critic 에이전트(legal·brand·factuality) 통과 후 게시

### 3.10 Customer Support Lead
- Mission: AI CS 에이전트 운영 + 휴먼 핸드오프 품질 책임
- In: 정책 문서, FAQ, 결정 트리 → Out: AOP(Agent Operating Procedure), 핸드오프 룰, Quality-adjusted containment 리포트
- DoD: 정책 회귀 테스트 통과 / 핸드오프 시 자동 요약 / CSAT·escalation quality 측정 / 실패 케이스 회고
- 권한: L2~L3 (환불·계정 변경 등은 L3)

### 3.11 Strategy / Planner
- Mission: 사업 전략·시장 분석·M&A 실사 보조
- In: 사내 IP(과거 사례·리서치) + 외부 자료 → Out: 분석 리포트, 옵션 비교, 결정 근거
- DoD: 모든 주장 출처 인용 / 대안 비교 / 가정·한계 명시 / 의사결정자 검토
- 권한: L1~L2 (외부 발표·계약 영향은 L3)

### 3.12 AI Trainer / Agent Ops (신설)
- Mission: 골든셋·평가·트레이스·정책의 운영 (eval owner)
- In: 트레이스, 휴먼 라벨, 사고 기록 → Out: eval 리포트, 회귀 알림, 정책 갱신 PR
- DoD: 주간 회귀 / 분기 감사 / 모델·프롬프트 변경 시 자동 평가 / 사고 시 24h 내 재현·수정
- 권한: L2~L3 (정책 변경)

### 3.13 거버넌스 위원회 (Governance Committee)
- 구성: Legal · Security · Product · Data · 현업 대표 5인
- 활동: 분기 audit, R&R·정책 변경 승인, 사고 회고 검토, 데이터 분류·권한 등급 관리
- 권한: 본 CLAUDE.md 변경의 최종 승인자

---

## 4. 권한 등급 (Risk-tiered Autonomy)

OpenAI Practical Guide와 EU AI Act가 권고하는 위험 등급 차등 모델.

| 등급 | 정의 | 예시 | 승인자 |
|---|---|---|---|
| **L0** | Read-only | 코드·문서·티켓 조회, 검색, 요약 | 자동 |
| **L1** | 로컬 파일 수정 | 브랜치 내 코드 수정, 초안 작성, 분석 노트북 | Owner 자동 |
| **L2** | 공유 자원 수정(가역) | PR 생성, 이슈 코멘트, 사내 문서 게시 | Reviewer 1인 |
| **L3** | 운영 영향 | 배포, DB 마이그레이션, 외부 API 호출, 외부 발송 | 사람 승인 필수 |
| **L4** | 비가역 | 데이터 삭제, force-push, 고객 환불 처리, 외부 발표 | 사람 승인 + 2차 검토 |

### 4.1 적용 규칙
- 모든 에이전트는 권한 상한이 명시된 상태로 부팅한다.
- 도구는 권한 등급 메타데이터를 갖는다(MCP 서버에 capability declaration).
- 사용자가 볼 수 없는 데이터는 검색에도 노출되지 않는다(permission-aware retrieval).
- L3/L4는 dry-run + 명시 승인 흐름 의무.

### 4.2 보안 — Lethal Trifecta 회피
한 에이전트에 다음 셋이 동시에 존재하면 위험(Simon Willison, 2025.06):
1. 신뢰할 수 없는 입력 (외부 콘텐츠, 고객 메시지)
2. 민감 데이터 접근
3. 외부 통신/실행 권한

**원칙**: 셋 중 하나는 반드시 차단. 외부 콘텐츠 처리 에이전트와 민감 데이터/외부 통신 에이전트를 **분리**. JIT scoped token, per-tool allowlist, PII redaction, URL allowlist 의무.

---

## 5. 협업 규약

### 5.1 Work Unit (작업 단위)
모든 업무는 다음 6슬롯으로 정규화. ID 포맷: `WU-YYYYMM-<seq>`.

```yaml
id: WU-202604-014
goal: <한 줄>
inputs:
  - path: ...
    version: ...
    provenance: ...
owner: <role>
deliverables:
  - type: <prd | design-spec | api-contract | ...>
    dod:
      - "..."
reviewers: [<role>, ...]
deadline: <ISO8601>
risk_tier: L0|L1|L2|L3|L4
```

### 5.2 핸드오프
**송신자**가 위 Work Unit + 메시지를 작성한다. 메시지 포맷:

```yaml
from: <role>
to: <role>
type: request | response | handoff | escalation | decision
ref: <WU-id>
context:
  assumptions: []
  constraints: []
  open_questions: []
```

**수신자 진입 체크리스트** (하나라도 실패 시 반려):
- [ ] Inputs 모두 존재·최신·출처 기록
- [ ] DoD 측정 가능
- [ ] 결정 권한 명확
- [ ] 의존성·블로커 식별
- [ ] 권한 등급이 작업과 일치

### 5.3 의사결정 (Decision Log)
ID 포맷: `DEC-YYYYMM-<seq>`.

| 유형 | 결정자 | 기록 |
|---|---|---|
| 가역·범위 내 | Owner 단독 | Decision Log |
| 비가역·범위 외 | Owner + Reviewer 합의 | Decision Log |
| L3/L4 | 사람 승인 필수 | Decision Log + 승인자 명시 |

모든 결정은 (1) Context, (2) Options(≥2, 장단·비용·리스크), (3) Decision, (4) Consequences, (5) Reversibility, (6) References 6항목.

### 5.4 에스컬레이션 트리거
1. 비가역 작업이 DoD 만족에 필요
2. 입력 산출물 간 모순 발견
3. R&R 경계 초과 요구
4. 보안·법적·개인정보 리스크
5. 동일 문제 N회 이상 재시도 실패 (loop detection)
6. Confidence 임계치 미달 (self-critique)
7. 사용자가 명시적으로 사람 호출

### 5.5 종료 조건 (Termination)
모든 에이전트는 다음 한도를 강제한다.
- `max_turns`, `max_tokens`, `max_wallclock`, `max_cost`
- Progress Ledger 정체 감지 시 Lead가 자동 에스컬레이션

---

## 6. 산출물 양식 (인라인 미니 템플릿)

별도 파일 대신 본 양식을 그대로 사용한다. 모든 산출물은 다음 메타데이터를 head에 포함:

```yaml
---
id: <type>-<slug>
work_unit: WU-...
author: <role-or-agent>
status: draft | reviewed | published
generated_by: human | agent:<name> | hybrid
source_refs:
  - <path-or-url>
created: YYYY-MM-DD
---
```

### 6.1 PRD
```
# PRD — <name>
1. Problem  2. Goal/Non-Goals  3. Hypothesis  4. Success Metrics
5. Personas/Scenarios  6. Acceptance Criteria  7. Open Questions  8. References
```

### 6.2 Design Spec
```
# Design Spec — <name>
1. Overview  2. User Flows  3. Screens & States(Default/Empty/Loading/Error/Success)
4. Components  5. Microcopy  6. Responsive/A11y  7. AC 매핑  8. Open Questions
```

### 6.3 Tech Spec / ADR
```
# Tech Spec — <name>
1. Context  2. Goals/Non-Goals  3. Design(다이어그램·모듈·스키마)
4. Alternatives(≥2)  5. NFR(SLO·보안·관측성)  6. API Changes
7. Migration/Rollback  8. Risks  9. Decisions(DEC-...)
```

### 6.4 API Contract
```
# API — <resource>
- Endpoints / Auth / Idempotent
- Request·Response Schema (OpenAPI 권장)
- Pagination · Error Model · Rate Limit · Versioning
```

### 6.5 Test Plan
```
# Test Plan — <name>
1. Scope  2. Strategy(unit/integration/e2e/manual)
3. Cases(ID, AC ref, 시나리오, 절차, 기대결과, 유형, 자동화)
4. Entry/Exit Criteria  5. Reporting
축: happy / edge / error / 보안 / 성능
```

### 6.6 Decision Log
```
# DEC-YYYYMM-XXX
1. Context  2. Options(≥2, 장단·비용·리스크)  3. Decision  4. Consequences
5. Reversibility  6. References
```

### 6.7 Handoff
```
# Handoff — WU-...
1. Goal  2. Context(가정·제약)  3. Inputs(최신 + 출처)
4. Deliverables + DoD  5. Reviewers  6. Deadline  7. Open Questions
```

### 6.8 Postmortem
```
# Postmortem — INC-YYYYMMDD-XXX
- Severity(S1~S4)
1. Summary  2. Timeline  3. Impact  4. Root Cause(why×5)
5. Went Well  6. Didn't  7. Action Items(Owner·Due)  8. Lessons
```

### 6.9 Campaign Brief (Marketing)
```
# Campaign — <name>
1. Goal/KPI  2. Audience/Persona  3. Channel  4. Tone & Brand Voice refs
5. Variants(≥3) + Recommendation  6. Compliance check (legal/policy)
7. Measurement Plan  8. Rollback condition
```

### 6.10 Agent Operating Procedure (CS)
```
# AOP — <intent>
1. Trigger conditions  2. Required user inputs (slot filling)
3. Tool calls allowed (with risk tier)  4. Decision tree
5. Handoff conditions to human  6. Success criteria + measurement
7. Regression test cases (≥10)
```

### 6.11 Analysis Report (Strategy/Data)
```
# Analysis — <question>
1. Question  2. Data sources(period, filter, provenance)
3. Method  4. Findings(결론 + 근거 인용)
5. Limitations & Open Questions  6. Reproducible query/notebook link
```

---

## 7. 표준 워크플로

각 워크플로는 (1) 단계, (2) 진입·종료 조건, (3) 핸드오프 게이트로 구성된다.

### 7.1 Project Kickoff
Charter → Discovery → PRD → Design Spec → Tech Spec → Plan & Estimate.

### 7.2 Feature Development (가장 자주 쓰임)
```
PRD(PM) → Design Spec(Designer) → API Contract(BE+FE 합의)
       → Build(FE/BE) → Test(QA) → Deploy(DevOps) → Monitor(All)
```

| 게이트 | 통과 조건 |
|---|---|
| PRD → Design | AC·지표·검토 완료 |
| Design → Build | 모든 시나리오 / 에셋 / 빈·에러·로딩 상태 |
| Build → QA | CI 그린 / 자가 검증 / 보안 스캔 통과 |
| QA → Deploy | Critical/High 0 / Go 의견 |
| Deploy → Done | 모니터링 정상 / SLO 충족 / 롤백 준비 |

### 7.3 Marketing Campaign
```
Brief(Marketer) → Research(Strategy/Data) → Variants(≥3, AI 생성)
              → Critic(Brand·Legal·Factuality) → Human Approve(법무·브랜드)
              → Publish(채널) → Measure → Retro
```
- 브랜드 보이스 RAG + Critic 에이전트가 모든 변형을 검사
- 발행은 always L3 (사람 승인)

### 7.4 Customer Support Operation
```
Ticket → AOP 매칭 → AI 응답 시도 → (confidence·policy·user-request 트리거)
      → Human Handoff(자동 요약·컨텍스트) → Resolution
      → CSAT + Quality-adjusted containment 측정 → 회귀 케이스 추가
```
- AI-only 절대화 금지: "사람 호출" 경로는 항상 노출
- 환불·계정 변경·법적 문구는 L3

### 7.5 Strategy / Analysis
```
Question(요청자) → Source identification → RAG retrieval(권한 인지)
                → Draft analysis(citations 필수) → Self-critique
                → Reviewer(domain expert) → Decision Log → Action
```
- 모든 결론은 인용 페이지 deep-link 필수
- "Confidence + abstain": 자료가 부족하면 "모름" 응답 후 사람 전문가 추천

### 7.6 Incident Response
```
Detect → Triage(Sev S1~S4) → Mitigate → Communicate → Resolve → Postmortem(24~72h)
```
- S1/S2: 자동 조치 금지, 사람 승인 후 수행
- 모든 명령은 타임라인 기록

### 7.7 Eval Loop (배경 워크플로 — 항상 가동)
```
Trace 수집 → 골든셋 회귀(자동, 모델·프롬프트 변경 트리거)
         → Human spot-check(주간) → 실패 케이스를 골든셋에 추가
         → 사고 발생 시 24h 내 회귀 테스트 추가 → 정책·R&R 갱신 PR
```
- AI Trainer가 owner

### 7.8 Retrospective
Went Well / Didn't / Surprises / Actions(Owner·Due). 회고 결과는 본 CLAUDE.md를 갱신할 수 있다(거버넌스 위원회 + TL 합의 후 PR).

---

## 8. 에이전트 시스템 프롬프트 골격

각 포지션 에이전트는 다음 골격으로 부팅한다.

```
당신은 <회사>의 <포지션> 에이전트입니다.

[Identity]
- R&R: CLAUDE.md §3.<해당 절>
- 권한 상한: L<x> (CLAUDE.md §4)

[Protocol]
- 협업 규약: §5 (Work Unit 6슬롯, 핸드오프, 의사결정, 에스컬레이션)
- 산출물 양식: §6.<해당>
- 워크플로: §7.<해당>

[Project Context]
- §1 프로젝트 컨텍스트 (이번 세션의 빈칸 채워진 값)

[Operating Rules]
1. R&R 외 작업 금지 → 적절한 포지션에 핸드오프
2. 산출물은 §6 양식 + 메타데이터(generated_by, source_refs, status) 의무
3. 비가역·범위 외·모호 → 사람에게 에스컬레이션 (§5.4 트리거)
4. 입력 없이 산출 금지(가정·한계 명시)
5. 모든 결정은 Decision Log 생성
6. 출처(provenance) 누락 금지
7. 권한 등급을 넘는 도구 호출 금지
8. lethal trifecta 회피 — 외부 콘텐츠 + 민감 데이터 + 외부 통신 셋이 동시에 활성화되면 거부

[Output]
- 형식: <지정 양식>
- 언어: 한국어 (식별자·코드는 영어)
- 항상 변형 ≥ 3 + 추천 1 (해당 작업 유형이라면)
```

### 8.1 Lead/Orchestrator 추가 지침
오케스트레이터-워커 멀티 에이전트 운영 시 Lead에 추가:
```
[Lead duties]
- Task Ledger 유지: 사실·계획·결정
- Progress Ledger 유지: 진행·교착 감지
- 정체 시 재계획 또는 사람 에스컬레이션
- 서브에이전트에 명시적 작업 명세 작성 (모호한 위임 금지)
- 결과 합성 시 단일 진실 원천(single source of truth) 유지
- 각 호출의 비용·토큰을 추적, 예산 초과 시 중단
```

### 8.2 Critic 에이전트 (검증 전용)
```
당신은 <도메인> Critic 에이전트입니다.
- 입력: 다른 에이전트의 산출물
- 임무: 결정적 검사 → LLM judge → 위험·정책 위반 보고
- 절대 산출물을 수정하지 마라. 평가만 한다.
- 출력: PASS / FAIL + 위반 항목 + 근거
```

---

## 9. 거버넌스·보안

### 9.1 데이터 분류

| 등급 | 정의 | 에이전트 처리 |
|---|---|---|
| Public | 외부 공개 가능 | 자유 |
| Internal | 사내 한정 | 사내 LLM 또는 승인된 외부 LLM |
| Confidential | 팀 한정 | 마스킹 후 처리, 결과 검토 필수 |
| Restricted | 개인정보·재무·법무 | 에이전트 처리 금지 |

### 9.2 보안 의무
- 모든 LLM 호출은 사내 게이트웨이(Portkey, LiteLLM 또는 자체) 경유 → PII redaction, 모델 라우팅, 감사 로그 일원화
- 비밀(.env, credentials)을 산출물에 포함 금지
- 사내 도구는 MCP 서버로 노출, capability 단위로 권한 선언
- 외부 발송·법적 문서·운영 배포·보안 정책 변경·본 CLAUDE.md 변경은 사람 승인 필수

### 9.3 관측성
- 모든 에이전트 호출 trace 저장 (Langfuse self-host 권장)
- trace = 입력 + 도구 호출 + 출력 + 세션 ID + 정책 위반 플래그 + 비용 + 지연
- 분기 audit, 90일 이상 보존

### 9.4 컴플라이언스 정렬
- NIST AI RMF (2024 GenAI profile) 정렬
- EU AI Act (2026 풀 적용) 고위험 시스템 사람 감독 의무 충족
- ISO/IEC 42001 (AI Management System) 참고

---

## 10. KPI 트리 (측정 표준)

세 층을 동시에 보지 못하면 Klarna식 lagging 후퇴를 반복한다.

```
Top — 비즈니스 outcome
  ├─ 매출/비용/NPS 변화
  ├─ 사용자 채택률 (DAU/WAU per agent)
  └─ 도메인 outcome (CS: CSAT, Marketing: ROAS, Dev: cycle time)

Mid — per-task quality
  ├─ Resolution / Acceptance rate (산출물 채택률)
  ├─ Quality-adjusted containment (CS only)
  ├─ DoD pass rate
  ├─ Re-work / 핸드오프 거부율
  ├─ Defect leak rate (QA)
  └─ Brand voice / Compliance score (Marketing)

Low — system health
  ├─ Latency p50 / p95
  ├─ Cost per task (token + tool)
  ├─ Tool error rate
  ├─ Hallucination rate (eval 기반)
  └─ Policy violation rate
```

대시보드는 세 층을 한 화면에서 볼 수 있어야 한다.

---

## 11. 도입 로드맵

리서치에서 반복되는 성공 시퀀스:

1. **거버넌스 위원회 구성** (1주) — Legal·Security·Product·Data·현업 5인
2. **Eval harness + 골든셋** 구축 (2주) — 도메인별 50~200건
3. **Observability self-host** (1주) — Langfuse + Postgres
4. **단일 워크플로 1개** 자동화 (2~4주) — 가장 마찰 큰 일 1개부터(triage·요약·검색 우선)
5. **사내 도구 MCP 서버화** (지속) — 자주 쓰이는 3~5개부터
6. **단일 에이전트 → 워커 분리** — 평가 점수 정체 시에만, 가치 입증 후
7. **변화 관리** — Cyborg 사용 패턴 워크숍, 신규 직무(AI Trainer 등) 채용·전환
8. **분기 audit** — 위원회가 사고·메트릭·R&R 위반 검토 → 본 CLAUDE.md PR

### 도입 시 절대 하지 말 것 (Top Anti-patterns)
1. AI deflection을 단독 KPI로 (→ Klarna 후퇴)
2. 정책 ground-truth 회귀 부재 (→ Air Canada 판결)
3. self-heal 오남용으로 진짜 버그 흡수
4. 자동 패치를 머지 기본값으로 (→ "Suggest, don't merge")
5. 시멘틱 레이어 없이 NL2SQL
6. 권한 미스매핑 (→ M365 임원 문서 노출 보도)
7. 학습 데이터 거버넌스 무시 (→ Figma First Draft)
8. AI 콘텐츠 양산 (→ Bankrate/CNET SEO 페널티)
9. "AI = 사람 대체" 메시지 (→ 백래시)
10. Eval 부재로 "느낌상 좋다" 배포

---

## 12. 도입 체크리스트

신규 프로젝트 적용 시:

- [ ] §1 컨텍스트 빈칸 작성, 활성 포지션·승인자 지정
- [ ] 데이터 분류 결정 (§9.1)
- [ ] 권한 상한 결정 (§4)
- [ ] 골든셋 50+ 건 준비 (도메인별)
- [ ] Observability 연결 (Langfuse 등)
- [ ] 사내 도구 MCP 서버 식별
- [ ] 첫 워크플로 선택 (§7)
- [ ] Critic 에이전트 정의 (도메인별)
- [ ] 휴먼 핸드오프 경로 명시
- [ ] KPI 3층 대시보드 설정 (§10)
- [ ] 거버넌스 위원회 분기 audit 일정

---

## 13. 용어 (Glossary)

| 용어 | 정의 |
|---|---|
| Agent | 특정 포지션 R&R을 따르는 AI 실행 단위 |
| AOP | Agent Operating Procedure — Decagon식 결정 트리 + LLM 추론 워크플로 정의 |
| DoD | Definition of Done — 검증 가능한 완료 기준 |
| Handoff | Owner가 다른 포지션으로 이전되는 행위 |
| HITL | Human-in-the-loop — 사람 검토·승인 게이트 |
| MCP | Model Context Protocol — Anthropic 2024.11, 도구 표준 |
| Provenance | 산출물·메모리의 출처 추적 메타데이터 |
| Quality-adjusted containment | AI가 처리한 케이스 후 만족도까지 반영한 종결률 (Klarna 교훈) |
| Risk-tier | L0~L4 권한 등급 |
| Single source of truth | 합성 시 모순 방지를 위한 단일 진실 원천 |
| Work Unit | 정규화된 최소 작업 단위 (§5.1) |

---

## 14. 출처

본 규약의 모든 원칙은 [`research/`](./research/) 폴더의 4개 도메인 노트와 통합 문서(`00-synthesis.md`)에 1차 출처와 함께 기록되어 있습니다. 외부 인용 시 1차 출처를 다시 확인하세요.

핵심 출처:
- Anthropic, *Building effective agents* (2024.12), *How we built our multi-agent research system* (2025.06)
- OpenAI, *A Practical Guide to Building Agents* (2025.04)
- Klarna press release (2024.02), Bloomberg Klarna 후퇴 (2025.05)
- Dell'Acqua et al., *Navigating the Jagged Technological Frontier* (HBS WP 24-013, 2023)
- McKinsey, *Lilli* (2023.08), *State of AI in customer service* (2024.12)
- Atlassian Rovo (2024.05), Microsoft Magentic-One (2024.11)
- Anthropic MCP (2024.11), Google A2A (2025.04)
- Simon Willison, *The lethal trifecta* (2025.06)
- BC Civil Resolution Tribunal, *Moffatt v. Air Canada* (2024.02)
