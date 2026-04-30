# R&R (Role & Responsibility)

각 포지션의 책임 범위·인풋·아웃풋·협업 관계·DoD를 명문화한 문서 모음입니다. 모든 R&R 문서는 동일한 스키마를 따릅니다.

## 공통 스키마

```markdown
# <Position Name>

## 1. Mission
한 문단으로 이 포지션이 회사·프로젝트에 기여하는 핵심 가치.

## 2. 책임 범위 (In-Scope)
- 명확히 이 포지션이 수행하는 일

## 3. 책임 외 (Out-of-Scope)
- 이 포지션이 수행하지 않는 일(혼동되기 쉬운 항목 위주)

## 4. 표준 인풋
- 어떤 산출물을 받아 작업을 시작하는가

## 5. 표준 아웃풋
- 어떤 산출물을 만들어내는가 (템플릿 링크)

## 6. 협업 인터페이스
| 상대 포지션 | 주는 것 | 받는 것 |

## 7. Definition of Done
- 산출물이 완료되기 위한 검증 가능한 기준

## 8. 권한 등급
- L0~L4 (governance.md 참조)

## 9. KPI / 품질 지표
- 이 포지션의 성과를 측정하는 지표

## 10. 에이전트 운영 가이드
- 에이전트로 이 포지션을 운영할 때의 시스템 프롬프트 핵심 지침
```

## 활성 포지션

| 포지션 | 파일 | 핵심 산출물 |
|---|---|---|
| Product Manager | [product-manager.md](./product-manager.md) | PRD, Roadmap |
| Product Designer | [product-designer.md](./product-designer.md) | Design Spec, Flow |
| Frontend Engineer | [frontend-engineer.md](./frontend-engineer.md) | UI 구현, 컴포넌트 |
| Backend Engineer | [backend-engineer.md](./backend-engineer.md) | API, 데이터 모델 |
| QA Engineer | [qa-engineer.md](./qa-engineer.md) | Test Plan, 검증 리포트 |
| DevOps Engineer | [devops-engineer.md](./devops-engineer.md) | 배포 파이프라인, 모니터링 |
| Data Analyst | [data-analyst.md](./data-analyst.md) | 분석 리포트, 지표 정의 |
| Tech Lead | [tech-lead.md](./tech-lead.md) | 기술 의사결정, 아키텍처 |
