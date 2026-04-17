# RTL Integration Service — Tables

## 개요

Engineer가 작성한 conf를 기반으로 RTL을 자동 생성한다.
RTL 생성 tool(Chisel3 / RocketChip 등) 연동이 필요하며 현재 미구현 상태이다.

## 트리거 조건

| 조건 | 설명 |
|---|---|
| Engineer 수동 실행 요청 | conf 작성 또는 변경 후 Engineer가 직접 요청 |

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| IntegrationConf | RTLModule별 RTL 생성 설정 (immutable 버전 관리) |
| Execution | RTL Integration 실행 단위 |

---

## IntegrationConf

conf 변경 시 새 row를 생성하는 immutable 구조.
conf 파일은 별도의 conf repository에서 관리되며, 특정 tag를 참조한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 RTL Module |
| tag_id | FK → RepositoryTag | conf가 위치한 repository의 특정 tag |
| created_at | timestamp | |

---

## Execution

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 RTL Module |
| integration_conf_id | FK → IntegrationConf | 사용된 conf 버전 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | SUCCESS / FAILED (nullable) |
| output_tag_id | FK → RepositoryTag (nullable) | 생성된 RTL tag |
| executed_at | timestamp | |
