# Company Works — AI Agent Collaboration Framework

회사 내부에서 AI 에이전트를 활용해 팀/포지션 간 협업을 표준화하기 위한 **프레임워크 및 템플릿** 저장소입니다. 각 포지션의 R&R(Role & Responsibility)을 명문화하고, 에이전트가 따라야 할 협업 규약과 산출물 템플릿을 제공해 어떤 프로젝트에든 빠르게 도입할 수 있도록 합니다.

## 목적

- **표준화**: 사람-에이전트, 에이전트-에이전트 간 협업을 일관된 규약으로 운영
- **재사용성**: 신규 프로젝트에 그대로 복사해 사용할 수 있는 템플릿 제공
- **추적성**: 의사결정·핸드오프·산출물의 이력을 구조화된 형태로 남김
- **품질 보증**: 포지션별 R&R과 Definition of Done(DoD)으로 산출물 품질 균질화

## 저장소 구조

```
.
├── README.md                       # 본 문서 — 프레임워크 진입점
├── docs/
│   ├── framework-overview.md       # 프레임워크 전반 개념 및 운영 모델
│   ├── collaboration-protocol.md   # 에이전트 간 협업 규약(메시지/핸드오프/에스컬레이션)
│   ├── governance.md               # 권한·보안·감사·검토 정책
│   └── glossary.md                 # 공통 용어 정의
├── rnr/                            # 포지션별 R&R 명문화
│   ├── README.md
│   ├── product-manager.md
│   ├── product-designer.md
│   ├── frontend-engineer.md
│   ├── backend-engineer.md
│   ├── qa-engineer.md
│   ├── devops-engineer.md
│   ├── data-analyst.md
│   └── tech-lead.md
├── workflows/                      # 업무 흐름 템플릿
│   ├── project-kickoff.md
│   ├── feature-development.md
│   ├── incident-response.md
│   └── retrospective.md
├── templates/                      # 산출물 템플릿
│   ├── prd.md
│   ├── design-spec.md
│   ├── tech-spec.md
│   ├── api-contract.md
│   ├── test-plan.md
│   ├── decision-log.md
│   ├── handoff.md
│   └── postmortem.md
└── agents/                         # 에이전트 설정 가이드
    ├── README.md
    ├── agent-charter-template.md
    └── prompt-conventions.md
```

## 빠른 시작

1. 이 저장소를 새 프로젝트의 `docs/` 또는 `.agents/`로 복사하거나 서브모듈로 추가합니다.
2. `agents/agent-charter-template.md`를 복사해 프로젝트별 에이전트 헌장(Charter)을 작성합니다.
3. `workflows/project-kickoff.md`를 따라 PRD → 디자인 스펙 → 기술 스펙 순으로 산출물을 채워나갑니다.
4. `docs/collaboration-protocol.md`의 핸드오프 규약에 맞춰 에이전트 간 작업을 위임합니다.

## 핵심 원칙

1. **명문화 우선**: 모든 R&R, 의사결정, 핸드오프는 문서로 남긴다.
2. **단일 책임**: 한 에이전트는 한 포지션의 R&R만 수행한다.
3. **검증 가능한 산출물**: 모든 산출물은 DoD를 만족해야 한다.
4. **사람의 최종 결정권**: 비가역적 결정·외부 영향 작업은 사람의 승인을 받는다.
5. **추적성**: 모든 산출물은 출처(prompt, 입력 문서, 결정자)를 기록한다.

## 라이선스 / 사용 범위

사내 전용. 외부 공개 시 보안·법무팀 검토 필요.
