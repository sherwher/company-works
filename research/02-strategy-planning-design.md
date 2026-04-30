# Research — Strategy, Planning, Design AI Agents

> 사업 전략·기획·디자인 영역에서 AI 에이전트 도입 사례. 인용 시 영문 원어 병기.
>
> ⚠️ 본 노트도 학습된 공개 자료(2024~2026 보도·발표) 기반이며, 외부 인용 시 1차 출처 재확인 권장.

## 0. 배경

2024~2026년 사이 AI 코파일럿은 단순 챗을 넘어 **에이전틱 워크플로** — 다단계 추론·도구 호출·문서 생성·승인 루프 — 로 진화. 핵심 변화:
1. 장기 컨텍스트 + RAG로 사내 지식 활용
2. **MCP**(Anthropic, 2024.11) 등 표준 프로토콜로 도구 연결 비용 감소
3. Claude Code/Cursor/Devin 등 자율 실행 에이전트 등장

## 1. 사업 전략·기획 영역

### 1.1 McKinsey "Lilli"
- **개요**: 2023년 8월 사내 출시한 GenAI 어시스턴트. 100,000+ 사내 문서·인터뷰·연구 자료에 RAG. 2024년 기준 전 직원 약 70%가 사용, 월 50만+ 쿼리
- **워크플로**: 자연어 질의 → Lilli가 사내 사례·전문가·프레임워크 인용 → 컨설턴트 검증 후 산출물에 활용
- **효과**: 리서치 단계 평균 시간 "수 주 → 수 시간". 1인당 주당 약 30% 리서치 시간 절감
- **교훈**: (1) 사내 IP 활용이 차별화 핵심 — 외부 LLM만으론 대체 불가, (2) 답을 주는 게 아니라 *전문가·자료를 매칭*하는 도구로 포지셔닝
- **출처**: McKinsey, "Meet Lilli, our generative AI tool" (2023.08)

