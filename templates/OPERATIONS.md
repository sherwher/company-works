# OPERATIONS — 운영 중인 프로젝트의 살아있는 구조

본 문서는 본 템플릿을 도입한 프로젝트가 **실제 운영하면서 산출물·결정·핸드오프를 어떻게 쌓아가는가**를 정의한다.
도입 절차는 [`SETUP.md`](./SETUP.md), 공통 규약은 [`CLAUDE.md`](./CLAUDE.md).

> 핵심: **템플릿(양식)** 과 **산출물(살아있는 결정)** 을 폴더 단위로 분리한다. 그래야 검색·갱신·인덱싱이 깔끔하다.

---

## 1. 권장 폴더 구조

```
<project>/
├── CLAUDE.md                    ← 공통 규약 (템플릿 사본)
├── agents/                      ← 포지션 부팅 파일 (템플릿 사본)
│   └── <role>.md
├── workflows/                   ← 워크플로 (템플릿 사본)
├── shared/outputs.md            ← 공통 산출물 양식 (템플릿 사본)
│
├── docs/                        ← 살아있는 산출물 (매일 늘어남)
│   ├── epics/
│   │   └── EP-2026-01-<slug>.md       ← 장기 전제 (1~6개월)
│   ├── iterations/
│   │   └── IT-202605-01-<slug>.md     ← 반복 사이클 (1~2주)
│   ├── prd/
│   │   └── PRD-2026-001-<slug>.md
│   ├── plan/
│   │   └── PLAN-2026-001-<slug>.md
│   ├── design/
│   │   └── DS-2026-001-<slug>.md
│   ├── api/
│   │   └── API-<resource>.md
│   ├── ai-spec/
│   │   └── AISPEC-2026-001-<slug>.md
│   ├── test-plan/
│   │   └── TP-2026-001-<slug>.md
│   ├── analysis/                ← 사업전략 분석 리포트
│   ├── faq/                     ← CS FAQ/응대 가이드
│   ├── campaign/                ← 캠페인 브리프
│   ├── decisions/
│   │   └── DEC-202605-001-<slug>.md   ← Decision Log (모든 비가역/범위 외 결정)
│   ├── handoffs/
│   │   └── WU-202605-001.md     ← Work Unit + 핸드오프 메시지
│   ├── glossary.md              ← 프로젝트 도메인 용어
│   └── team.md                  ← 팀/담당자 매핑
│
└── .agents/                     ← 에이전트 작업 상태/캐시 (gitignore)
```

### 1.1 분리 원칙
- **템플릿 디렉터리** (`agents/`, `workflows/`, `shared/`, `CLAUDE.md`): 거의 정적. 갱신은 [`VERSIONING.md`](./VERSIONING.md) 절차.
- **산출물 디렉터리** (`docs/`): 매일 늘어남. 검색·인덱싱 대상.
- **에이전트 상태** (`.agents/`): 로컬 캐시, 커밋 금지.

---

## 2. 산출물 ID 규칙

모든 산출물은 **추적 가능한 ID**를 갖는다. 양식 본문(`shared/outputs.md`)의 메타데이터 `id` 필드와 일치해야 한다.

| 산출물 | ID 포맷 | 예시 |
|---|---|---|
| Epic | `EP-YYYY-<seq>` | `EP-2026-01` |
| Iteration | `IT-YYYYMM-<seq>` | `IT-202605-01` |
| PRD | `PRD-YYYY-<seq>-<slug>` | `PRD-2026-001-payments` |
| 기획서 | `PLAN-YYYY-<seq>-<slug>` | `PLAN-2026-001-payments` |
| Design Spec | `DS-YYYY-<seq>-<slug>` | `DS-2026-001-payments` |
| API Contract | `API-<resource>` | `API-payments` |
| AI Spec | `AISPEC-YYYY-<seq>-<slug>` | `AISPEC-2026-001-recommend` |
| Test Plan | `TP-YYYY-<seq>-<slug>` | `TP-2026-001-payments` |
| Decision Log | `DEC-YYYYMM-<seq>-<slug>` | `DEC-202605-003-pg-vendor` |
| Work Unit | `WU-YYYYMM-<seq>` | `WU-202605-001` |
| 분석 리포트 | `ANL-YYYY-<seq>-<slug>` | `ANL-2026-001-market` |
| 캠페인 | `CMP-YYYY-<seq>-<slug>` | `CMP-2026-001-launch` |
| FAQ | `FAQ-<service>-vN` | `FAQ-payments-v1` |

