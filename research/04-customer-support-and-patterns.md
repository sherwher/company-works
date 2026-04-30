# Research — Customer Support & Cross-cutting Patterns

> 고객지원 도입 사례 + 횡단 협업 패턴(HITL·메모리·거버넌스·평가·프로토콜). 외부 인용 시 1차 출처 재확인 권장.

## 1. 고객지원(CS) 영역

### 1.1 Klarna × OpenAI — "성공 후 후퇴" 사례
**도입(2024.02)**
- AI Assistant 출시 한 달 만에 230만 건 대화 처리, "700명 풀타임 상담원(FTE) 일에 상응" 발표 (Klarna press release, 2024.02.27)
- AHT 11분 → 2분, 반복 문의 25%↓, "CSAT는 사람과 동등(on par with human agents)" 주장
- 23개 시장, 35개 언어, 3.3억 달러 추정 영업이익 기여(2024 연환산)

**후퇴(2025.05)**
- CEO Sebastian Siemiatkowski가 Bloomberg/CNBC에 "AI에 너무 의존했고 품질이 떨어졌다(we went too far... lower quality)"고 인정 → 인간 상담원 재채용 하이브리드 모델 (Bloomberg, 2025.05.08)
- 채용 형태는 풀타임이 아닌 "Uber-like" 원격·온디맨드. 학생·시골 거주자 우선

**교훈**:
1. CSAT lagging — AI 도입 직후 유지되었으나 6~12개월 후 복잡 케이스 누적 불만으로 하락
2. Edge case 학습 정체 — 학습 데이터가 사람 상담 로그였는데 사람을 줄이자 새 학습 신호 고갈
3. 브랜드 가치 훼손 — "Klarna는 사람과 통화 못한다"는 평판이 SNS 확산
4. → AI deflection을 단독 KPI로 삼지 말 것. **"Quality-adjusted resolution rate"** 필요

### 1.2 Intercom Fin AI Agent
- GPT-4 기반 출시(2023) → Fin 2(Claude 기반, 2024.10) → Fin 3(2025)
- 평균 자동 해결률(resolution rate): "average 56% of customer questions instantly and correctly" (Intercom blog, "Fin 2 launch")
- **과금 모델**: $0.99 per resolution — outcome-based pricing이 업계 표준화 흐름의 신호
- 휴먼 핸드오프: confidence score + intent classification + 명시적 "talk to a human" 요청 트리거. 핸드오프 시 대화 요약 자동 생성

### 1.3 Decagon
- 2023 창업, a16z·Accel. 2025 Series C valuation ~$1.5B
- 차별점: **"Agent Operating Procedures (AOPs)"** — 결정 트리 + LLM 추론 결합 워크플로 정의 언어. 자연어로 정책 작성하면 에이전트가 따름
- 고객: Eventbrite, Notion, Bilt Rewards, Duolingo. Notion 도입 후 "Tier-1 티켓 자동화율 70%+"
- 거버넌스 강조: 모든 응답 traceability, 정책 위반 시 회귀 테스트 자동 실행

### 1.4 Ada
- 캐나다 토론토, 2016 창업의 원조 CS 자동화. 2023년 LLM 기반 "Ada Reasoning Engine" 재출시
- 자체 발표 평균 자동 해결률(AR) 70%+ (Ada AR Benchmark Report 2024)
- 특징: **"Coaching"** — 인간 QA 담당자가 응답 평가하면 자동으로 가이드라인 업데이트되는 RLHF 유사 루프

### 1.5 Sierra (Bret Taylor / Clay Bavor)
- 전 Salesforce Co-CEO이자 OpenAI 이사회 의장 Bret Taylor 공동창업(2023). 2024 valuation $4.5B
- 고객: Sonos, WeightWatchers, ADT, SiriusXM
- 차별점: **"Agent Development Lifecycle"** — 에이전트를 SW처럼 다루며 시뮬레이션 기반 회귀 테스트 강조
- WeightWatchers 도입 후 CSAT 4.6/5, 휴먼 상담원이 "더 어려운 일에 집중" → 직무만족도 상승

