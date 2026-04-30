# API Contract — <Resource/Feature>

- **WU ID**:
- **Owner**: Backend
- **Consumers**: Frontend, ...
- **Version**: vN
- **Status**: Draft | Agreed | Implemented

## 1. Endpoints
| Method | Path | Auth | Idempotent |
|---|---|---|---|
| GET | /api/... | Bearer | Yes |

## 2. Request / Response Schemas
각 엔드포인트별로 다음을 명시.

```yaml
GET /api/resource/{id}
request:
  path:
    id: string (uuid)
response:
  200:
    schema:
      id: string
      name: string
      created_at: ISO8601
  404: { code: NOT_FOUND }
errors:
  - code: INVALID_INPUT
  - code: UNAUTHORIZED
```

## 3. Pagination / Filtering / Sorting
- 규약:

## 4. Error Model
공통 에러 포맷, 에러 코드 표.

## 5. Rate Limit / Quotas

## 6. Backward Compatibility
- Breaking 여부, 마이그레이션 계획

## 7. Examples
- 성공/실패 cURL 예

## 8. DoD
- [ ] FE/BE 합의
- [ ] 스키마 검증 가능(예: OpenAPI)
- [ ] 에러 코드·페이지네이션 명시
- [ ] 변경 시 버전 정책 준수