### 2.1 시퀀스 운영
- `<seq>`는 해당 분류 내 일련번호. 충돌 회피를 위해 PR 머지 시점 기준으로 부여.
- 채번 주기:
  - **연 단위 리셋**: PRD/PLAN/DS/AISPEC/TP/ANL/CMP/Epic (`YYYY-<seq>`).
  - **월 단위 리셋**: WU/DEC/Iteration (`YYYYMM-<seq>`). 한 달치 워크가 한눈에 보이도록.
- 시퀀스 충돌이 잦으면 `docs/<type>/_INDEX.md`에 사용 중 ID 목록 유지.

### 2.2 ID 위계와 역참조 (필수)

산출물은 위계의 상위 ID를 메타데이터에 명시한다. 큰 그림을 잃지 않기 위함이다 (안티패턴 12).

```
Epic (EP-YYYY-NN)
  └─ Iteration (IT-YYYYMM-NN)         메타: epic
       └─ Work Unit (WU-YYYYMM-NN)    메타: epic, iteration
            └─ Deliverable             메타: work_unit
            └─ Decision Log (DEC-...)  본문 References에 EP/IT/WU 명시
```

규칙:
- **Iteration**은 `epic` 필드 필수.
- **WU**는 `epic`, `iteration` 필드 필수 (`shared/outputs.md` Work Unit 양식).
- **PRD/PLAN/DS/AISPEC/TP/Runbook 등 Deliverable**은 메타에 `work_unit` 필수. 어느 WU의 산출물인지 추적 가능해야 G5 QA 진입에서 막힘 방지.
- **DEC**는 본문 §6 References에 EP/IT/WU 명시. 결정의 맥락 손실 방지.

진입 거부 규칙(§5.2)에 다음을 추가한다:
- [ ] WU 메타에 `epic`/`iteration`이 없거나, 참조 ID가 실제 존재하지 않음 → 반려.

---

## 3. AC 추적 규칙 (가장 자주 깨지는 곳)

PRD의 AC가 기획서 → Design Spec → Test Plan을 거치며 변형되는 것을 막는다.

1. **AC ID 부여**: PRD에서 `AC-001`, `AC-002` ... 부여.
2. **하향 매핑 의무**: 기획서/Design Spec/Test Plan은 각 항목에 "관련 AC: AC-001, AC-003" 명시.
3. **QA 체크**: Test Plan에 PRD AC ID가 100% 포함됐는지 검증. 누락 시 G5(QA 진입) 반려.
4. **변경 시 절차**: AC 의미를 바꿔야 하면 PRD를 먼저 수정하고 Decision Log 작성. 하위 산출물은 그 PR을 따라간다.

---

## 4. Decision Log — 살아있는 지식 베이스의 척추

### 4.1 작성 트리거
다음 중 하나라도 해당하면 Decision Log를 작성한다.
- 비가역 작업 (L4)
- 범위 외 결정 (다른 포지션의 R&R 침범 또는 위임)
- 옵션이 2개 이상이고 골라야 함
- 외부 의존성 추가/변경 (PG사, 클라우드 리전, 모델 공급자 등)
- 보안/법적/개인정보 정책 결정

### 4.2 PR과의 연결
- 모든 PR description에 **관련 DEC ID** 필드를 둔다 (없으면 "N/A" 명시).
- DEC가 새로 필요하면 **PR과 같은 브랜치에서 함께 작성**한다 (별도 PR로 미루지 않는다).
- PR template 예시:
  ```
  ## Summary
  ## Related
  - PRD: PRD-2026-001
  - DEC: DEC-202605-003
  - WU:  WU-202605-007
  ```

### 4.3 검색
- `docs/decisions/` 안에서 키워드 grep. 또는 `docs/decisions/_INDEX.md`에 한 줄 요약 유지.

