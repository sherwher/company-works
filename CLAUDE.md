# CLAUDE.md - 본 리포 운영 지침

본 리포는 **사내 AI 에이전트 협업 규약 템플릿**을 관리하는 메타 리포입니다.
이 파일은 Claude Code가 본 리포에서 작업할 때 따라야 할 지침이며, 다른 프로젝트로 복사되는 산출물이 아닙니다.

## 구조

- [`templates/CLAUDE.md`](./templates/CLAUDE.md) — 배포용 협업 규약 템플릿. 신규 프로젝트는 이 파일을 자기 루트에 `CLAUDE.md`로 복사해서 사용한다.
- [`research/`](./research/) — 템플릿 원칙의 출처 리서치 노트. `00-synthesis.md`가 통합 문서.
- `.omc/` — 로컬 작업 상태 (커밋 대상 아님).

## 작업 규칙

1. **두 역할을 섞지 않는다.** 본 파일(루트 CLAUDE.md)은 리포 운영용이고, `templates/CLAUDE.md`는 다른 프로젝트로 배포되는 산출물이다. 협업 규약 본문 수정은 항상 `templates/CLAUDE.md`에서 한다.
2. **템플릿 수정 시 근거 동기화.** `templates/CLAUDE.md`의 원칙/안티패턴/출처를 바꾸면 `research/`의 해당 노트와 일치 여부를 확인한다. 근거 없는 원칙은 추가하지 않는다.
3. **템플릿 내부 경로는 사본 기준.** `templates/CLAUDE.md` 안의 상대 경로(`research/...` 등)는 "템플릿이 복사된 신규 프로젝트의 루트"를 가정한다. 본 리포 기준으로 보면 한 단계 위(`../research/...`)임을 인지하고 편집한다.
4. **Conventional Commits.** `feat/fix/refactor/docs/research` 등. PR은 한 변경 단위, 500줄 이내.
5. **언어.** 문서/커밋 메시지는 한국어, 식별자는 영어.

## 도입(다른 프로젝트에서)

```
cp <this-repo>/templates/CLAUDE.md <new-project>/CLAUDE.md
# 신규 프로젝트에서 S1 컨텍스트 작성 후 사용
```
