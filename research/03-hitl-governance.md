# Research — HITL · 권한 등급 · 거버넌스 · 실패 사례

> 조사 시점: 2026년 5월. 외부 인용 시 1차 출처 재확인 권장.

## 1. HITL (Human-in-the-Loop) 프레임워크

### 1.1 3단계 자율성 모델

출처: [Strata HITL Guide](https://www.strata.io/blog/agentic-identity/practicing-the-human-in-the-loop/), [IBM HITL](https://www.ibm.com/think/topics/human-in-the-loop), [ByteBridge HITL to HOTL](https://bytebridge.medium.com/from-human-in-the-loop-to-human-on-the-loop-evolving-ai-agent-autonomy-c0ae62c3bf91)

| 단계 | 설명 | 적합 상황 |
|---|---|---|
| **HITL** (Human-in-the-Loop) | 사람이 실행 전 승인·수정 | 고위험: 비가역적, 금전적, 민감한 작업 |
| **HOTL** (Human-on-the-Loop) | AI가 자율 실행, 사람이 사후 모니터링·개입 | 중위험: 속도 중요하되 실수가 복구 가능 |
| **HOOTL** (Human-out-of-the-Loop) | 완전 자율, 모니터링만 | 저위험: 사전 정의된 안전한 작업 |

### 1.2 동적 권한 아키텍처

출처: [Permit.io HITL Best Practices](https://www.permit.io/blog/human-in-the-loop-for-ai-agents-best-practices-frameworks-use-cases-and-demo)

핵심 발견: 같은 워크플로 안에서도 단계별로 감독 수준이 달라야 한다.
- 예: 항공편 예약(저위험) → 벤더 계약 협상(고위험) → 같은 에이전트지만 다른 감독 수준
- **정책 기반 동적 감독**: 의사결정 위험도에 따라 감독 수준을 실시간 조정
- **필수 구성요소**: 에이전트 실행 일시정지 → 승인 요청 라우팅 → 시간 제한 결정 창 → 모든 개입 감사 로그

### 1.3 위험 기반 승인 트리거

비가역적 결정인가? → 금전 거래, 데이터 삭제, 프로덕션 시스템 변경, 설정 변경은 **실행 전 사람 승인 필수**.

2025~2026년 업계 합의: 균일한 승인 요구가 아니라 **의사결정 위험도에 따라 동적으로 감독을 조정**하는 방향으로 수렴 중.

## 2. 권한 등급 설계

### 2.1 OpenAI 권고

출처: [OpenAI Practical Guide](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/)

- 민감하거나 비가역적이거나 고액인 작업 → 에이전트 신뢰도가 충분해질 때까지 사람 감독
- 가드레일을 모든 단계(입력 필터링, 도구 사용, HITL 개입)에 적용

### 2.2 Microsoft Agent Framework

출처: [Microsoft HITL Workflows](https://learn.microsoft.com/en-us/agent-framework/workflows/human-in-the-loop)

- 에이전트 워크플로의 정의된 체크포인트에서 시스템이 일시정지하고 사람 승인 대기
- 승인 흐름, 거부 흐름, 타임아웃 처리가 기본 제공

### 2.3 우리 회사에 적용할 5단계 등급

리서치 종합 결과, 서비스 개발 맥락에 맞는 권한 등급:

| 등급 | 정의 | 예시 | 승인 방식 |
|---|---|---|---|
| **L0** | 읽기 전용 | 코드·문서·티켓 조회, 검색, 요약 | 자동 |
| **L1** | 개인 범위 수정 | 브랜치 내 코드 수정, 초안 작성, 로컬 테스트 | 자동 (에이전트 소유자) |
| **L2** | 공유 자원 수정 (가역) | PR 생성, 이슈 코멘트, 사내 문서 초안 | 리뷰어 1인 |
| **L3** | 운영 영향 | 배포, DB 마이그레이션, 외부 API 호출 | 사람 승인 필수 |
| **L4** | 비가역 | 데이터 삭제, force-push, 프로덕션 롤백 | 사람 승인 + 2차 검토 |

## 3. 실패 사례: Klarna

### 3.1 도입 (2024.02)

출처: [Klarna Press Release 2024.02](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/)

- AI Assistant 출시 1개월 만에 230만 건 대화 처리
- "700명 풀타임 상담원에 상응" 발표
- AHT 11분 → 2분, 반복 문의 25% 감소
- "CSAT는 인간 상담원과 동등" 주장

### 3.2 후퇴 (2025.05)

출처: [Klarna AI Rollback](https://lasoft.org/blog/klarna-walks-back-ai-overhaul-rehires-staff-after-customer-service-backlash/), [FinTech Weekly](https://www.fintechweekly.com/magazine/articles/klarna-hires-customer-service-after-ai-pivot)

- CEO: "효율성과 비용에 너무 집중했다. 결과는 품질 하락이었고, 지속 가능하지 않다"
- **고객 만족도 22% 하락**
- 인간 상담원 재채용 (Uber형 원격·온디맨드 모델)

### 3.3 핵심 교훈

1. **CSAT는 후행 지표**: AI 도입 직후에는 유지되지만 6~12개월 후 복잡 케이스 누적으로 하락
2. **학습 신호 고갈**: 학습 데이터가 인간 상담 로그인데, 인간을 줄이자 새 학습 신호가 고갈
3. **에지 케이스 과소평가**: 분쟁, 사기, 복잡한 금융 문의는 AI가 처리할 수 없었음
4. **브랜드 훼손**: "Klarna는 사람과 통화 못한다"는 평판이 SNS 확산
5. **단일 KPI 함정**: AI deflection(AI 처리율)만 측정 → Quality-adjusted resolution rate 필요

### 3.4 우리 적용 시사점
- AI 처리율을 단독 KPI로 삼지 말 것
- "사람 호출" 경로는 항상 열어둘 것
- 품질 조정 후의 성과만 의미 있음 (Quality-adjusted metrics)
- 에지 케이스·복잡 케이스는 처음부터 사람 경로로

## 4. 거버넌스 원칙

### 4.1 산출물 관리
- **Draft-by-default**: 모든 에이전트 산출물은 "초안" 상태로 시작
- **3단계 승격**: AI 초안 → 동료 리뷰 → 오너 승격
- **Citation 의무**: 모든 산출물에 `generated_by`, `source_refs[]`, `status` 메타데이터

### 4.2 검증 우선순위
```
결정적 검사기 (컴파일러, 테스트, 스키마, 린터)
    → LLM 판단 (AI 리뷰)
        → 사람 리뷰
```
- 환각을 LLM으로 잡으려 하지 말 것. 결정적 검사가 먼저.

### 4.3 "Suggest, don't merge"
- 자동 머지·자동 발행은 기본이 아님. 옵트인.
- PR/Draft 형태로 사람의 명시 승인이 기본값

### 4.4 관측성 (Observability)
- 모든 에이전트 호출의 trace 저장 (입력, 도구 호출, 출력, 비용, 지연)
- 주간 회귀 + 분기 감사
- 비용 추적: 토큰 + 도구 호출 비용 per task

## 5. 안티패턴 요약

| # | 안티패턴 | 실증 사례 | 대안 |
|---|---|---|---|
| 1 | AI 처리율을 단독 KPI | Klarna 22% CSAT 하락 | Quality-adjusted metrics |
| 2 | 사람 핸드오프 경로 폐쇄 | Klarna 브랜드 훼손 | 항상 사람 호출 가능 |
| 3 | 자동 머지 기본값 | 보안 취약점 유입 | Suggest, don't merge |
| 4 | 모호한 에이전트 위임 | Anthropic MARS 초기 실패 | 명시적 작업 명세 6슬롯 |
| 5 | 에이전트 한계 밖 작업 위임 | BCG 연구 정확도 -19%p | 한계 인식 + 에스컬레이션 |
| 6 | 평가 없이 배포 | 감지 못한 품질 저하 | Eval-first: 골든셋부터 |
| 7 | 관측성 부재 | 비용 폭증, 장애 원인 불명 | Day-1 trace 저장 |
| 8 | 단일 에이전트에 과도한 권한 | 보안 사고 | 최소 권한 + 등급별 분리 |