### 1.6 Zendesk AI Agents (구 Ultimate.ai)
- Zendesk가 2024년 핀란드 Ultimate.ai 인수 → "Zendesk AI Agents" 통합
- "Agents Copilot" + "AI Agents (autonomous)" 듀얼 라인. 과금: per resolution
- 공개 자료: "고객사 중앙값 80%+ ticket deflection" 주장. 산업 평균은 40~60% (Zendesk CX Trends Report 2025)

### 1.7 Salesforce Agentforce / Service Cloud Einstein
- 2024.09 Dreamforce에서 Agentforce 발표. Atlas Reasoning Engine + Data Cloud + MuleSoft
- 가격: $2 per conversation
- Wiley(textbook): case resolution time 50%↓; OpenTable, Saks
- 2025년 "Agentforce 2dx" 업데이트로 비동기·proactive 에이전트 트리거

### 1.8 Shopify Sidekick
- 머천트(셀러)용 AI 어시스턴트. CS보다 내부 운영 코파일럿에 가깝지만 Shopify Inbox와 결합되어 머천트 대신 고객 메시지 응답·드래프트
- 2025.04 Tobi Lütke 메모: "AI 사용은 baseline expectation"

### 1.9 한국 시장 사례
- **채널톡**: 2024 "ALF (AI Live Friend)" 출시. 한국어·일본어 특화. 일본 고객사 평균 응답시간 70%↓. 핸드오프 시 한국어 존댓말 톤 유지가 차별점
- **솔트룩스**: "Luxia" 기반 자체 LLM과 RAG. 공공기관·금융권 보수적 고객 대응. 2025 KB국민은행·NH농협 도입 보도
- **네이버 클로바**: HyperCLOVA X 기반 컨택센터 솔루션. 라인플러스 일본 고객지원
- **카카오엔터프라이즈**: KakaoWork 내 AI 에이전트. 2025 "Kanana" 모델 통합

### 1.10 측정 지표 — 업계 표준화 추세

| 지표 | 정의 | 산업 벤치마크(2025) |
|---|---|---|
| Deflection / Resolution Rate | AI가 종결한 티켓 비율 | 40~70% |
| CSAT (post-AI) | AI 응대 후 만족도 | 사람 대비 ±5%p |
| AHT | 평균 처리 시간 | 30~80%↓ |
| Escalation Quality | 에스컬레이션 시 첫 응답 적합도 | 측정 표준 미흡 |
| Containment | 사람 개입 없이 종결 비율 | Deflection과 유사 |

출처: Gartner "Magic Quadrant for CCaaS 2025", Zendesk CX Trends 2025, McKinsey "The state of AI in customer service" (2024.12)

## 2. Cross-cutting 협업 패턴

### 2.1 Human-in-the-Loop (HITL) 디자인 패턴

Anthropic "Building effective agents"(2024.12)와 OpenAI "A Practical Guide to Building Agents"(2025.04 PDF)가 공통 강조하는 트리거:
1. **Confidence-based**: 모델 자체 confidence/self-critique가 임계치 이하
2. **Risk-tier-based**: 행위의 영향(돈·외부 통신·데이터 변경)에 따라 사전 승인. OpenAI: "irreversible actions require human approval"
3. **Policy-based**: 명시적 룰("환불 100달러 초과 시 사람 승인")
4. **User-requested**: 사용자가 "talk to a human" 요청
5. **Loop-detection**: 같은 도구를 N회 이상 호출 시 escalate

세부 모드:
- **Approval mode** (사람 승인 후 실행) vs **Observe-and-correct** (실행 후 검토)
- **Pair mode** (Cursor, GitHub Copilot — 모든 커밋이 사람 동의)
- **Async review queue** (Sierra, Decagon — 신뢰 점수 낮은 케이스를 큐에 적재해 야간 검토)

### 2.2 에이전트 메모리·컨텍스트 공유
- **Mem0** (mem0.ai): 오픈소스 메모리 레이어. fact extraction → embedding → graph
- **Letta** (구 MemGPT, UC Berkeley): "tiered memory"(working memory + archival memory). OS의 가상 메모리 비유
- **OpenAI Memory** (ChatGPT 2024.02 출시, API 2025.04 Responses API 통합)
- **Anthropic Claude Projects / Memory tool** (2025): file-system 형태 영속 메모리
- 경쟁 OSS: Zep, Cognee

