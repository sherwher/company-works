# Workflow — Incident Response

운영 장애 발생 시의 표준 흐름.

## 단계

1. **Detect**: 알림/제보 수신 → Incident ID 발급(`INC-YYYYMMDD-<seq>`)
2. **Triage**: Severity 판정 (S1~S4), Incident Commander 지정
3. **Mitigate**: 영향 축소(롤백·차단·우회). 근본 원인 수정보다 우선
4. **Communicate**: 상태 페이지·내부 채널에 정기 업데이트
5. **Resolve**: 정상 상태 확인 → 인시던트 종료
6. **Postmortem**: `templates/postmortem.md`로 24~72h 내 작성

## Severity 가이드

| Sev | 정의 | 응답 시간 |
|---|---|---|
| S1 | 핵심 기능 전면 중단 | 즉시(15분) |
| S2 | 핵심 기능 일부 중단 또는 다수 사용자 영향 | 1시간 |
| S3 | 비핵심 기능 영향 | 영업일 내 |
| S4 | 사용자 영향 미미 | 다음 스프린트 |

## 역할

- **Incident Commander**: 의사결정·커뮤니케이션 총괄(주로 TL/DevOps)
- **Operator**: 실제 완화 조치 수행(Engineer/DevOps)
- **Communicator**: 상태 업데이트(PM)
- **Scribe**: 타임라인 기록(아무나)

## 에이전트 행동 규칙
- S1/S2 인시던트는 자동 조치 금지 — 사람 승인 후 수행
- 모든 명령은 타임라인에 기록
- 가설은 검증 전에 단언하지 않는다
