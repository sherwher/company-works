# SETUP — 사내 AI 협업 규약 도입 가이드

본 문서는 신규 프로젝트가 본 템플릿을 자기 리포에 도입하는 절차를 정의한다.
**선택형 도입**: 자기 프로젝트에 필요한 포지션만 골라 가져간다.

---

## 0. 사전 결정

도입 전 다음 3가지를 결정한다.

1. **활성 포지션**: 어떤 포지션을 사용할지. 최소 PM/기획/FE/BE/QA, 보통 + DevOps. AI 기능 있으면 + AI 엔지니어. 사업전략/CS는 오프라인.
2. **권한 상한**: 프로젝트가 허용할 최대 권한 등급 (CLAUDE.md S4의 L0~L4).
3. **첫 워크플로**: 신규 개발 / 기능 추가 / 캠페인 / CS 세팅 중 어떤 것으로 시작할지.

---

## 1. 파일 복사

본 템플릿의 다음 구조를 신규 프로젝트 루트에 복사한다.

```
<new-project>/
├── CLAUDE.md                  ← templates/CLAUDE.md 복사 (공통 규약 진입점)
├── agents/                    ← 활성 포지션 파일만 선택 복사
│   ├── pm.md
│   ├── planner.md
│   └── ...                    ← 미사용 포지션은 복사하지 않는다
├── workflows/                 ← 활성 워크플로 파일만 선택 복사
│   └── full-cycle.md
└── shared/
    └── outputs.md             ← 항상 복사 (공통 산출물 양식)
```

선택 복사 예시 (Full Cycle, AI 기능 없음):
```
cp <template>/CLAUDE.md <new-project>/CLAUDE.md
cp <template>/shared/outputs.md <new-project>/shared/outputs.md
cp <template>/agents/{pm,planner,designer,frontend,backend,qa,devops}.md <new-project>/agents/
cp <template>/workflows/full-cycle.md <new-project>/workflows/
```

---

## 2. 컨텍스트 작성

신규 프로젝트의 `CLAUDE.md` S1 빈칸을 채운다.

- Project / Owner / 목표 / 범위 / 활성 포지션 / 활성 워크플로 / 권한 상한

---

## 3. 카탈로그 정리

`CLAUDE.md` S3 포지션 카탈로그에서 **사용하지 않는 포지션 행을 삭제**한다. 카탈로그가 실제 활성 포지션과 일치해야 에이전트가 잘못된 핸드오프 대상을 호출하지 않는다.

S6 워크플로 표도 동일하게 정리한다.

---

## 4. 도구별 진입점 매핑

각 에이전트는 `agents/<role>.md`가 정본이다. 도구별 진입점은 사본/링크로 만든다.

| 도구 | 진입점 파일 | 권장 방식 |
|---|---|---|
| Claude Code | 작업 디렉터리의 `CLAUDE.md` | 루트 `CLAUDE.md`(공통 규약) + 포지션별 작업 시 해당 `agents/<role>.md`를 컨텍스트로 주입 |
| Cursor | `.cursor/rules/<role>.mdc` | `agents/<role>.md` 내용을 mdc로 변환하여 배치 |
| AGENTS.md 호환 도구 | 작업 디렉터리의 `AGENTS.md` | `CLAUDE.md`를 심볼릭 링크 또는 사본으로 배치 |
| 자체 에이전트 시스템 | 시스템 프롬프트 | `CLAUDE.md` S8 골격 + `agents/<role>.md` 본문을 주입 |

### 4.1 Claude Code 사용 시

- 프로젝트 루트 `CLAUDE.md`는 공통 규약(본 템플릿).
- 특정 포지션으로 작업할 때는 사람이 명시적으로 "지금 PM 모드. `agents/pm.md`를 따라 작업" 같이 지시하거나, 서브에이전트/슬래시 커맨드를 정의해 자동 주입한다.

### 4.2 AGENTS.md 도구

AGENTS.md를 사용하는 도구는 `ln -s CLAUDE.md AGENTS.md` 또는 사본으로 운영. 두 파일이 다른 내용을 갖지 않도록 한다.

---

## 5. 시스템 프롬프트 세팅

각 활성 포지션마다:

1. `CLAUDE.md` S8 공통 골격을 베이스로 사용
2. `agents/<role>.md`의 Identity / R&R / 진입·이탈 게이트 / 추가 규칙을 주입
3. `CLAUDE.md` S1 프로젝트 컨텍스트를 [Project Context] 슬롯에 삽입

---

## 6. 도입 검증 체크리스트

- [ ] CLAUDE.md S1 빈칸 모두 작성
- [ ] 활성 포지션 카탈로그(S3) 정리 완료
- [ ] 활성 워크플로 카탈로그(S6) 정리 완료
- [ ] 활성 포지션의 `agents/<role>.md` 모두 복사
- [ ] 활성 워크플로의 `workflows/<flow>.md` 모두 복사
- [ ] `shared/outputs.md` 복사
- [ ] 권한 상한 결정 + 각 에이전트 부팅에 반영
- [ ] 에이전트별 시스템 프롬프트 세팅
- [ ] 사람 에스컬레이션 경로 명시
- [ ] 관측성 연결 (trace 저장)
- [ ] 사내 도구 MCP 서버 식별

---

## 7. 운영 후 갱신

도입 이후의 운영·갱신·변화 관리는 다음 문서로 분리된다.

- **운영 구조** (폴더 레이아웃, ID 규칙, AC 추적, Decision Log): [`OPERATIONS.md`](./OPERATIONS.md)
- **템플릿 갱신과 프로젝트 동기화** (시맨틱 버전, CHANGELOG, 오버라이드): [`VERSIONING.md`](./VERSIONING.md)
- **팀·포지션·에이전트 변화 관리** (사람 합류/이탈, 포지션 추가, 에이전트 병렬화): [`TEAM-EVOLUTION.md`](./TEAM-EVOLUTION.md)

요약:
- 새 포지션 추가: `agents/<new-role>.md` 작성 + CLAUDE.md S3 카탈로그 업데이트 (TEAM-EVOLUTION S3)
- 새 워크플로 추가: `workflows/<new-flow>.md` 작성 + CLAUDE.md S6 카탈로그 업데이트 (TEAM-EVOLUTION S7)
- 산출물 양식 변경: 포지션별이면 해당 `agents/<role>.md`, 공통이면 `shared/outputs.md`
- 원칙 변경: 메타 리포 PR (VERSIONING S6)
