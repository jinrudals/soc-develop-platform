# Platform Service — Tables

## 개요

인증은 Keycloak이 담당하며, Role / Permission 관리도 Keycloak에서 수행한다.
플랫폼은 JWT token을 통해 사용자 정보와 역할을 읽어 처리한다.
Platform Service DB에는 플랫폼 사용자 식별 정보만 저장한다.

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| PlatformUser | 플랫폼 사용자 계정 |

---

## PlatformUser

Keycloak 최초 로그인 시 생성된다. 이후 `keycloak_id`로 식별한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| keycloak_id | string | Keycloak user UUID (sub claim) |
| email | string | |
| name | string | |
| attributes | JSONB | 확장용 |
| created_at | timestamp | |

| Unique | `keycloak_id`, `email` |
|---|---|
