# Synthesis — 모범사례 및 설계 원칙

본 문서는 4개의 도메인 리서치(`01`~`04`)에서 추출한 횡단 원칙을 통합한 것입니다. 사내 협업 시스템(`/CLAUDE.md`)의 직접적 설계 근거가 됩니다.

## 한 줄 요약

> **단일 에이전트 + 결정적 워크플로 + 외부 ground truth + 사람 게이트 + Day-1 평가**가 멀티에이전트 협업의 기본기다. 멀티 에이전트는 가치가 입증된 좁은 영역에 한해 도입한다.

## I. 18가지 핵심 원칙

각 원칙은 (1) 진술, (2) 근거 사례, (3) 우리 회사 적용 형태로 구성됩니다.

### A. 아키텍처 (Architecture)

**1. 워크플로 우선, 에이전트는 마지막 (Workflow-first)**
- 근거: Anthropic *"Building effective agents"* — *"Don't build agents when workflows suffice"*. OpenAI 가이드도 *single-agent first* 권고
- 적용: 라우팅·체이닝·병렬화로 풀 수 있는 일에 자율 에이전트를 쓰지 않는다. 자율 에이전트는 "도구 다수(>15) + 경로 예측 불가 + 답의 가치 > 토큰 비용 15배" 조건 충족 시에만

**2. 오케스트레이터-워커를 멀티 에이전트의 기본 골격으로**
- 근거: Anthropic Multi-Agent Research System(2025.06), Magentic-One(MS 2024.11). 두 시스템 모두 Lead가 분해·위임·합성하고 Worker가 병렬 실행
- 적용: Lead 에이전트가 Task Ledger(사실·계획) + Progress Ledger(진행·교착 감지) 두 원장을 유지. Worker는 명시 작업 명세로만 실행

**3. 단일 책임 — 한 에이전트 = 한 역할**
- 근거: MetaGPT(PM·Architect·Engineer·QA), ChatDev, CrewAI, Marketing의 Researcher/Writer/Critic 패턴
- 적용: PM·Designer·FE·BE·QA·DevOps·Data·TL·CS·Marketer 단위로 에이전트 분리. 한 에이전트가 두 역할 수행 금지(요청·핸드오프)

**4. 상태는 외부에, 컨텍스트는 좁게**
- 근거: LangGraph Checkpointer, Mem0, Letta tiered memory, Anthropic MARS의 외부 메모리 사용. *"Memory is a tool, not infinite context"*
- 적용: 단기(session) + 중기(project) + 장기(profile) 3계층 메모리. 각 메모리는 출처(provenance) 동봉. row-level security

### B. 협업 인터페이스 (Interface)

**5. Embedded > Standalone Chat**
- 근거: Notion AI, Coda, Linear, Atlassian Rovo의 채택률은 별도 챗 패널이 아닌 inline 블록·사이드 패널에서 우위
- 적용: 사람이 이미 일하는 도구(Slack, Notion, Figma, GitHub, Jira) 안에서 슬래시·우클릭·@멘션으로 호출. 별도 포털 X

**6. 구조화 메시지 + 표준 프로토콜**
- 근거: 자유 텍스트 핸드오프는 누수·해석 오차. MCP(2024.11) 사실상 도구 표준, A2A·AGNTCY 에이전트 통신 경쟁
- 적용: 에이전트 간 핸드오프는 JSON schema 강제(`from`, `to`, `goal`, `inputs`, `deliverables`, `dod`, `deadline`). 사내 도구는 MCP 서버로 노출

**7. 명시적 작업 명세 (Explicit task spec)**
- 근거: Anthropic MARS의 핵심 학습 — *"서브에이전트에 충분히 구체적 프롬프트를 줄 것"*. 모호한 위임이 중복·누락 주원인
- 적용: 모든 핸드오프는 Goal·Inputs·Owner·Deliverables·DoD·Deadline 6슬롯 정규화

### C. 산출물 (Deliverables)

**8. Citation + Draft-by-default**
- 근거: Hebbia, Dovetail, McKinsey Lilli — 모든 답변에 출처 deep-link. 모든 산출물은 "draft" 상태로 시작
- 적용: 메타데이터에 `generated_by`, `source_refs[]`, `status=draft|reviewed|published` 강제

