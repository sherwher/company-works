# Research — Multi-Agent Frameworks & Dev Collaboration

> 본 문서는 사내 AI 에이전트 협업 시스템 설계를 위한 사전 리서치이며, 2024~2026년 사이 공개된 프레임워크·논문·기업 사례를 정리합니다. 인용은 한국어(영문 원어) 형식.
>
> ⚠️ 본 노트의 일부 수치(예: 토큰 15배, SWE-bench 점수, 생산성 55% 등)는 벤더·연구자 자가보고가 다수입니다. 외부 인용 시 1차 출처 재확인 권장.

## 1. 프레임워크별 정리

### 1.1 AutoGen (Microsoft Research)

**개요** — Microsoft Research가 2023년 말 공개, 2024년 v0.4에서 비동기·이벤트 기반(액터 모델)으로 재작성한 멀티 에이전트 대화 프레임워크. "대화 가능한 에이전트(conversable agents)"가 핵심 추상으로, LLM·툴·휴먼을 동일한 메시지 인터페이스로 통합 (논문: "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation").

**핵심 협업 패턴**
- 역할 분리: `AssistantAgent`, `UserProxyAgent`, `GroupChatManager`
- 핸드오프: GroupChat에서 매니저가 다음 발화자 선택(라운드 로빈, LLM 라우팅, 커스텀 함수)
- 메모리: 메시지 히스토리 기반, 외부 메모리(Mem0, Redis)와 어댑터로 연동
- 종료 조건: `max_turns`, `is_termination_msg` 콜백, "TERMINATE" 키워드

**강점·약점**
- 강점: 코드 실행 에이전트(Code Executor) 내장, HITL 1급 지원, v0.4부터 액터 모델 기반 분산
- 약점: GroupChat 길어지면 토큰 폭증, 매니저의 발화자 선택이 불안정, v0.2 → v0.4 마이그레이션 비용

**대표 사용 사례**: Microsoft 내부 데이터 분석 자동화, 학술 코드 실행 벤치마크.

**출처**
- https://microsoft.github.io/autogen/
- https://arxiv.org/abs/2308.08155

### 1.2 CrewAI

**개요** — João Moura가 2024년 초 오픈소스 공개. 역할 기반(role-based) 멀티 에이전트 프레임워크. `Crew`(팀)·`Agent`(역할)·`Task`(작업)·`Process`(실행 절차) 4개 추상.

**핵심 패턴**
- Agent에 `role`, `goal`, `backstory` 자연어 정의
- Sequential / Hierarchical Process. Hierarchical에서는 Manager Agent가 위임
- short-term, long-term, entity memory 분리 (내부 ChromaDB)
- Task 단위 종료. Crew는 모든 Task 완료 시 종료

**강점·약점**
- 강점: 학습 곡선 낮음, YAML 정의 가능, Enterprise 버전에서 관측·배포 통합
- 약점: 그래프 분기·루프 표현력 제한, 동적 재계획·실패 복구 패턴 빈약

**대표 사용 사례**: 마케팅 콘텐츠 생성, 리서치 보고서 자동화, 영업 리드 자격 평가.

**출처**: https://docs.crewai.com/ , https://github.com/crewAIInc/crewAI

### 1.3 LangGraph (LangChain)

**개요** — LangChain 팀이 2024년 공개한 상태 그래프(state graph) 기반 오케스트레이션 라이브러리. 노드(함수/에이전트)와 엣지(조건부 전이)로 워크플로 정의, Pregel 스타일 메시지 패싱.

**핵심 패턴**
- 각 노드가 임의의 함수/에이전트, "Supervisor" 패턴 템플릿 제공
- 조건부 엣지 + `Command` 객체로 다음 노드와 상태 갱신을 동시 지정
- `Checkpointer`(SQLite/Postgres/Redis)로 상태 영속화 → 일시 정지·재개·시간 여행
- `END` 노드 도달 시 종료

**강점·약점**
- 강점: 복잡한 분기·루프·HITL 정밀 표현, durable execution(장시간 실행), LangSmith 관측 통합
- 약점: 보일러플레이트 많음, 그래프 커질수록 디버깅 난이도 상승, LangChain 의존

**대표 사용 사례**: Klarna 고객지원 에이전트, Replit Agent 백엔드, Elastic 보안 분석 에이전트.

**출처**: https://langchain-ai.github.io/langgraph/

### 1.4 MetaGPT

