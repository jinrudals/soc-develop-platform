# RTL Quality Check Service — Tables

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| Criterion | 수행해야 할 검사 항목 정의 |
| CriterionRelation | Criterion 간 순서 및 의존성 관계 |
| CriterionTarget | RTL Module + Criterion 매핑 |
| CrossDependencyRule | 하위 Module의 Criterion 완료 후 실행 조건 |
| QualityPolicy | Project + MilestoneLevel + CriterionTarget 조합의 PASS 판단 기준 |
| Execution | CriterionTarget 실행 단위 |
| ExecutionBatch | 여러 Execution을 묶어 수행하는 단위 |
| ExecutionBatchItem | ExecutionBatch ↔ Execution 매핑 |

---

## Criterion

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: COMPILE, RTL_LINT, RTL_CDC, RTL_RDC, SYNTHESIZABLE |
| description | string | |
| clean | JSON | cleanup 스크립트/커맨드 |
| run | JSON | 실행 스크립트/커맨드 |
| post | JSON | 후처리 스크립트/커맨드 |

| Unique | `name` |
|---|---|

---

## CriterionRelation

Criterion 간 순서(AFTER / BEFORE)와 실제 데이터 의존성(REQUIRES)을 구분한다.
- `AFTER` / `BEFORE`: 실행 순서만 정의
- `REQUIRES`: 상대 Criterion의 output이 있어야 실행 가능 (데이터 의존성)

| 필드 | 타입 | 설명 |
|---|---|---|
| criterion_id | FK → Criterion | |
| related_criterion_id | FK → Criterion | |
| relation_type | enum | AFTER / BEFORE / REQUIRES |

| Unique | `(criterion_id, related_criterion_id, relation_type)` |
|---|---|

---

## CriterionTarget

RTL Module과 Criterion의 M:N 매핑. Project 스코프 없이 모듈 자체의 속성으로 정의된다.
Project 기준 조회 시 `Project → ProjectRepository → Repository → RTLModule → CriterionTarget` 경로로 join한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | |
| criterion_id | FK → Criterion | |

| Unique | `(rtl_module_id, criterion_id)` |
|---|---|

---

## CrossDependencyRule

특정 CriterionTarget을 실행하기 전에, 하위 Module들의 특정 Criterion이 먼저 완료되어야 하는 규칙.
하위 Module 목록은 RTLHierarchy에서 동적으로 조회한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| criterion_target_id | FK → CriterionTarget | 이 CriterionTarget을 실행하려면 |
| prerequisite_criterion_id | FK → Criterion | 하위 Module들의 이 Criterion이 먼저 완료되어야 함 |

| Unique | `(criterion_target_id, prerequisite_criterion_id)` |
|---|---|

---

## QualityPolicy

Project + MilestoneLevel + CriterionTarget 조합에 대해 PL이 정의한 PASS 판단 기준.
같은 Criterion이라도 Project / MilestoneLevel에 따라 기준이 달라질 수 있다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| project_id | FK → Project | |
| milestone_level_id | FK → MilestoneLevel | ML1 / ML2 / ML3 |
| criterion_target_id | FK → CriterionTarget | 대상 Module + Criterion 조합 |
| criteria | JSON | PASS 판정 조건. 예: `{"critical": 0, "warning": 10}` |
| updated_at | timestamp | |

| Unique | `(project_id, milestone_level_id, criterion_target_id)` |
|---|---|

---

## Execution

CriterionTarget을 특정 버전(tag)으로 실제 실행하는 단위.
`output_path`를 저장하여 ExecutionBatch에서 재활용할 수 있다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| criterion_target_id | FK → CriterionTarget | |
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
