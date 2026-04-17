# Verification Service — Tables

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| VerificationScope | 검증 수행 단위 (IP / HPDF / SoC) |
| VerificationType | 검증 유형 (Functional / Power / Performance) |
| VerificationTarget | RTL Module + Scope + Type 조합 |
| TestCommand | 실제 실행 커맨드 (compile 옵션, simv 옵션 등) |
| QualityPolicy | Project + MilestoneLevel + VerificationTarget 조합의 PASS 판단 기준 |
| Execution | TestCommand 실행 단위 |
| ExecutionBatch | 여러 Execution을 묶어 수행하는 단위 |
| ExecutionBatchItem | ExecutionBatch ↔ Execution 매핑 |

---

## VerificationScope

Scope마다 대상 RTL Module과 트리거 시점이 다르다.

| Scope | 대상 | 트리거 |
|---|---|---|
| IP | RTLModuleType = IP인 Module | stage tag 생성 시 |
| HPDF | RTLModuleType = HPDF인 Module | stage tag 생성 시 |
| SOC | SoC Top (Functional + DFTed RTL 모두) | RTL release 시 |

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: IP, HPDF, SOC |
| description | string | |

| Unique | `name` |
|---|---|

---

## VerificationType

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: FUNCTIONAL, POWER, PERFORMANCE |
| description | string | |

| Unique | `name` |
|---|---|

---

## VerificationTarget

RTL Module + Scope + Type 조합. Project 스코프 없이 모듈 자체의 속성으로 정의된다.
Project 기준 조회 시 `Project → ProjectRepository → Repository → RTLModule → VerificationTarget` 경로로 join한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | |
| scope_id | FK → VerificationScope | |
| type_id | FK → VerificationType | |

| Unique | `(rtl_module_id, scope_id, type_id)` |
|---|---|

---

## TestCommand

VerificationTarget에 속하는 실제 실행 커맨드 정의. 하나의 VerificationTarget에 여러 TestCommand가 있을 수 있다.
다른 VerificationTarget으로 복사 가능하다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| verification_target_id | FK → VerificationTarget | |
| name | string | |
| description | string | |
| compile_options | JSON | 컴파일 옵션/플래그 |
| sim_options | JSON | simv 실행 옵션 |

---

## QualityPolicy

Project + MilestoneLevel + VerificationTarget 조합에 대해 PL이 정의한 PASS 판단 기준.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| project_id | FK → Project | |
| milestone_level_id | FK → MilestoneLevel | ML1 / ML2 / ML3 |
| verification_target_id | FK → VerificationTarget | 대상 Module + Scope + Type 조합 |
| criteria | JSON | PASS 판정 조건. 예: `{"pass_rate": 95, "coverage": 90}` |
| updated_at | timestamp | |

| Unique | `(project_id, milestone_level_id, verification_target_id)` |
|---|---|

---

## Execution

TestCommand를 특정 버전(tag)으로 실행하는 단위.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| test_command_id | FK → TestCommand | |
| tag_id | FK → RepositoryTag | 실행 대상 버전 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | PASS / FAIL (nullable) |
| output_path | string | 결과물 경로. 재활용 시 참조 |
| executed_at | timestamp | |

---

## ExecutionBatch

여러 Execution을 묶어 수행하는 단위.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| description | string | |
| created_at | timestamp | |

---

## ExecutionBatchItem

ExecutionBatch에 포함된 Execution 목록.
`is_reused`가 true이면 새로 실행하지 않고 기존 Execution의 `output_path`를 참조한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| batch_id | FK → ExecutionBatch | |
| execution_id | FK → Execution | |
| is_reused | boolean | 이전 실행 결과 재활용 여부 |

| Unique | `(batch_id, execution_id)` |
|---|---|