**개요** — arXiv:2308.00352. 표준 운영 절차(SOP)를 LLM 에이전트에 인코딩해 SW 개발을 자동화. PRD → 설계 → 태스크 → 코드 → QA로 이어지는 폭포수형 파이프라인을 가상의 PM·아키텍트·엔지니어·QA 에이전트가 수행.

**핵심 패턴**
- 역할: Product Manager, Architect, Project Manager, Engineer, QA Engineer
- "Publish-Subscribe" 메시지 풀, 각 역할이 관심 메시지만 구독
- 공유 환경(Environment) 객체에 산출물 누적, 문서 단위 컨텍스트 분리
- SOP의 마지막 단계(QA 통과) 도달 시 종료

**강점·약점**
- 강점: 산출물 표준화(PRD, 시퀀스 다이어그램), HumanEval·MBPP에서 단일 에이전트 대비 우수
- 약점: 폭포수 가정으로 변경 요구에 취약, 실제 대형 프로젝트 확장 시 컨텍스트 한계, 코드 실행 검증 약함

**출처**: https://arxiv.org/abs/2308.00352

### 1.5 ChatDev

**개요** — arXiv:2307.07924, "Communicative Agents for Software Development". 칭화대·OpenBMB가 발표한 가상 SW 회사 시뮬레이션. CEO·CTO·프로그래머·리뷰어·테스터 역할이 "Chat Chain"이라는 단계적 대화 체인으로 협업.

**핵심 패턴**
- CXO 계층 + 실무 계층, 각 단계에 instructor/assistant 짝
- Chat Chain — 각 단계의 산출물이 다음 단계 입력
- 단계별 대화 로그, "communicative dehallucination"으로 환각 감소

**강점·약점**
- 강점: 1달러 미만 비용으로 소규모 SW 생성 가능(논문 보고), 단계별 산출물 추적 용이
- 약점: 토이 사례 중심, 외부 API·복잡 의존성 처리 약함

**출처**: https://arxiv.org/abs/2307.07924

### 1.6 Anthropic Multi-Agent Research System (2025)

**개요** — Anthropic이 2025년 6월 공개한 엔지니어링 포스트("How we built our multi-agent research system"). Claude.ai의 Research 기능 백엔드. Lead 에이전트가 서브에이전트들을 병렬로 분기시켜 웹·내부 자료를 조사하고 결과를 합성 (orchestrator-worker 패턴).

**핵심 패턴**
- Lead Researcher(계획·합성) + Subagents(병렬 탐색) + Citation Agent(인용 검증)
- 명시적 작업 명세(프롬프트 템플릿)로 위임, 결과는 텍스트로 반환
- Lead가 외부 메모리(파일)에 계획을 저장해 컨텍스트 한계 완화
- Lead의 종료 판단(충분한 근거 수집 여부)

**핵심 발견**
- 멀티에이전트가 단일 에이전트 대비 내부 평가에서 약 90.2% 향상(특정 리서치 태스크)
- 토큰 사용량은 단일 챗 대비 약 15배 → 가치가 높은 태스크에만 적합
- 평가는 LLM-as-judge + 휴먼 평가 조합, 프롬프트 엔지니어링이 결정적

**출처**: https://www.anthropic.com/engineering/built-multi-agent-research-system

### 1.7 OpenAI Swarm / Agents SDK

**개요** — Swarm은 OpenAI가 2024년 10월 공개한 실험 라이브러리. "에이전트"와 "핸드오프(handoff)" 두 추상에 집중. 2025년 프로덕션용 **OpenAI Agents SDK**로 계승, tracing·guardrails·세션 관리 추가.

**핵심 패턴**
- Agent = 시스템 프롬프트 + 툴 집합
- 핸드오프: 툴 호출의 반환값으로 다음 Agent 객체를 반환 → 라우팅
- 기본 stateless, Agents SDK에서는 Session 추상 제공

**강점·약점**
- 강점: 매우 단순한 정신 모델, Responses API와 결합 시 stateful 운영
- 약점: 복잡한 분기·병렬·루프는 직접 구현, 그래프 기반 대비 표현력 낮음

**출처**: https://github.com/openai/swarm , https://openai.github.io/openai-agents-python/

### 1.8 Google Agent Development Kit (ADK)

**개요** — Google이 2025년 4월 Cloud Next에서 공개. Gemini·Vertex AI와 통합된 멀티 에이전트 프레임워크. Agent2Agent(A2A) 프로토콜로 이종 프레임워크 간 상호운용성 표방.