### 1.2 BCG "GENE" / "Deckster"
- **개요**: GENE = 범용 컨설팅 어시스턴트, Deckster = 슬라이드 자동 생성·리뷰 에이전트. 2024년 BCG 직원 90%+ GenAI 도구 일상 사용
- **워크플로**: Deckster — 슬라이드 초안 업로드 → 메시지 일관성·시각 계층·BCG 브랜드 가이드 검토 → 수정 제안
- **효과**: BCG-Harvard-Wharton-MIT 공동 연구(Dell'Acqua et al. 2023, HBS WP 24-013)에서 GPT-4 사용군이 비사용군 대비 작업 완수율 +12.2%, 품질 +40%, 속도 +25%. 단 "프런티어 밖" 작업에서는 정확도 -19%p
- **교훈**: **"Centaur vs Cyborg" 사용 패턴** — AI를 부분 위임(centaur)하는 사용자보다 전면 통합(cyborg)하는 사용자가 우수
- **출처**: HBS Working Paper 24-013

### 1.3 Bain & Company × OpenAI
- 2023년 글로벌 서비스 얼라이언스. 사내·고객 양쪽에 GenAI 솔루션. 2024년 Coca-Cola·Microsoft 등 공동 프로젝트
- 효과: 일부 리서치 작업 시간 20–60% 단축 (Bain 자체 추정)

### 1.4 M&A 실사 자동화 — Hebbia, Rogo
- **Hebbia** ($130M Series B, a16z 2024): 수천 페이지 PDF·CIM 분석. JP Morgan, Charles Schwab, Bridgewater 사용 보도
- **Rogo** ($50M Series B, 2024): 투자은행 분석가용. Tiger Global·BNY Mellon
- 워크플로: 데이터룸 업로드 → 재무 모델·계약 조항·리스크 추출 → 표준 실사 체크리스트 자동 채움 → 분석가 검수
- **교훈**: **"답"이 아닌 "발췌+근거 페이지 링크(citation)"를 출력**해야 신뢰 확보 (Hebbia 차별점)

### 1.5 사내 워크 OS — Notion AI / Coda AI / ClickUp Brain
- Notion AI(2023.02), Coda AI(`AI.text()` 컬럼 함수), ClickUp Brain(2024)
- 채택률 패턴: **embedded AI** (별도 챗 X, 문서 안의 inline 블록)이 별도 챗 패널보다 우위
- 교훈: **워크 OS의 AI는 "검색 + 요약 + 작성" 3종 세트가 80% 가치**. 복잡한 추론보다 일상 마찰 제거가 우선

### 1.6 PRD 자동화 — Productboard, Linear
- **Productboard Pulse** (2024): 고객 피드백·인터뷰 클립 → 테마 자동 클러스터링 → PRD 초안 제안
- **Linear** (2024 후반~2025): 이슈 자동 분류·중복 검출. PRD 전체 생성보다 *마찰 제거(triage, summarization)*에 집중
- Linear blog (2025): *"AI should reduce work, not add review work"*
- 교훈: **PRD 전체 자동 생성은 PM 평가가 엇갈림**. 이유: PRD의 핵심 가치가 "쓰기"가 아니라 "생각 정리·이해관계자 정렬"

### 1.7 데이터 의사결정 — Hex, Mode, ThoughtSpot
- Hex Magic, ThoughtSpot Sage, Mode AI Assist
- 워크플로: **시멘틱 레이어** → LLM이 메타데이터 인지 → SQL 생성 → 결과 자연어 해설
- 교훈: **시멘틱 레이어 없는 자연어 BI는 환각률이 높음**. "스키마 ground truth" 우선 투자가 성공 조건

## 2. 디자인 영역

### 2.1 Figma AI / Figma Make
- 2024 Config: Figma AI(자동 레이어 명명, 검색, First Draft). 2025: Figma Make(자연어 → 인터랙티브 프로토타입), Sites, Buzz
- 효과: First Draft 사용자가 비사용자 대비 초안 시간 50–70%↓ (Figma 자체)
- **사고 사례**: 2024.07 First Draft가 "이미 존재하는 사이트와 닮은 결과" 산출 (Andy Allen 트윗) → Figma 일시 비활성화 후 재출시
- 교훈: **데이터 셋 거버넌스가 디자인 AI의 평판 리스크 핵심**

### 2.2 Adobe Firefly + Express
- Adobe Stock + 라이선스 콘텐츠 학습 → "**상업적 안전(commercial safety)**" 모델 강조
- Firefly Services API로 기업 워크플로(자동 배너, 로컬라이즈) 통합. Coca-Cola, IBM, Mattel 사례
- 교훈: B2B 디자인 AI는 **IP 안전성 보증(indemnification)**이 도입 결정 핵심 변수

### 2.3 v0 / Framer AI / Stitch (구 Galileo AI)
- v0 by Vercel: 자연어 → React/Tailwind/shadcn. 2024–2025 풀스택 Next.js 프로젝트로 확장
- Framer AI: 자연어 → 라이브 웹사이트
- Stitch (Google, 구 Galileo, 2025 I/O): UI 디자인 → Figma export
- 핵심: 모두 "**디자인 시스템(DS)을 입력으로**" 받음
- 교훈: **AI 디자인 도구의 효용은 DS 토큰화·컴포넌트 카탈로그의 질에 비례**. DS 정비는 AI 도입의 사전조건

### 2.4 Figma Dev Mode + Code Connect (2024)
- Figma 컴포넌트를 코드 베이스의 실제 컴포넌트와 매핑. 핸드오프 시 Frame이 *실제 React import 코드*로 표시
- 사례: Stripe, GitHub (Config 2024)
- 교훈: 핸드오프 자동화의 본질은 LLM이 아니라 **결정론적 매핑**. AI는 매핑 후보 추천에만 사용

### 2.5 UX 리서치 — Maze, UserTesting, Dovetail
- Dovetail Magic(2024): 인터뷰 영상 트랜스크립트 자동 태깅·테마 추출. 인용 시 *원본 영상 timestamp 링크* 자동
- 교훈: **"인용 가능한 인사이트(traceable insight)"** — 모든 AI 요약이 원 발화로 역추적 가능해야 신뢰

### 2.6 디자인 AI 운영 패턴
1. AI 산출물은 항상 "draft" 마킹
2. 시니어 디자이너 리뷰 후 라이브러리 승격(component promotion)
3. 브랜드/접근성 자동 검사(Stark, Polypane)를 PR 단계에 결합

## 3. 제품 기획(PM) 영역

### 3.1 Atlassian Rovo
- 2024.05 발표, 2024.10 GA. Jira/Confluence + 타사 SaaS 검색·요약 + Rovo Agents + Rovo Studio
- 워크플로: PM이 "다음 분기 OKR 초안" 요청 → Rovo가 지난 분기 epic·회고·티켓을 RAG 통합 → 초안 + 출처
- 교훈: **이미 PM이 일하는 곳(Jira/Confluence) 안에서 작동**. 사이드 패널 형태

### 3.2 Microsoft Copilot for PMs (M365 + Loop + Planner)
- Copilot Studio로 KPMG, Visa, Bayer가 사내 PM 에이전트 구축 (Ignite 2024)
- 교훈: 엔터프라이즈에서 **신원·권한(identity)이 가장 큰 진입장벽**. M365 채택률 우위는 Entra ID 통합 덕

### 3.3 PRD 자동화 — ChatPRD, Reforge, Productboard
- ChatPRD: PM 직군 전용 GPT 래퍼. Lenny Rachitsky 추천으로 인지도 상승
- Reforge Artifacts: 검증된 PRD 템플릿 + AI 작성. 차별점: "베스트프랙티스 기반 제약(constrained generation)"
- 교훈: **"빈 페이지 채우기"는 잘 풀지만, "이해관계자 정렬"은 AI가 못 푼다**

### 3.4 사용자 인터뷰 분석 — Notably, Marvin, Dovetail
- 자동 코딩(automated coding) 50–70% 정확 — 리서처 검수 필수, 그러나 첫 패스 비용 80%↓

### 3.5 우선순위 결정 보조
- ICE/RICE 점수의 자동 *제안*은 유용, 자동 *결정*은 위험. 우선순위는 정치적 합의가 본질이라 사람이 책임

## 4. 영역별 공통 패턴

### 4.1 인풋 패턴
- **사내 IP가 1차 입력**: McKinsey Lilli, BCG GENE, Atlassian Rovo, Productboard Pulse 모두 외부 LLM + 사내 RAG. 외부 모델만으로는 차별화 불가
- **시멘틱 레이어/디자인 시스템/온톨로지**: Hex(스키마), Figma(컴포넌트), Atlassian(이슈 타입). **AI 이전에 "구조"를 정비**한 조직이 성공
- **권한 인지 검색**: Microsoft Copilot, Glean, Rovo가 표준화. 사용자가 볼 수 없는 문서는 검색에도 안 뜸

### 4.2 아웃풋 패턴
- **Always-cited**: 모든 답변에 출처 페이지/타임스탬프 deep-link. Hebbia·Dovetail·Lilli 공통
- **Draft-by-default**: AI 산출물은 "초안" 상태. 승격(promotion) 워크플로 별도
- **Embedded over chat**: Notion·Coda·Linear 채택률은 *별도 챗 패널이 아닌 inline 블록*에서 우위

### 4.3 검증 패턴
- HITL이 법무·실사·디자인 라이브러리 등 모든 *외부 영향 산출물* 에 필수
- 사내 골든 셋 정의 → 모델 업그레이드 시 리그레션 측정 (Atlassian, Stripe 사례)
- **Confidence + abstain**: 답을 모르면 "모른다" — Lilli는 "관련 자료 없음 → 사람 전문가 추천"으로 폴백

### 4.4 승인 패턴
- **3단계 게이트**: AI 생성 → 동료 리뷰 → 책임자 승격. 디자인 라이브러리·PRD·재무 모델 공통
- 감사 로그: 누가·어떤 프롬프트로·무엇을 생성·승인했는지 보존

## 5. 안티패턴

1. **AI for AI's sake** — 마찰을 줄이지 않고 추가. Linear가 명시 경계: *"AI should reduce work, not add review work"*
2. **데이터 거버넌스 무시** — Figma First Draft(2024.07) 표절 논란; M365 Copilot 초기 배포에서 권한 미스매핑으로 임원 문서 노출 보도(*Business Insider* 2024)
3. **"전체 자동화" 환상** — PRD 풀 자동화. 가치는 *문서가 아니라 정렬 과정*
4. **시멘틱 레이어 없는 NL2SQL** — 환각으로 잘못된 매출 보고. Hex/ThoughtSpot 모두 "시멘틱 레이어 우선" 발표
5. **평가 부재** — 모델 업그레이드(GPT-4 → GPT-4o) 후 출력 포맷 변화로 다운스트림 파이프라인 깨짐
6. **Cyborg vs Centaur 무시** — 동일 도구라도 사용 패턴 차이가 성과의 큰 부분. 교육 없이 "도구만 깔아주는" 도입은 실패
7. **핸드오프 자동화 과신** — AI가 디자인→코드 변환을 "끝낸다"는 마케팅에 속지 말 것. 결정론적 매핑 + AI 추천 하이브리드만 운영 가능

## 6. 우리 회사 협업 시스템 설계 인사이트

1. **사내 지식 그래프(internal knowledge graph)를 1급 시민으로** — 제품·고객·OKR·회의록·디자인 컴포넌트를 단일 온톨로지로 연결. RAG 품질이 시스템 품질의 상한
2. **Embedded AI > Standalone Chat** — 이미 일하는 도구(Notion, Figma, Linear, Slack) 안에서 우클릭·슬래시 명령. 별도 포털 X
3. **모든 산출물은 출처 + 초안 상태(citation + draft-by-default)** — 메타데이터에 `generated_by`, `source_refs[]`, `status=draft` 필수
4. **승격(promotion) 워크플로 명시 설계** — PRD·디자인 컴포넌트·SQL 쿼리·재무 모델 각각 (1) AI 초안 → (2) 동료 리뷰 → (3) 오너 승격 3단계
5. **시멘틱 레이어·DS 정비를 AI 도입 전에** — dbt + semantic layer, 토큰화된 DS + Code Connect, 이슈 분류 체계를 6개월 선행 투자
6. **사람의 판단이 본질인 작업은 보조만** — 우선순위 결정, PRD 합의, 디자인 방향성은 *제안 + 옵션 비교*까지만. 결정 책임은 사람
7. **Eval harness를 first-class 인프라로** — 골든 셋(과거 PRD/디자인 리뷰/M&A 메모 50–200건)을 사내 비공개로 보관, 모델·프롬프트 변경 시 자동 회귀
8. **Cyborg 사용 패턴 교육 + 측정** — 도구 배포 시 4시간 워크숍 + 매월 사용 패턴(질문 유형, 산출물 채택률) 측정. BCG 연구 직접 인용 가능
9. **MCP 등 표준 프로토콜로 도구 연결** — Claude·Cursor·Devin·내부 에이전트가 동일 MCP 서버를 통해 사내 도구 접근. 도구별 1대1 통합 회피
10. **위험 등급별 자율성 차등(risk-tiered autonomy)**:
    - Tier 1 (요약·검색): 자동 실행
    - Tier 2 (초안 작성): 자동 생성, 사람 승인 후 게시
    - Tier 3 (외부 발신·재무·법무): 항상 2인 승인 + 감사 로그
