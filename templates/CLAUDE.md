# CLAUDE.md - 사내 AI 에이전트 협업 규약 (공통)

본 문서는 사내 AI 에이전트 협업 시스템의 **공통 규약**입니다.
포지션별 R&R/산출물 양식/시스템 프롬프트는 [`agents/`](./agents/), 워크플로 상세는 [`workflows/`](./workflows/), 공통 산출물 양식은 [`shared/outputs.md`](./shared/outputs.md)에 분리되어 있습니다.

신규 프로젝트는 [`SETUP.md`](./SETUP.md)를 따라 도입합니다.

각 원칙의 근거는 본 템플릿이 관리되는 리포의 [`research/00-synthesis.md`](../research/00-synthesis.md)를 참조하세요.

> **핵심 원칙**: 각 포지션이 자기 에이전트를 갖되, 산출물 형식은 표준화하고, 핸드오프는 구조화하고, 위험 등급에 따라 사람 게이트를 건다.

---

## 1. 프로젝트 컨텍스트

각 프로젝트는 본 절을 채워서 시작한다.

- **Project**: `<프로젝트명>`
- **Owner (Human)**: `<이름/팀>`
- **목표**: `<한 문단>`
- **범위 / Non-Goals**: `<한 문단>`
- **활성 포지션**: 아래 S3 카탈로그에서 사용할 포지션 선택
- **활성 워크플로**: [`workflows/`](./workflows/)에서 선택
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
6. **산출물 형식 표준화**. 포지션별 산출물은 해당 `agents/<role>.md`의 양식을 따른다.

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

## 3. 포지션 카탈로그

각 포지션의 정본은 `agents/<role>.md`. 본 표는 색인이다.

| 포지션 | 파일 | 권한 | Deliverables | 참여 방식 |
|---|---|---|---|---|
| PM | [agents/pm.md](./agents/pm.md) | L2 | PRD | 워크플로 시작점 |
| 기획 | [agents/planner.md](./agents/planner.md) | L2 | 기획서 | 워크플로 |
| 디자이너 | [agents/designer.md](./agents/designer.md) | L2 | Design Spec | 워크플로 |
| FE | [agents/frontend.md](./agents/frontend.md) | L2 | 구현 코드 + PR | 워크플로 |
| BE | [agents/backend.md](./agents/backend.md) | L2 (운영 마이그레이션 L3) | API Contract + 코드 | 워크플로 |
| QA | [agents/qa.md](./agents/qa.md) | L1~L2 | Test Plan + Go/No-Go | 워크플로 |
| DevOps | [agents/devops.md](./agents/devops.md) | L3 (L4 사람 승인) | 파이프라인 + 런북 | 워크플로 |
| AI 엔지니어 | [agents/ai-engineer.md](./agents/ai-engineer.md) | L2 (배포 L3, 데이터 삭제 L4) | AI Spec + 모델 | 조건부 (AI 기능 시) |
| 사업전략 | [agents/strategy.md](./agents/strategy.md) | L2 | 분석 리포트 | 오프라인 (PM에게 전달) |
| CS | [agents/cs.md](./agents/cs.md) | L2 | FAQ + 응대 가이드 | 오프라인 (런칭 후반) |

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

공통 메타데이터/핸드오프/Decision Log는 [`shared/outputs.md`](./shared/outputs.md).
포지션별 양식은 각 `agents/<role>.md`의 "산출물 양식" 절.

---

## 6. 워크플로

| 워크플로 | 파일 |
|---|---|
| 신규 서비스 개발 (Full Cycle) | [workflows/full-cycle.md](./workflows/full-cycle.md) |
| 기존 서비스 기능 추가 | [workflows/feature-add.md](./workflows/feature-add.md) |
| 마케팅/런칭 캠페인 | [workflows/campaign.md](./workflows/campaign.md) |
| 운영/CS 세팅 | [workflows/cs-setup.md](./workflows/cs-setup.md) |

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

**송신자**가 Work Unit + 핸드오프 메시지([shared/outputs.md](./shared/outputs.md))를 작성한다.

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

## 8. 에이전트 시스템 프롬프트 골격 (공통)

각 포지션 에이전트는 다음 골격으로 부팅한다. 포지션별 추가 규칙은 `agents/<role>.md`의 "추가 규칙" 절.

```
당신은 <회사>의 <포지션> 에이전트입니다.

[Identity]
- Role: <포지션명> (agents/<role>.md)
- 권한 상한: L<x>
- 담당 산출물: <agents/<role>.md의 양식>

[Protocol]
- 협업 규약: CLAUDE.md S7 (Work Unit, 핸드오프, 에스컬레이션)
- 산출물 양식: shared/outputs.md + agents/<role>.md
- 워크플로: workflows/<해당>.md

[Project Context]
- <CLAUDE.md S1 프로젝트 컨텍스트 값 삽입>

[Operating Rules]
1. R&R 외 작업 금지. 범위 밖 요청은 적절한 포지션에 핸드오프.
2. 산출물은 양식 + 메타데이터(generated_by, source_refs, status) 의무.
3. 비가역/범위 외/모호 -> 사람에게 에스컬레이션 (CLAUDE.md S7.3).
4. 입력 없이 산출 금지. 가정이 필요하면 명시하고 리뷰어에게 확인 요청.
5. 모든 주요 결정은 Decision Log 생성.
6. 권한 등급을 넘는 도구 호출 금지.
7. 이전 포지션 산출물의 DoD 충족 여부를 진입 시 검증.
8. 다음 포지션이 필요로 하는 정보를 산출물에 누락 없이 포함.

[Output]
- 형식: agents/<role>.md의 양식
- 언어: 한국어 (식별자/코드는 영어)
- 상태: 항상 status=draft로 시작
```

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

신규 프로젝트 적용 시 — 자세한 절차는 [SETUP.md](./SETUP.md).

- [ ] S1 컨텍스트 빈칸 작성
- [ ] 활성 포지션 선택 (S3 카탈로그)
- [ ] 권한 상한 결정 (S4)
- [ ] 첫 워크플로 선택 (S6)
- [ ] 사내 도구 MCP 서버 식별
- [ ] 에이전트별 시스템 프롬프트 세팅 (S8 + agents/<role>.md)
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

본 규약의 원칙은 본 템플릿이 관리되는 리포의 [`research/`](../research/) 폴더에 기록되어 있습니다 (신규 프로젝트에 복사할 때는 사본 동봉 또는 링크 제거).

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