**핵심 패턴**
- `LlmAgent`, `WorkflowAgent`(Sequential/Parallel/Loop), Custom Agent
- A2A 프로토콜 — Agent Card(능력 메타데이터) + 표준화된 메시지
- Vertex AI Memory Bank, 세션 서비스

**강점·약점**
- 강점: 멀티 프레임워크(LangGraph/CrewAI 호환) 호출, GCP 관측·배포 일체화
- 약점: 신생 프레임워크로 커뮤니티 작음, 비-GCP 환경에서 매력 감소

**출처**: https://google.github.io/adk-docs/

### 1.9 Magentic-One (Microsoft)

**개요** — Microsoft Research가 2024년 11월 공개한 일반 목적 멀티 에이전트 시스템 (arXiv:2411.04468). Orchestrator(Lead)가 WebSurfer·FileSurfer·Coder·ComputerTerminal 등 전문 에이전트를 지휘.

**핵심 패턴**
- Orchestrator가 두 원장(ledger): Task Ledger(사실·계획) + Progress Ledger(진행·교착 감지)
- 진행이 멈추면 Orchestrator가 재계획(replanning)
- AutoGen v0.4 위에서 구현

**강점·약점**
- 강점: GAIA·WebArena·AssistantBench에서 경쟁력 있는 성과
- 약점: 위험한 실세계 행동(파일 삭제 등) 가능성, 비용·지연

**출처**: https://arxiv.org/abs/2411.04468

### 1.10 CAMEL-AI

**개요** — arXiv:2303.17760. 2개 에이전트(user/assistant)의 역할 놀이로 시작한 초기 멀티 에이전트 연구. 현재는 OWL(Optimized Workforce Learning) 등 워크포스 추상으로 확장.

**핵심 패턴**
- Inception Prompting: 역할·과제 자동 생성해 자가 협상
- Society/Workforce: 다수 에이전트의 비동기 작업 큐

**출처**: https://arxiv.org/abs/2303.17760

### 1.11 비교 표

| 프레임워크 | 추상 단위 | 오케스트레이션 | HITL | 영속화 | 주요 강점 |
|---|---|---|---|---|---|
| AutoGen v0.4 | Conversable Agent | GroupChat / 액터 | 1급 | 외부 어댑터 | 코드 실행, 분산 |
| CrewAI | Agent/Task/Crew | Sequential/Hierarchical | 보통 | 내장 메모리 | 학습 곡선 낮음 |
| LangGraph | Node/Edge/State | 상태 그래프 | 강함(interrupt) | Checkpointer 1급 | 복잡 워크플로 |
| MetaGPT | Role | SOP 파이프라인 | 약함 | 환경 객체 | SW SOP |
| ChatDev | Role pair | Chat Chain | 약함 | 단계 로그 | 가상 회사 |
| Anthropic MARS | Lead/Subagent | Orchestrator-Worker | 보통 | 외부 메모리 | 리서치 |
| Swarm/Agents SDK | Agent | Handoff | 보통 | Session | 단순성 |
| Google ADK | Agent/Workflow | A2A 프로토콜 | 보통 | Memory Bank | GCP 통합 |
| Magentic-One | Orchestrator/전문가 | Ledger 기반 재계획 | 보통 | AutoGen 기반 | 일반 태스크 |
| CAMEL | Role-play | Society/Workforce | 약함 | - | 데이터 생성 |

## 2. 기업 도입 사례

### 2.1 GitHub Copilot Workspace & Coding Agent
- 워크플로: 이슈 → Spec → Plan → Implementation → PR. 2025년 GA된 Copilot coding agent는 이슈 할당만으로 백그라운드 VM에서 분기·구현·PR 생성
- 효과: GitHub 자체 보고로 PR 생성 시간 단축; Accenture 사내 실험에서 Copilot 일반에 한정한 코드 작성 생산성 약 55% 향상(2024년)
- 교훈: 머지 전 휴먼 리뷰 필수, 보안 격리(Actions firewall)와 권한 최소화 결정적
- 출처: https://github.blog/news-insights/product-news/github-copilot-the-agent-awakens/

### 2.2 Cognition Labs Devin
- 워크플로: Slack/웹에서 자연어 요청 → Devin이 자체 셸·브라우저·에디터에서 장시간 작업 → PR
- 효과: SWE-bench Verified 13.86%(초기) → 후속 향상. 외부 재현 평가에서 실제 종단 성공률은 낮음
- 교훈: 데모 vs 실측 격차, 장시간 자율 실행은 비용·환각 누적 위험
- 출처: https://cognition.ai/blog/introducing-devin