---

## 5. Work Unit 운영

### 5.1 시작
송신자가 `docs/handoffs/WU-YYYYMM-<seq>.md`를 만들어 Work Unit + 핸드오프 메시지를 함께 기록한다.

### 5.2 진입 거부 규칙 (수신 에이전트)
다음 중 하나라도 위반 시 진입 거부 + 송신자에게 반려:
- [ ] WU 파일이 없음 (채팅·구두 핸드오프 금지)
- [ ] Inputs 경로/버전이 비어있거나 존재하지 않음
- [ ] DoD가 측정 불가능 (모호한 형용사만 있음)
- [ ] 권한 등급이 작업과 불일치

### 5.3 종료
WU 파일 하단에 **종료 결과**(완료/지연/취소 + 산출물 ID + 다음 WU 링크)를 추가하고 status를 `closed`로 변경.

---

## 6. 검색·인덱싱

운영 1~2개월 후 산출물이 100건을 넘어가면 검색이 필요하다.

### 6.1 최소 인덱스
각 `docs/<type>/` 폴더에 `_INDEX.md`를 둔다.
```
# PRD Index
| ID | Slug | Status | Owner | Created |
|---|---|---|---|---|
| PRD-2026-001 | payments | approved | @hong | 2026-04-01 |
| PRD-2026-002 | refund   | draft    | @kim  | 2026-05-02 |
```

### 6.2 자동화 (선택)
- 사내 도구가 있으면 grep 기반 인덱스 생성 스크립트를 `.agents/index.sh` 등에 둔다.
- MCP 서버를 통해 LLM이 직접 검색하게 만들 수도 있다 (CLAUDE.md S10 "사내 도구 MCP").

---

## 7. 산출물 상태 운영

`status` 필드는 단방향으로만 진행한다: `draft → reviewed → approved`. 되돌릴 일이 생기면 새 버전을 만든다.

| 상태 | 의미 | 다음 포지션 인수 가능? |
|---|---|---|
| `draft` | AI 또는 사람이 작성 중 | 불가 |
| `reviewed` | 동료 리뷰 통과 | 조건부 (오너 승격 대기) |
| `approved` | 오너 승격 완료 | 가능 (게이트 통과) |

게이트(G1~G7) 통과 조건은 **수신 측 산출물이 `approved`** 일 때만 충족된다.

---

## 8. 사람-에이전트 책임 경계

### 8.1 사람만 하는 것
- L3/L4 승인 (CLAUDE.md S4)
- 산출물 `approved` 승격
- Decision Log의 최종 결정자 서명
- 에스컬레이션 수신·판단 (S7.3)

### 8.2 에이전트가 하는 것
- 초안 작성 (`draft`)
- 결정적 검사 (CI/린터/테스트)
- AI 리뷰 1차
- 산출물 양식 준수 검증

### 8.3 PR 리뷰 단계 (CLAUDE.md S2 원칙7)
1. 결정적 검사 (CI/린터/테스트) — 자동
2. AI 리뷰 (LLM judge) — 에이전트
3. 사람 리뷰 — 게이트

3단계가 모두 통과하지 않으면 머지 금지.

---

## 9. 운영 체크리스트 (월간)

- [ ] `docs/decisions/_INDEX.md`가 최신인가
- [ ] 모든 PR이 관련 DEC/PRD/WU ID를 명시했는가
- [ ] AC 매핑 누락된 산출물이 있는가
- [ ] 한 달간 에스컬레이션 트리거(S7.3)가 어디서 자주 발생했는가 → 워크플로 개선 신호
- [ ] 템플릿 버전이 최신인가 (`VERSIONING.md` 동기화 절차)
- [ ] 새로운 포지션/인원이 추가됐는가 (`TEAM-EVOLUTION.md` 절차)

---

## 관련 문서
- 도입 절차: [SETUP.md](./SETUP.md)
- 템플릿 갱신·동기화: [VERSIONING.md](./VERSIONING.md)
- 포지션·인원 변경: [TEAM-EVOLUTION.md](./TEAM-EVOLUTION.md)