**9. 3단계 승격(Promotion) 게이트**
- 근거: 디자인 시스템 라이브러리 운영, BCG Deckster, Atlassian Rovo, Decagon AOP의 공통 패턴
- 적용: AI 초안 → 동료 리뷰 → 오너 승격. 디자인 컴포넌트·PRD·SQL·재무 모델·정책 문서 모두 동일

**10. 변형 N + 추천 1 (Variants then pick)**
- 근거: Marketing의 광고 크리에이티브, Salesforce Atlas Reasoning Engine의 plan→retrieve→act→evaluate. 단일 결과보다 변형이 회귀·롤백·A/B에 유리
- 적용: 콘텐츠·디자인·코드 변경 모두 N개 변형 + 비교 + 추천 1. 사람이 최종 선택

### D. 검증 (Validation)

**11. 결정적 검사기를 LLM 검증보다 먼저**
- 근거: 컴파일러·테스트·정적분석·정규식·스키마가 LLM judge보다 신뢰. Aider·Claude Code의 build/test 루프, 보안 스캐너
- 적용: 환각을 LLM으로 잡으려 하지 않는다. 먼저 결정적 검사 → 통과 후 LLM judge → 사람 승인

**12. 외부 Ground Truth 검증 루프**
- 근거: Reflexion·Self-Refine만으로는 부족. Aider/Claude Code/Cursor가 빌드·테스트 결과로 자가 수정
- 적용: 모든 코드 변경은 CI로, 콘텐츠는 정책 룰로, 데이터는 시멘틱 레이어로 검증

**13. Eval-first — 평가 시스템을 먼저 만든다**
- 근거: Anthropic *"Start with evals, not the agent"*. Sierra의 시뮬레이션 기반 회귀, Decagon의 정책 회귀, Bain·McKinsey의 골든 셋
- 적용: 도메인별 골든셋(PRD 50, 디자인 50, 코드 100, 마케팅 100, QA 200, CS 500). LLM-as-judge + 주간 휴먼 스팟체크. CI에 통합

### E. 사람 게이트 (HITL)

**14. 위험 등급별 자율성 차등 (Risk-tiered autonomy)**
- 근거: OpenAI Practical Guide *"irreversible actions require human approval"*, Stripe Agent Toolkit, Claude Code permission, EU AI Act
- 적용: L0(read) / L1(local write) / L2(shared write, reviewer 1인) / L3(운영 영향, 사람 승인) / L4(비가역, 사람 + 2차 검토)

**15. 휴먼 핸드오프를 First-class 기능으로**
- 근거: **Klarna 후퇴(2025.05) 핵심 교훈**. Intercom Fin·Sierra의 핸드오프 요약 자동화
- 적용: 사용자/내부 직원이 언제든 사람 호출 가능. 핸드오프 시 자동 요약 + 컨텍스트 전달. *"AI 처리 후 만족도"* 별도 측정 → **Quality-adjusted containment** KPI

**16. "Suggest, don't merge" 기본값**
- 근거: Snyk Autofix, GitHub Code Scanning Autofix, Mabl self-heal 모두 이 방향으로 수렴(2024~2025)
- 적용: 자동 머지·자동 발행은 옵트인. 기본은 PR/Draft 형태로 사람의 명시 승인

### F. 운영 (Operations)

**17. Observability Day-1 + Eval 회귀**
- 근거: 멀티 에이전트는 비결정적·다양 경로. LangSmith·Langfuse·Arize·Helicone 등 표준화. Klarna 후퇴는 lagging CSAT를 못 본 결과
- 적용: 모든 호출 trace 저장(Langfuse self-host 권장 — 사내 데이터 외부 유출 X). trace = 입력·도구 호출·출력·세션 ID·정책 위반 플래그·비용·지연. 주간 회귀 + 분기 감사

**18. Outcome-based KPI 트리**
- 근거: a16z(outcome-based pricing), Klarna 단일 deflection KPI 실패, McKinsey CS 보고서의 4축 표준
- 적용: 최상위(비즈니스 outcome — 매출·비용·NPS) → 중간(per-task quality — resolution rate, accuracy, CSAT, defect rate) → 하위(system health — latency, tool error, hallucination rate). 세 층 동시 대시보드. 한 층만으로 의사결정 금지

## II. 안티패턴 모음 (피해야 할 것)

