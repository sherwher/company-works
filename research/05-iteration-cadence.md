# 05 — Iteration Cadence (Epic / Iteration / Work Unit 위계)

본 노트는 `templates/`에 Epic/Iteration 계층을 도입하는 근거를 정리한다.
검증 케이스(`test-runs/senior-behavior-ai/`)에서 "Full Cycle 1회 + WU만"으로는 장기 프로젝트의 반복 리듬을 표현할 수 없다는 점이 드러나 추가한다.

## 1. 문제 정의

기존 템플릿은 다음을 가정했다.
- 프로젝트 = 1회의 Full Cycle (PRD → ... → 배포)
- 최소 작업 단위 = Work Unit (1~3일)

실제 운영은 다르다.
- **장기 전제(Epic/Objective)**가 수개월 유지되며 변하지 않는다.
- 그 아래 **반복 사이클(Iteration/Sprint)**이 1~2주 단위로 돌고, 각 사이클마다 회고·재계획이 발생한다.
- 한 Iteration 안에서 여러 **Work Unit**이 병렬·순차로 처리된다.
- 동일 Full Cycle의 게이트(특히 Dev-ready Review)는 **WU별로 반복 호출**된다.

WU만으로는 "이 WU가 어느 큰 그림에 속하는가", "이번 사이클에서 무엇이 끝났는가"를 추적할 곳이 없다.

## 2. 외부 근거

### 2.1 Scrum / Agile (반복 사이클)
- Scrum Guide (Schwaber & Sutherland, 2020): Sprint(1~4주)는 "잠재적으로 출하 가능한 증분"을 만드는 단위. Sprint Goal로 사이클의 단일 전제를 묶고, 회고로 다음 사이클을 조정.
- 적용: Iteration = Sprint. 시간 박스 + 회고 의무.

### 2.2 Shape Up (Basecamp, 2019)
- 6주 cycle + 2주 cooldown. cycle 시작 전 "shaped pitch"가 전제 = Epic에 해당.
- Pitch는 Problem / Appetite / Solution / Rabbit Holes / No-gos 5요소. 본 템플릿의 PRD/Epic과 1:1 대응 가능.
- 적용: Epic 양식에 "Appetite(투입 가능 시간 상한)"와 "Rabbit Holes(피할 함정)" 항목 권장.

### 2.3 OKR (Doerr, *Measure What Matters*, 2018)
- Objective(질적 목표) + Key Results(측정 가능한 결과 3~5개), 분기 단위 갱신.
- 적용: Epic의 Success Metrics를 KR 형태로 강제하면 측정 가능성이 보장됨. 본 템플릿 PRD §4 Success Metrics와 호환.

### 2.4 Anthropic / OpenAI 에이전트 가이드
- Anthropic, *Building effective agents*: "긴 자율 실행은 평가 어려움. 짧은 사이클 + 평가 + 사람 게이트가 신뢰 형성".
- OpenAI, *Practical Guide to Building Agents*: "각 turn마다 명시적 종료 조건". Iteration의 회고 = turn 단위 종료 조건과 동형.
- 적용: AI 산출물 신뢰 등급(L1/L2/L3)은 Iteration 안에서 누적 검증되는 구조가 자연스러움.

## 3. 도입 결정

### 3.1 위계
```
Epic (EP-YYYY-NN)
  └─ Iteration (IT-YYYYMM-NN)
       └─ Work Unit (WU-YYYYMM-NN)
            └─ Decision Log (DEC-YYYYMM-NN)
```

### 3.2 시간/책임
| 계층 | 길이 | 책임자 | 산출물 |
|---|---|---|---|
| Epic | 1~6개월 | Owner (보통 PM) | Epic Brief + Success Metrics |
| Iteration | 1~2주 | Owner + 활성 포지션 | Iteration Plan + Review |
| Work Unit | 1~3일 | 단일 포지션 | 포지션별 Deliverable + 핸드오프 |

### 3.3 게이트와의 관계
- Full Cycle 게이트(G1~G7)는 **Epic 1개당 1회 통과**하는 큰 흐름.
- Iteration 안에서는 Dev-ready Review가 WU별로 반복 호출됨.
- 한 Iteration이 끝날 때 **Iteration Review**(데모 + 회고)를 의무화. 회고 없이 다음 사이클 시작 금지.

## 4. 안티패턴 추가 (CLAUDE.md S9 후보)

| # | 안티패턴 | 왜 위험한가 |
|---|---|---|
| 12 | Epic 전제 없이 WU만 쌓기 | 큰 그림 상실, 로컬 최적화 누적 |
| 13 | Iteration 회고 없이 다음 사이클 시작 | 평가 없이 배포(AP6)의 사이클 버전 |
| 14 | Epic Success Metrics가 측정 불가능 (정성형용사만) | OKR 부재. 종료 판정 불가 |

## 5. 검증 케이스에서의 적용

`test-runs/senior-behavior-ai/`를 다음 위계로 재구성 가능:

- **EP-2026-01 SBA MVP 출시** (3개월, Owner=이성근/Care-Tech)
  - **IT-202605-01** 페어링·베이스라인 골격 (2주)
    - WU-202605-001~005: PRD/PLAN/AISPEC/DESIGN/API
  - **IT-202606-01** 이상감지 v0.1 + 알림 (2주)
    - WU-202606-001~005: 모델 학습/평가, 알림 발송, 대시보드 추세 카드
  - **IT-202607-01** 파일럿 5가구 셰도우 + 평가 보정 (2주)
    - AI Spec §9 Open Questions(합성→운영 도메인 갭) 처리

이 구조에서 Dev-ready Review는 IT-202605-01과 IT-202606-01 양쪽에서 반복 호출되고, AP12~14가 자연스럽게 차단된다.

## 6. 출처
- Schwaber & Sutherland, *The Scrum Guide* (2020)
- Singer, *Shape Up* (Basecamp, 2019)
- Doerr, *Measure What Matters* (2018)
- Anthropic, *Building effective agents* (2024.12)
- OpenAI, *A Practical Guide to Building Agents* (2025.04)
