# Research — 에이전트 아키텍처 패턴 및 도구 표준

> 조사 시점: 2026년 5월. 외부 인용 시 1차 출처 재확인 권장.

## 1. Anthropic: 워크플로 패턴 5가지

출처: [Building Effective AI Agents](https://www.anthropic.com/research/building-effective-agents)

Anthropic은 에이전틱 시스템을 **워크플로**(사전 정의된 코드 경로)와 **에이전트**(LLM이 스스로 프로세스 제어)로 구분하고, "가장 단순한 해법부터 시작하라"고 권고한다.

### 1.1 다섯 가지 워크플로 패턴

| 패턴 | 설명 | 적합 상황 |
|---|---|---|
| **Prompt Chaining** | 한 LLM 출력이 다음 LLM 입력 | 순차적 단계가 명확한 작업 |
| **Routing** | 입력을 분류하여 전문 경로로 분배 | 유형별 처리가 다른 작업 |
| **Parallelization** | 동시 실행 후 결과 합산 (voting / sectioning) | 독립적 하위 작업 |
| **Orchestrator-Workers** | 리드가 분해·위임·합성, 워커가 병렬 실행 | 복잡한 다단계 작업 |
| **Evaluator-Optimizer** | 생성 → 평가 → 개선 반복 | 명확한 평가 기준이 있는 작업 |

### 1.2 핵심 원칙
- **워크플로 우선**: "워크플로로 충분한 일에 에이전트를 쓰지 마라"
- **단순성**: 에이전틱 시스템은 지연·비용을 증가시키므로, 그만큼의 가치가 있을 때만
- **도구 설계**: 소수의 신중하게 설계된 도구부터 시작, 점진적 확장

## 2. Anthropic: 멀티 에이전트 리서치 시스템 (MARS)

출처: [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)

### 2.1 아키텍처
- **Lead Agent** (Opus): 쿼리 분석 → 전략 수립 → 서브에이전트 생성·조율
- **Sub-agents** (Sonnet): 각각 독립적 리서치 수행, 병렬 실행
- Lead가 결과를 합성하여 최종 보고서 생성

### 2.2 핵심 학습
- **명시적 작업 명세 필수**: "반도체 부족 조사해"처럼 모호한 위임 → 중복·누락 발생. 목적·출력형식·도구·범위를 명시해야 함
- **노력 스케일링**: 에이전트는 작업 난이도에 맞는 적절한 노력 수준을 판단하기 어려움 → 프롬프트에 스케일링 규칙 내장
- **성능**: 멀티 에이전트가 단일 에이전트 대비 90.2% 우수 (내부 리서치 평가)
- **비용**: 일반 채팅 대비 약 15배 토큰 소비 → "결과 가치 > 비용"일 때만 사용

## 3. OpenAI: 에이전트 구축 실용 가이드

출처: [A practical guide to building agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/)

### 3.1 권고사항
- **단일 에이전트 우선**: 멀티 에이전트는 단일로 부족할 때만 도입
- **핸드오프는 일방향 위임**: 오케스트레이션 패턴에서 에이전트 간 핸드오프는 one-way transfer
- **가드레일**: 입력 필터링 → 도구 사용 제한 → HITL 개입까지 모든 단계에 가드레일 적용
- **고위험 작업은 사람 승인**: 비가역적·고액·민감한 작업은 반드시 사람이 확인

### 3.2 Agents SDK (2025~)
- Python/TypeScript 오픈소스
- tool use, handoffs, guardrails, tracing 기본 제공
- provider-agnostic (OpenAI 외 모델도 사용 가능)

## 4. MCP (Model Context Protocol) — 도구 표준

출처: [MCP Specification](https://modelcontextprotocol.io/specification/2025-11-25), [2026 MCP Roadmap](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/)

### 4.1 개요
- Anthropic이 2024년 11월 공개한 오픈 표준
- LLM이 외부 도구·데이터 소스와 연결되는 방식을 표준화
- 2026년 4월 기준: 월간 SDK 다운로드 9,700만+, GitHub 스타 81,000+

### 4.2 채택 현황 (2026)
- Anthropic, OpenAI, Google, Microsoft, AWS 모두 지원
- 기업 AI 팀 78%가 최소 1개 MCP 기반 에이전트 운영 중
- CTO 67%가 "12개월 내 MCP를 기본 에이전트 통합 표준으로 채택" 응답

### 4.3 2026 로드맵 주요 과제
- Streamable HTTP: 원격 서비스로서의 MCP 서버 운영
- 수평 확장, 세션 관리, 레지스트리 표준화
- 멀티 에이전트 오케스트레이션 프리미티브 (아직 표준화 진행 중)

### 4.4 우리 회사 적용 시사점
- 사내 도구(Jira, Figma, GitHub, Slack 등)를 MCP 서버로 노출하면 각 포지션 에이전트가 통일된 방식으로 도구 접근 가능
- 에이전트별 도구 가시성을 namespace로 제어 가능 (보안)

## 5. AI 코딩 에이전트 현황 (2026)

출처: [AI Coding Agents 2026](https://www.digitalapplied.com/blog/ai-coding-agents-claude-code-cursor-codex-replit-2026), [AI Coding Agents: What Actually Works](https://deepfounder.ai/ai-coding-agents-2026-guide/)

### 5.1 주요 도구
| 도구 | 모드 | 특징 |
|---|---|---|
| **Claude Code** | 감독형 페어프로그래밍 (CLI) | CI/CD 파이프라인 통합에 강점, 복잡한 추론 |
| **Cursor** | 감독형 IDE 내장 | 일상 개발 생산성, 코드베이스 인지 |
| **Codex Desktop** | 감독형 | GitHub Actions 네이티브 통합 |
| **Devin** | 비동기 자율 위임 | 백로그 클리어링, 종료 후 리뷰 |
| **Replit Agent 3** | 제한적 비동기 | 호스팅 런타임 포함 |

### 5.2 팀 운영 패턴
- 가장 효과적인 팀은 **복합 도구 전략** 사용: Cursor(일상) + Claude Code(복잡 추론) + Devin(비동기 백로그)
- PR 기반 리뷰 프로세스에 자연스럽게 통합되는 도구가 팀 워크플로에 적합
- CI/CD 파이프라인에 에이전트를 연결하여 코드 품질·보안·문서 자동 검사 가능