| # | 안티패턴 | 실증 사례 |
|---|---|---|
| A1 | AI deflection 단독 KPI | Klarna 후퇴(2025.05) |
| A2 | 정책 ground-truth 회귀 테스트 부재 | Air Canada(2024.02) — 회사가 책임진다는 판결 |
| A3 | self-heal 오남용으로 진짜 버그 흡수 | 결제 버튼 변경 미감지 → 7시간 결제 오류 |
| A4 | 자동 패치가 비즈니스 로직 파괴 | Snyk Autofix, GitHub Autofix 초기 사례 |
| A5 | 시멘틱 레이어 없는 NL2SQL | 환각 매출 보고 (Hex/ThoughtSpot 교훈) |
| A6 | 권한 미스매핑으로 임원 문서 노출 | M365 Copilot 초기 배포(Business Insider 2024) |
| A7 | 데이터 거버넌스 무시한 학습 | Figma First Draft 표절 논란(2024.07) |
| A8 | 콘텐츠 양산 → SEO 페널티 | Bankrate, CNET, Sports Illustrated(2023~2024) |
| A9 | 광고 자동 생성의 컴플라이언스 우회 | Performance Max 상표 침해(AdAge 2024) |
| A10 | AI SDR 도메인 평판 손상 | 11x — Gmail 차단 사례 |
| A11 | 음성·소음·코드스위칭 견고성 부족 | McDonald's IBM 드라이브스루 종료(2024.06) |
| A12 | 공공·규제 도메인의 자유 생성 | NYC MyCity 위법 조언(2024.03) |
| A13 | prompt injection / jailbreak | DPD 챗봇 욕설(2024.01) |
| A14 | 메모리 누수 (사용자 A → B 노출) | 일반 보고 다수 |
| A15 | 무한 위임·발화자 선택 오류 | AutoGen GroupChat 핑퐁 |
| A16 | 데모 vs 실측 격차 | Devin SWE-bench 외부 재현 시 성공률 낮음 |
| A17 | 비가역 도구 권한 노출 | Replit Agent DB 임의 삭제(2025) |
| A18 | "AI = 사람 대체" 메시지 백래시 | Klarna, Duolingo 백래시 |

## III. "Lethal Trifecta" — 보안 핵심

Simon Willison(2025.06): 다음 셋이 한 에이전트에 동시 존재하면 위험.
1. 신뢰할 수 없는 입력(외부 콘텐츠·고객 메시지)
2. 민감 데이터 접근
3. 외부 통신/실행 권한

**원칙**: 셋 중 하나는 반드시 차단. 외부 콘텐츠를 읽는 에이전트와 민감 데이터/외부 통신 권한 에이전트를 분리. JIT scoped token, per-tool allowlist, PII redaction, URL allowlist.

## IV. 도입 순서 (Adoption Roadmap)

리서치 사례에서 반복되는 성공 시퀀스:

1. **Eval harness + 골든셋** 구축 (1~2주) — 도메인별 50~200건
2. **단일 워크플로 1개** 자동화 — 가장 마찰 큰 일 1개부터(요약·검색·triage 우선)
3. **Observability** 도입 — Langfuse self-host
4. **사내 도구 MCP 서버화** — 가장 자주 쓰이는 3~5개부터
5. **단일 에이전트 → 워커 분리**는 평가 점수 정체 시에만
6. **거버넌스 위원회** 분기 audit + 회고 → R&R·정책 갱신
7. **변화관리** — Cyborg 사용 패턴 워크숍, 새 직무(AI Trainer 등) 신설

## V. 측정 지표 표준 (KPI Tree)

```
Top: 비즈니스 outcome
  ├─ 매출/비용/NPS 변화량 (도메인별)
  └─ 사용자 채택률 (DAU/WAU per agent)

Mid: per-task quality
  ├─ Resolution / Acceptance rate (산출물 채택률)
  ├─ Quality-adjusted containment (CS)
  ├─ CSAT / 휴먼 평가 점수
  ├─ Defect leak rate (QA)
  ├─ DoD pass rate (산출물 품질)
  └─ Re-work rate (핸드오프 거부율)

Low: system health
  ├─ Latency p50 / p95
  ├─ Cost per task (token + tool)
  ├─ Tool error rate
  ├─ Hallucination rate (eval 기반)
  └─ Policy violation rate
```

세 층을 동시에 보지 못하면 Klarna식 후퇴를 반복한다.