### 2.3 Cursor
- 에디터 내 Composer/Agent 모드, Background Agents가 원격 환경에서 멀티 파일 변경
- 2025년 ARR 1억 달러 돌파(공개 보도)
- 교훈: 빠른 피드백 루프(인-IDE)가 자율성보다 가치 있는 경우 많음

### 2.4 Claude Code
- 셸에서 자연어 → 파일 읽기·편집·테스트·커밋. Subagents·MCP·Hooks 지원
- 교훈: 터미널 일급 에이전트 + 명시적 권한 모델이 신뢰 형성에 중요
- 출처: https://docs.claude.com/en/docs/claude-code/overview

### 2.5 Aider / Replit Agent
- Aider: 오픈소스 페어 프로그래머, git diff 기반 편집·자동 커밋
- Replit Agent: 자연어로 풀스택 앱 생성·배포, LangGraph 백엔드 사용 사례 공개

### 2.6 Shopify
- 2025년 Tobi Lütke CEO 메모: "AI 사용은 기본 기대치"라며 신규 채용 전 AI로 해결 가능 여부 검토 의무화
- 내부 Sidekick(머천트용) + 엔지니어 전용 코딩 에이전트 광범위 도입

### 2.7 Stripe
- Stripe Agent Toolkit(2024) 공개로 LangChain·Vercel AI SDK·OpenAI에서 결제 API를 안전하게 호출
- 내부적으로 부정거래 탐지·문서 자동화에 LLM 광범위 사용
- 출처: https://stripe.com/newsroom/news/agent-toolkit

### 2.8 Block (Square)
- 오픈소스 에이전트 프레임워크 **Goose**(2025) 공개. MCP 일급 지원. 사내 엔지니어가 데이터 분석·코드 작성에 사용
- 출처: https://block.github.io/goose/

### 2.9 Atlassian Rovo
- 2024년 발표, Jira/Confluence 데이터를 Knowledge Graph로 통합. Rovo Agents가 SDLC 자동화(이슈 분류, PR 요약, 릴리스 노트). Agent Studio로 사내 커스텀 에이전트 빌드
- 출처: https://www.atlassian.com/software/rovo

### 2.10 멀티에이전트 PR 리뷰·테스트·배포
- CodeRabbit, Greptile, Korbit: PR 리뷰 전용 에이전트
- Sweep, Codegen: 이슈 → PR 자동화
- Harness AI / GitLab Duo: 파이프라인 실패 분석·자동 수정 제안
- Anthropic 내부 사례: Claude Code subagents로 테스트 작성·리뷰·문서화 분리

## 3. 공통 패턴 (모범사례)

### 3.1 오케스트레이션 패턴 분류

| 패턴 | 설명 | 대표 구현 |
|---|---|---|
| Orchestrator-Worker | Lead가 작업 분해·위임, Worker는 병렬 실행 | Anthropic MARS, Magentic-One |
| Hierarchy(계층) | Manager → Sub-manager → Worker 다단계 | CrewAI Hierarchical, ChatDev |
| Sequential / Pipeline | 단계별 산출물 전달 | MetaGPT, ChatDev Chat Chain |
| Router / Handoff | 라우터가 적합 에이전트로 전달 | Swarm, Agents SDK |
| Debate / Reflection | 다수 에이전트의 비판·합의 | Society of Mind 변형 |
| Graph / State Machine | 명시적 상태·전이 | LangGraph |
| Blackboard / Pub-Sub | 공유 메모리에 산출물 게시 | MetaGPT 환경 객체 |

### 3.2 컨텍스트 공유·핸드오프
- **명시적 작업 명세**: Anthropic 보고서는 "서브에이전트에 충분히 구체적 프롬프트를 줄 것"을 핵심 학습으로 제시. 모호한 위임은 중복·누락의 주원인
- **외부 메모리**: 컨텍스트 윈도 한계를 우회하려 파일·벡터 DB·구조화 메모리 사용. LangGraph Checkpointer, Vertex Memory Bank, Mem0
- **구조화 산출물**: 자유 텍스트보다 JSON/Markdown 스키마 강제 → 다음 단계 파싱 안정화