설계 원칙(Anthropic MARS, 2025.06):
- "메모리는 도구 — context window를 무한 확장하지 말고 selective retrieval"
- 메모리에 저장된 내용도 source tracking 필요(어느 대화에서 학습했나)

### 2.3 거버넌스·감사·관측성

| 도구 | 강점 | 라이선스 |
|---|---|---|
| LangSmith (LangChain) | LangChain 생태계 통합, trace 시각화 | 상용 + free tier |
| Langfuse | OSS self-host, OpenTelemetry 호환 | MIT |
| Arize Phoenix / AX | ML 옵저버빌리티 출신, drift 감지 | OSS + 상용 |
| Helicone | proxy 기반 간편 도입 | OSS |
| Honeycomb | 일반 옵저버빌리티 통합 | 상용 |

거버넌스 프레임워크: NIST AI RMF(2023, 2024 GenAI profile), EU AI Act(2024 발효, 2026 풀 적용 — 고위험 AI 시스템 사람 감독 의무화), ISO/IEC 42001(2023)

### 2.4 평가·품질 보증
- **LLM-as-judge**: 두 응답을 비교 평가. 단점은 자기 모델 편향(self-preference bias) — Zheng et al. 2023, "Judging LLM-as-a-Judge with MT-Bench" (NeurIPS 2023)
- **Braintrust**: 평가 데이터셋 + CI 통합
- **Galileo**: hallucination metric, 자체 "Luna" 평가 모델
- **OpenAI Evals**, **Anthropic 자체 evals**, **Inspect AI** (UK AISI, OSS)
- **Sierra의 시뮬레이션 평가**: 가상 사용자(LLM 생성)와의 multi-turn 대화로 회귀 테스트

원칙(Anthropic): *"Start with evals, not the agent. If you can't measure it, you can't improve it."*

### 2.5 권한·보안 (Tool access control)
- **Model Context Protocol (MCP)** — Anthropic 2024.11 발표. LLM ↔ 외부 툴/데이터 표준화. 2025년 OpenAI·Google·Microsoft 채택 발표
- MCP 권한 모델: 서버 단위 capability 선언, 클라이언트가 사용자 동의 후 호출
- **OAuth 2.1 for AI Agents** (IETF draft, 2025): 에이전트를 first-class principal로 인증
- **Confused deputy 문제**: 에이전트가 사용자 권한을 빌려 의도치 않은 작업 수행 → scoped tokens, just-in-time permission
- **Prompt injection 방어 — Simon Willison "lethal trifecta"**: (1) 신뢰할 수 없는 입력, (2) 민감 데이터 접근, (3) 외부 통신. 셋이 동시면 위험. 셋 중 하나는 차단 (simonwillison.net 2025.06)

### 2.6 에이전트 간 통신 프로토콜
- **MCP** — Anthropic 주도. 에이전트 ↔ 도구·데이터
- **A2A (Agent-to-Agent Protocol)** — Google 주도, 2025.04. 50+ 파트너(Atlassian, SAP, Salesforce). 에이전트 간 task delegation 표준
- **AGNTCY** (Cisco, LangChain, Galileo, Glean) — 2025.03 오픈 컨소시엄. "Internet of Agents"
- **ACP** — IBM Research / Linux Foundation, 2025

업계 컨센서스(2026 Q1): **MCP가 도구 연결 표준으로 사실상 정착**, 에이전트 간 통신은 A2A vs AGNTCY 경쟁

### 2.7 조직적 영향
- 새 직무: **AI Trainer**, **Agent Ops engineer**, **Prompt/Policy designer**, **AI QA reviewer**
- 기존 CS 상담원 → "Tier-2 에스컬레이션 전문가" 또는 "AI 트레이너"로 전환
- 거버넌스 위원회: Legal·Security·Product·Data Science·현업 대표 5인. 분기별 audit
- McKinsey "Superagency in the workplace"(2024): "AI 도입 성공 기업의 70%는 명시적 거버넌스 프로세스를 6개월 내 수립"

## 3. 메타 모범사례 (정제된 원칙)

