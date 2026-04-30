# Research Notes

사내 AI 에이전트 협업 시스템 설계를 위한 사전 리서치 노트입니다. 멀티에이전트 프레임워크, 도메인별(개발·전략·디자인·기획·마케팅·QA·고객지원) 도입 사례, 횡단 패턴(거버넌스·평가·메모리·HITL)을 정리하고, 최종적으로 모범사례(`00-synthesis.md`)로 통합합니다.

> **신뢰도 주의**: 본 노트는 4개의 리서치 서브에이전트가 학습된 공개 자료(2024~2026 공식 블로그·논문·언론 보도)를 기반으로 작성한 것입니다. 일부 ROI 수치는 벤더 자가보고이며, 외부 인용 시 1차 출처 재확인이 필요합니다.

## 구성

| 파일 | 범위 |
|---|---|
| [`00-synthesis.md`](./00-synthesis.md) | 4개 노트에서 추출한 횡단 모범사례·원칙 (협업 시스템의 직접 근거) |
| [`01-frameworks-and-dev.md`](./01-frameworks-and-dev.md) | 멀티에이전트 프레임워크 + SW 개발 협업 사례 |
| [`02-strategy-planning-design.md`](./02-strategy-planning-design.md) | 사업 전략·기획·디자인 영역 도입 사례 |
| [`03-marketing-and-qa.md`](./03-marketing-and-qa.md) | 마케팅·QA 영역 도입 사례 |
| [`04-customer-support-and-patterns.md`](./04-customer-support-and-patterns.md) | 고객지원 사례 + 횡단 협업 패턴 (HITL·메모리·거버넌스·평가·프로토콜) |

## 사용 방법

1. `00-synthesis.md`로 모범사례 18개 원칙을 먼저 검토.
2. 도메인별 상세는 01~04에서 사례·교훈 확인.
3. 시스템 적용은 루트 `CLAUDE.md` 참조 (본 노트의 원칙이 시스템 설계의 근거).
