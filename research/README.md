# Research Notes

사내 AI 에이전트 협업 시스템 설계를 위한 리서치 노트입니다.
각 포지션(PM, 기획, 디자이너, FE, BE, QA, DevOps)이 개인 AI 에이전트를 활용하면서 공통 목표(서비스 런칭/제작)를 위해 협업할 때의 워크플로와 규약을 설계하기 위한 근거 자료입니다.

> **조사 시점**: 2026년 5월. 2024~2026년 공개 자료 기반.
> **신뢰도 주의**: 일부 수치는 벤더 자가보고이며, 외부 인용 시 1차 출처 재확인 필요.

## 구성

| 파일 | 범위 |
|---|---|
| [`00-synthesis.md`](./00-synthesis.md) | 리서치에서 추출한 핵심 원칙 + 안티패턴 (CLAUDE.md의 직접 근거) |
| [`01-agent-architecture.md`](./01-agent-architecture.md) | 에이전트 아키텍처 패턴 · 프레임워크 · 도구 표준(MCP) |
| [`02-team-workflow.md`](./02-team-workflow.md) | 팀 협업 워크플로 사례 · 핸드오프 · 코딩 에이전트 실무 |
| [`03-hitl-governance.md`](./03-hitl-governance.md) | HITL · 권한 등급 · 거버넌스 · 실패 사례(Klarna 등) |
| [`04-ai-artifact-trust.md`](./04-ai-artifact-trust.md) | AI 산출물 신뢰 등급(L1/L2/L3) · Dev-ready 게이트 · SSOT 6섹션 |

## 사용 방법

1. `00-synthesis.md`로 핵심 원칙을 먼저 검토
2. 상세는 01~03에서 사례·교훈 확인
3. 시스템 적용은 루트 `CLAUDE.md` 참조
