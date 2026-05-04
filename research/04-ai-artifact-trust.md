# 04. AI 산출물 신뢰 등급과 Dev-ready 게이트

본 노트는 `templates/shared/outputs.md`의 "AI 산출물 신뢰 등급(L1/L2/L3)", "Dev-ready 체크리스트", "SSOT 섹션"과 `templates/workflows/dev-ready-review.md`의 30분 회의 구조의 출처 근거를 정리한다.

## 1. 문제 진단

### 1.1 관찰된 갈등 (2026-05)

기획팀(PM/기획/디자인)과 개발팀이 AI 에이전트 기반 협업에서 충돌:

- **기획팀**: AI로 거의 퍼블리싱된 HTML 산출물을 만들어 전달. "다 만들어 줬는데 왜 또 리뷰? 왜 개발이 빨리 안 나오나?"
- **개발팀**: 받은 산출물이 부분적, 데이터 모델/도메인 검토 부족, AI raw output, 작성자 본인도 산출물 전체를 따라가지 못함. "이 상태로 개발 들어가면 품질 저하 확정."

### 1.2 본질 (advisor 합의)

두 advisor(Codex, Gemini)가 독립적으로 동일 결론에 수렴:

- **Codex (구조적 진단)**: HTML은 `UX artifact`이지 `System artifact`가 아니다. 그 사이의 변환 레이어(도메인/데이터/API)가 비어 있다. 핵심 결손은 ① 정보/데이터 모델 누락, ② 산출물 정의 부재, ③ 검증 게이트 부재. 시스템 관점에서 가장 먼저 고쳐야 할 것은 **산출물 정의 부재**.
- **Gemini (심리·협업 진단)**: "완성도의 착시(Illusion of Completeness)" + "검증되지 않은 자동화" → 작성자의 사고 과정을 개발자가 역추적(Reverse Engineering)하는 상태. 책임 경계의 모호함, 오너십 부재.

공통 결론: **"리뷰가 더 필요"한 게 아니라 리뷰의 이름·목적·통과 기준을 명확히 한 변환 시스템이 필요하다.**

핵심 명제:
> AI가 만든 HTML은 코드가 아니라 고해상도 와이어프레임이다. 개발 착수 조건은 "HTML 완료"가 아니라 "시스템 명세 승인"이다.

## 2. 적용된 원칙

### 2.1 신뢰 등급 (L1/L2/L3)

두 advisor가 독립 제안한 라벨 체계를 통합:

| 통합 라벨 | Codex 라벨 | Gemini 라벨 |
|---|---|---|
| L1 (Draft) | `AI-generated` | Level 1 (Draft) |
| L2 (Verified Logic) | `PM-reviewed` + `Design-reviewed` | Level 2 (Verified Logic) |
| L3 (Dev-ready) | `Domain-reviewed` + `Engineering-reviewed` = `Dev-ready` | Level 3 (Dev-Ready) |

→ `templates/shared/outputs.md` §AI 산출물 신뢰 등급에 박제.

### 2.2 SSOT 6섹션

Gemini 제안 그대로 채택. AI의 결과(Visual Prototype)와 작성자의 사고(Data Requirements / Business Logic / Prompt Intent)를 **분리**하여 한 문서에 둔다.

→ `templates/shared/outputs.md` §SSOT 섹션.

### 2.3 Dev-ready 체크리스트 (13항목)

Codex 제안 그대로 채택. 개발 착수 게이트의 통과 기준을 객관화.

→ `templates/shared/outputs.md` §Dev-ready 체크리스트.

### 2.4 30분 회의

Codex의 5분 단위 시간 분배 + Gemini의 "데이터 흐름 리뷰(화이트보드)" 권고를 통합. 4명 이상은 부르지 않는다는 제약은 의사결정 속도 보존.

→ `templates/workflows/dev-ready-review.md`.

### 2.5 오너십 강제 — 역추적 설명 의무

Gemini의 "AI가 짜줬어요 금지" + "Prompt Intent 첨부" 권고를 채택. 작성자가 비즈니스 로직 관점에서 산출물을 설명하지 못하면 핸드오프 반려.

→ `templates/shared/outputs.md` §AI 산출물 신뢰 등급.

### 2.6 Atomic Planning

Gemini 제안. AI에 한 번에 큰 화면을 그리게 하면 작성자의 인지 한계를 넘어가 "내가 모르는 코드"가 산출물에 섞임. 기능 단위로 쪼개 호출.

→ `templates/shared/outputs.md` §AI 산출물 신뢰 등급.

## 3. 기존 원칙과의 관계

본 노트는 `00-synthesis.md`의 12원칙 중 다음을 강화·구체화한다:

- **원칙 4 (Draft-by-default + Citation)** → `trust_level` 등급으로 구체화. "draft"가 단일 상태가 아니라 L1/L2/L3로 분화.
- **원칙 5 (3단계 승격 게이트)** → Dev-ready 체크리스트가 L2→L3 승격의 객관 기준 제공.
- **원칙 6 (산출물 형식 표준화)** → SSOT 6섹션이 UI/HTML 산출물의 표준 양식.
- **원칙 7 (결정적 검사 → AI 리뷰 → 사람 리뷰)** → 30분 Dev-ready Review가 사람 리뷰 단계의 표준화된 형식.

## 4. 의도적으로 채택하지 않은 권고

- **신규 직책 (Product Architect / Technical PM)**: Codex가 권고. 본 템플릿은 직책이 아닌 **책임**으로 정의(`agents/<role>.md`). 변환 책임은 [`agents/pm.md`](../templates/agents/pm.md)와 [`agents/backend.md`](../templates/agents/backend.md)의 R&R로 분담하고, Dev-ready Review를 통해 협업으로 흡수.
- **`AI-generated` / `Assumption-heavy` / `Blocked` 같은 다중 라벨**: Codex가 권고. 라벨이 많아지면 신호가 분산되어 무력화 위험. 3등급(L1/L2/L3) + `coverage` / `known_missing` / `assumptions` 메타필드로 충분.

## 5. 출처

- 자문 산출물:
  - `.omc/artifacts/ask/codex-ai-pm-ai-html-html-db-raw-output-1-2-ai-3-html-api-4-ai-5-2026-05-04T08-18-54-606Z.md`
  - `.omc/artifacts/ask/gemini-ux-pm-ai-ai-html-html-ai-raw-1-ai-2-ai-ritual-3-ai-ownership-2026-05-04T08-18-17-502Z.md`
- 두 자문은 `/oh-my-claudecode:ccg`로 동일 문제를 시스템 관점(Codex)과 협업/UX 관점(Gemini)으로 병렬 자문한 결과.

## 6. 후속 검토 항목

- 6주 운영 후 L2→L3 승격률, "Not ready" 비율, 회의 평균 시간 측정 → 게이트의 실효성 검증.
- 작성자별 "AI가 짜줬어요" 발생 빈도 추적 → Atomic Planning 가이드 보강 필요 여부 판단.
- AI 엔지니어 산출물(모델/프롬프트/데이터)에 동일 등급 체계 적용 시 추가 메타데이터 필요한지 검토.
