# Workflow: 신규 서비스 개발 (Full Cycle)

> 공통 규약은 [`../CLAUDE.md`](../CLAUDE.md). 포지션별 R&R/산출물은 [`../agents/`](../agents/).

## 흐름

```
PRD(PM) -> 기획서(기획) -> Design Spec(디자이너) -> API Contract(BE)
                                                 -> UI 구현(FE)
                                                     -> 테스트(QA) -> 배포(DevOps)
```

**AI 기능 포함 시 (조건부 분기):**
```
기획서(기획) -> AI Spec(AI엔지니어) -> 모델/파이프라인 구현(AI엔지니어) ─┐
                                                                        ├─> 통합 테스트(QA)
기획서(기획) -> Design Spec(디자이너) -> API Contract(BE) + UI 구현(FE) ─┘
```
- AI 엔지니어는 기획서에 AI 요구사항이 명시된 경우에만 합류
- BE와 AI 엔지니어가 AI 관련 API Contract를 공동 합의
- AI 모델/파이프라인은 BE API와 병렬로 개발 후 통합 테스트에서 합류

## 게이트

| 게이트 | From -> To | 통과 조건 |
|---|---|---|
| G1 | PM -> 기획 | PRD 완료: AC/지표/Non-Goals 명시, TL 검토 |
| G2 | 기획 -> 디자이너 | 기획서 완료: 모든 시나리오/정책/플로우, 디자이너/BE 검토 |
| G2-AI | 기획 -> AI엔지니어 | AI 요구사항 포함 시: AI Spec 작성 착수 |
| G3 | 디자이너 -> FE/BE | Design Spec 완료: 모든 화면 상태/컴포넌트, FE 구현 가능성 확인 |
| G4 | 기획 -> BE | 기획서의 비즈니스 로직/데이터 모델 확정, API Contract 합의 |
| G4-AI | AI엔지니어 + BE | AI Spec 완료 + AI 관련 API Contract 합의 (평가 메트릭/SLO 포함) |
| G5 | FE/BE/AI엔지니어 -> QA | CI 그린 + 자가 검증 완료 + PR 리뷰 통과 + AI 평가 기준선 충족 |
| G6 | QA -> DevOps | Critical/High 결함 0 + Go 의견 |
| G7 | DevOps -> Done | 모니터링 정상 + 롤백 준비 완료 (AI 모델 롤백 포함) |

## 오프라인 참여
- 사업전략([../agents/strategy.md](../agents/strategy.md)): G1 이전에 PM에게 시장 분석/전략 인사이트 제공
- CS([../agents/cs.md](../agents/cs.md)): G6 이후 FAQ/응대 가이드 작성 ([cs-setup.md](./cs-setup.md))