### 3.3 검증 루프
- **자기 비판(Self-critique)**: Reflexion, Self-Refine
- **이중 에이전트 검증**: ChatDev의 reviewer/tester, Anthropic의 Citation Agent
- **외부 검증**: 컴파일러·테스트·정적 분석을 ground truth로 사용. Aider·Claude Code의 빌드/테스트 루프

### 3.4 Human-in-the-loop 적용 지점

| 지점 | 목적 | 구현 |
|---|---|---|
| 계획 승인 | 위험·비용 큰 작업 전 점검 | LangGraph `interrupt` |
| 툴 실행 승인 | DB 쓰기·결제·배포 등 | Claude Code permission |
| 산출물 리뷰 | PR 머지·문서 게시 | GitHub Copilot agent PR |
| 실패 재진입 | 교착·실패 시 사람 개입 | Magentic-One progress ledger |

## 4. 안티패턴 / 실패 사례

1. **토큰·비용 폭증** — 멀티에이전트는 단일 챗 대비 약 15배 토큰. 가치 낮은 태스크 적용 시 ROI 음수. AutoGen GroupChat에서 무한 루프 다수 보고
2. **에이전트 간 일관성 붕괴** — 같은 사실에 다른 답 → 합성 단계에서 모순. 대응: Lead가 single source of truth 유지
3. **무한 위임·발화자 선택 오류** — AutoGen GroupChat 핑퐁 사례 → LangGraph로 명시 종료 조건 마이그레이션
4. **실세계 행동의 위험** — Magentic-One 논문이 명시 경고. Replit Agent 사고(2025년): 사용자 데이터베이스 임의 삭제 보고 → 이후 권한 격리·드라이런 강화
5. **데모 vs 실측 격차** — Devin SWE-bench Verified 외부 재현 시 성공률 낮음. 사내 도입 시 자체 골든셋·실측 평가 필수
6. **공유 메모리 오염** — 한 에이전트의 환각이 공유 메모리에 들어가 후속으로 전파. 대응: 출처 메타데이터(provenance), 인용 검증 에이전트
7. **평가 부재** — 멀티에이전트는 비결정적·다양 경로. 단순 단위 테스트 불가. 모범: LLM-as-judge + 휴먼 라벨링 + 트레이스 회귀

## 5. 핵심 인사이트 (회사 도입 관점)

1. **단일 에이전트 우선, 멀티는 가치 입증 후** — Anthropic도 "리서치 같은 병렬·탐색형 태스크에만 멀티가 합리적"이라 못박음
2. **오케스트레이터-워커를 기본 골격으로** — Magentic-One의 두 원장(Task Ledger / Progress Ledger) 차용해 계획·진행 분리
3. **상태는 외부에, 컨텍스트는 좁게** — LangGraph Checkpointer 또는 동급 영속 계층. 공유 메모리에는 출처 항상 동봉
4. **휴먼 게이트를 비가역 행동에 의무화** — 배포·DB 쓰기·외부 결제·고객 통신은 기본값 차단. Stripe Agent Toolkit, Claude Code permission 모델
5. **검증 루프는 외부 ground truth로** — 컴파일러·테스트·린터·정적분석·운영 메트릭 등 결정론적 신호를 루프에 포함
6. **자체 평가셋과 트레이스 회귀 테스트 운영** — 사내 골든셋(50~200) + LLM-as-judge + 주간 휴먼 스팟체크. LangSmith/Braintrust/Phoenix 중 택1
7. **종료 조건과 예산을 일급으로** — max_turns·max_tokens·max_wallclock·max_cost 강제. 진행 정체(progress ledger) 감지 시 자동 에스컬레이션
8. **상호운용성·표준 우선** — MCP(툴)·A2A(에이전트)·OpenTelemetry(관측) 채택해 프레임워크 락인 회피

## 부록 A. 권장 출발 스택

| 레이어 | 1차 권장 | 대안 |
|---|---|---|
| 오케스트레이션 | LangGraph | AutoGen v0.4, Agents SDK |
| 코딩 에이전트 | Claude Code (서브에이전트) | Cursor Background Agents, Aider |
| 툴 통합 | MCP 서버(사내) | OpenAPI + 함수 호출 |
| 관측·평가 | LangSmith 또는 Phoenix | Braintrust, Langfuse |
| 메모리 | Postgres + pgvector + Checkpointer | Mem0, Vertex Memory Bank |
| HITL | Slack 승인 봇 + LangGraph interrupt | GitHub PR 리뷰 |
