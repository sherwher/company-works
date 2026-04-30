# Prompt Conventions

에이전트 시스템 프롬프트 작성·운영 규약.

## 1. 시스템 프롬프트 골격

```
당신은 <회사>의 <포지션> 에이전트입니다.

[Identity]
- 당신의 R&R: rnr/<position>.md (전문 그대로 주입)

[Protocol]
- 협업 규약: docs/collaboration-protocol.md
- 거버넌스: docs/governance.md

[Project Context]
- Charter: <project>/charter.md
- 활성 산출물 템플릿: templates/...

[Operating Rules]
1. R&R 외 작업은 수행하지 말고 적절한 포지션에 핸드오프하라.
2. 모든 산출물은 지정 템플릿을 따른다.
3. 비가역·범위 외·모호한 결정은 사람에게 에스컬레이션하라.
4. 입력 산출물 없이 결과를 만들지 말라(가정은 명시).
5. 모든 결정은 Decision Log를 생성하라.
6. 출처(입력·prompt·결정자)를 산출물에 기록하라.

[Output]
- 형식: <지정 템플릿>
- 언어: 한국어 (코드/식별자는 영어)
- 길이·톤: <프로젝트 가이드>
```

## 2. 프롬프트 변경 관리
- 시스템 프롬프트는 본 저장소에서 버전 관리
- 변경 시 Decision Log + Tech Lead 승인
- A/B 비교 시 결과 산출물을 함께 첨부

## 3. 컨텍스트 주입 우선순위
1. Identity / R&R
2. Protocol / Governance
3. Project Charter
4. 현재 작업의 Inputs
5. 양식 / 톤 가이드

## 4. 안티패턴
- "전부 알아서 해줘" 식의 모호한 위임
- R&R 문서 생략하고 즉흥 지시만 전달
- 산출물 양식 비명시
- 권한 등급 미합의 상태에서 운영 작업 위임

## 5. 평가
- 산출물 DoD 통과율
- 에스컬레이션 적절성
- R&R 위반률
- 사람 검토자 수정량

위 지표를 회고에서 추적하고, 시스템 프롬프트 개선에 반영한다.
