# PD After P&R Service — Tables

## 개요

PD P&R이 생성한 Layout을 입력으로 받아 Timing / DRC / LVS 등의 검사를 수행한다.
HPDF 단위로 개별 실행된다.

## 트리거 조건

| 조건 | 설명 |
|---|---|
| PD P&R Execution 완료 (SUCCESS) | Layout이 정상 생성된 경우 |

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| CheckItem | 검사 항목 정의 |
| QualityPolicy | Project + MilestoneLevel + RTLModule + CheckItem 조합의 PASS 판단 기준 |
| Execution | PD After P&R 실행 단위 |

---

## CheckItem

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: TIMING, DRC, LVS |
| description | string | |

| Unique | `name` |
|---|---|

---

## QualityPolicy

Project + MilestoneLevel + RTLModule + CheckItem 조합에 대해 PL이 정의한 PASS 판단 기준.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| project_id | FK → Project | |
| milestone_level_id | FK → MilestoneLevel | ML1 / ML2 / ML3 |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| check_item_id | FK → CheckItem | |
| criteria | JSON | PASS 판정 조건. 예: `{"drc_violations": 0, "lvs_mismatch": 0}` |
| updated_at | timestamp | |

| Unique | `(project_id, milestone_level_id, rtl_module_id, check_item_id)` |
|---|---|

---

## Execution

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| pd_pr_execution_id | FK → PD P&R Execution | 검사 대상 Layout의 출처 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | PASS / FAIL (nullable) |
| output_path | string | 검사 결과 log/report 경로 |
| executed_at | timestamp | |