### 3.1 Anthropic — "Building Effective Agents" (2024.12)
1. **"Don't build agents when workflows suffice"** — 단순 체인·라우팅으로 충분한 일에 자율 에이전트 쓰지 말라
2. **워크플로 vs 에이전트 구분**: 워크플로는 사전 정의 코드 경로, 에이전트는 LLM이 동적으로 도구·경로 결정
3. 5가지 워크플로 패턴: Prompt chaining, Routing, Parallelization, Orchestrator-workers, Evaluator-optimizer
4. **Simplicity first**: 프레임워크보다 직접 LLM API 호출이 나을 때가 많다
5. 도구 인터페이스는 **"ACI(Agent-Computer Interface)"**로 신중히 설계 — human UX만큼 중요

### 3.2 Anthropic — "How we built our multi-agent research system" (2025.06)
- **오케스트레이터-워커 패턴**: 메인 Claude가 서브 에이전트들에게 병렬 분배
- **Token economics**: 멀티에이전트 약 15× 토큰. 답의 가치가 토큰 비용보다 큰 도메인(연구·전략)에서만 정당화
- "Agents are stateful and errors compound" → checkpointing, resume from failure 필수
- **End-state evaluation**: 과정이 아닌 결과 기반 평가. LLM judge로 final answer 판정

### 3.3 OpenAI — "A Practical Guide to Building Agents" (2025.04)
- 에이전트 정의: "systems that independently accomplish tasks on your behalf"
- 모델 선택: "단순한 일은 작은 모델, 복잡한 추론은 큰 모델 — 시작은 가장 강력한 모델로 정확도 베이스라인 잡고 다운사이징"
- **Single-agent first**: 멀티에이전트 분리는 도구가 너무 많거나(>15), 도메인이 분명히 분리될 때만
- 가드레일 레이어: input classification, output filtering, tool call validation, PII redaction, brand alignment

### 3.4 a16z — "The AI agent stack" (2024~2025)
- 11 layers: agent hosting, observability, agent frameworks, memory, tool libraries, sandboxes, model serving, storage, voice, model API
- "Outcome-based pricing이 SaaS의 종말을 가속" (2025.01 Olivia Moore)
- "Vertical agents가 horizontal보다 먼저 PMF에 도달"(legal, healthcare, customer support)

### 3.5 Sequoia — "Generative AI's Act II/III" (2024.09 / 2025.09)
- "Reasoning + agents = Act III". 추론 모델(o1, DeepSeek-R1, Claude 3.7 Sonnet thinking)과 결합
- 추천 패턴: **"Cognitive architecture"** — 도메인별 추론·검색·도구 사용을 명시 그래프로

### 3.6 핵심 원칙 비교

| 원칙 | Anthropic | OpenAI | a16z/Sequoia |
|---|---|---|---|
| 단순성 우선 | 강조 (workflows > agents) | 강조 (single-agent first) | 언급 |
| 평가 우선 | 매우 강조 | 강조 (eval before scale) | 강조 |
| HITL | 신뢰·안전 측면 강조 | 가드레일 명시 | 거버넌스 측면 |
| 멀티에이전트 | "토큰 가치 큰 곳만" | "분리 명확할 때만" | 적극 옹호 |
| 메모리 | 도구로 취급 | Responses API memory | "memory layer" 별도 계층 |

## 4. 안티패턴 / 부메랑 사례

1. **Klarna 후퇴(2025.05)** — 위 1.1 참조
2. **Air Canada 챗봇(2024.02)** — 잘못된 환불 정책 안내. BC Civil Resolution Tribunal: "the company is responsible for all information on its website, whether from a chatbot or static page". 환각·허위 정보의 법적 책임은 회사
3. **McDonald's IBM AI 드라이브스루(2024.06 종료)** — 100여 매장 테스트 후 종료. "베이컨 260장 추가" 같은 오작동 영상 SNS. 음성·소음·코드스위칭 환경의 견고성 부족
4. **NYC MyCity 챗봇 환각(2024.03)** — 비즈니스 챗봇이 "팁을 떼도 된다, 거주민 차별해도 된다" 위법 조언 (The Markup). 공공·규제 도메인은 자유 생성보다 retrieval-only + 명시 거부가 안전
5. **DPD 챗봇 욕설(2024.01)** — 영국 택배사 챗봇이 사용자 유도에 따라 회사 욕설·시 작성. prompt injection·jailbreak 가드레일 필수

