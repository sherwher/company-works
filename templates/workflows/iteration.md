# Workflow — Iteration (반복 사이클)

> 공통 규약은 [`../CLAUDE.md`](../CLAUDE.md). Full Cycle은 [`./full-cycle.md`](./full-cycle.md).
> 본 워크플로의 근거는 메타 리포의 [`research/05-iteration-cadence.md`](../../research/05-iteration-cadence.md).

## 위치
```
Epic (EP-YYYY-NN)         ← 불변 전제. PRD 위 단계.
  └─ Iteration (IT-...)    ← 본 워크플로가 다루는 단위.
       └─ Work Unit (WU-...)
```

Full Cycle은 **Epic 1개당 1회** 통과한다. Iteration은 그 Epic을 **N회 반복**으로 쪼개 실행하는 사이클이다.

## 흐름 (1~2주 고정)

```
Iteration Plan ─▶ Execute (WU 다발) ─▶ Iteration Review ─▶ Re-plan(다음 IT)
                       │
                       └─ WU별로 Dev-ready Review 반복 호출 (필요 시)
```

## 단계

### 1. Iteration Plan (시작일, 60분 이내)
- 입력: Epic Brief, 직전 IT의 Review 결과(있으면), 미해결 Open Questions.
- 출력: Iteration 산출물 1건 (`IT-YYYYMM-NN.md`).
- 의무 항목:
  - Iteration Goal (한 줄)
  - 포함 WU 목록 + 책임 포지션
  - Epic Success Metrics 중 본 사이클이 영향 주는 KR
  - Dev-ready 게이트 호출 예정 횟수
  - 위험·블로커 + 완화

### 2. Execute (사이클 본문)
- 각 WU는 `shared/outputs.md` Work Unit 양식.
- WU별 Dev-ready Review는 [`./dev-ready-review.md`](./dev-ready-review.md) 그대로.
- Decision Log는 발생 즉시 작성 (PR과 같은 브랜치).

### 3. Iteration Review (종료일, 60분 이내)
- 데모: WU별 Deliverable 시연(가능한 것).
- 회고 4개 항목: Kept / Dropped / Learned / Next.
- 산출물 갱신: Iteration 파일에 Review 결과 append + status `closed`.
- **회고 없이 다음 IT 시작 금지** (안티패턴 13).

### 4. Re-plan
- Review의 "Next" 항목을 다음 Iteration Plan의 후보 WU로 이월.
- Epic Success Metrics 진척이 정체되면 Epic 차원 재검토 (Owner 판단).

## 게이트 / 권한
- Iteration 시작·종료 결정은 Owner 단독(L2). Decision Log 1건 생성.
- 사이클 중 Epic 범위 변경이 필요하면 → 에스컬레이션(S7.3) + 별도 DEC.

## 입력
- Epic Brief (`EP-YYYY-NN.md`)
- 직전 Iteration Review (있으면)
- 활성 포지션 카탈로그 (CLAUDE.md S3)

## 출력
- `IT-YYYYMM-NN.md` 1건 (Plan + 종료 시 Review 섹션 append)
- Decision Log (필요 시)
- 다음 IT 후보 WU 목록

## 안티패턴
- Iteration Goal 없이 WU만 모음 → "이번 사이클이 무엇을 의미하는가" 답할 수 없음.
- 회고를 비공식 채팅으로 끝냄 → 학습 휘발.
- Iteration이 시간 박스를 넘김 → 다음 사이클 압박, 누적 부채.
- WU가 IT를 넘어 무한 연장 → 분할 또는 Epic 재정의 필요.

## 출처
- 메타 리포 [`research/05-iteration-cadence.md`](../../research/05-iteration-cadence.md).