일반화된 안티패턴:
- AI-only 절대화 (사람 에스컬레이션 경로를 숨기기)
- 벤치마크 over-fitting (deflection만 올리려고 어려운 질문에 "잘 모릅니다"로 강제 deflect)
- 메모리 누수 (사용자 A 컨텍스트가 사용자 B 응답에 노출)
- 툴 폭주 (같은 API를 수십 번 호출)
- Eval 부재 배포

## 5. 우리 회사 협업 시스템 설계 인사이트

1. **워크플로 우선, 에이전트는 마지막** — 사전 정의 가능한 업무는 결정적 워크플로(routing, prompt chaining)로. 자율 에이전트는 도구가 많고 경로 예측 불가일 때만
2. **평가 시스템을 먼저 만든다(Eval-first)** — 골든셋 50~200 + LLM-as-judge + CI 회귀. Braintrust나 Langfuse + 자체 dataset 권장
3. **HITL은 리스크 등급 기반으로 명시 설계** — (a) reversible 자동, (b) reversible 사후검토, (c) irreversible 사전승인. 금전·외부 통신·데이터 삭제는 c등급
4. **휴먼 핸드오프를 First-class 기능으로** — Klarna 핵심 교훈. 사용자가 언제든 사람 호출, 핸드오프 시 자동 요약 + 컨텍스트, "AI 처리 후 만족도" 별도 측정. **Quality-adjusted containment** 사용
5. **MCP·A2A 표준 채택(Interop-first)** — 사내 도구는 처음부터 MCP 서버로 구현. 향후 LLM 벤더 교체 비용 절감
6. **관측성·감사 가능성을 Day 1에** — 모든 호출 trace 저장(Langfuse self-host 권장 — 사내 데이터 외부 유출 X). trace에 입력·도구 호출·출력·세션 ID·정책 위반 플래그 포함
7. **메모리는 명시적·계층적으로** — opt-in. 단기(session) + 중기(project) + 장기(profile) 3계층. 출처 추적. row-level security
8. **프롬프트 인젝션·도구 권한 통합 보안** — Simon Willison "lethal trifecta" 회피. 외부 콘텐츠 읽는 에이전트와 민감 데이터/외부 통신 권한 에이전트를 분리. JIT scoped token, per-tool allowlist, PII redaction, URL allowlist
9. **조직적 변화 관리** — AI Trainer / Agent Ops / Policy Designer 신설. 거버넌스 위원회 5인. 톤은 "AI는 도구이며 결과 책임은 사람"
10. **Outcome-based KPI 트리** — 최상위(매출·비용·NPS) / 중간(per-task quality) / 하위(system health). 세 층 동시에 보는 대시보드. 한 층만으로 의사결정 금지

## 주요 출처

- Anthropic, "Building effective agents" (2024.12) — anthropic.com/research/building-effective-agents
- Anthropic, "How we built our multi-agent research system" (2025.06)
- OpenAI, "A Practical Guide to Building Agents" PDF (2025.04)
- Klarna press release (2024.02.27); Bloomberg (2025.05.08)
- Intercom blog "Fin 2 launch" (2024.10)
- Sierra blog "Building agents you can trust" (2024)
- Salesforce Dreamforce 2024
- Google Cloud Next 2025, A2A Protocol (2025.04)
- Anthropic, MCP specification — modelcontextprotocol.io (2024.11)
- Simon Willison, "The lethal trifecta" (2025.06)
- BC Civil Resolution Tribunal, *Moffatt v. Air Canada* (2024.02)
- Zheng et al., "Judging LLM-as-a-Judge with MT-Bench" NeurIPS 2023
- McKinsey, "The state of AI in customer service" (2024.12)
- Gartner, "Magic Quadrant for CCaaS" (2025)
- a16z, "The AI agent stack" (2024.12)
- Sequoia, "Generative AI's Act III: Reasoning + Agents" (2025.09)
