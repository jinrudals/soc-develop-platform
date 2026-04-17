# Milestone Start Workflow — Tables

## 개요

상위 Milestone의 release tag 생성 시 하위 Milestone branch를 자동으로 초기화하는 Workflow.
Project Service / Version Service 호출로 완결되며, 별도 실행 이력만 보관한다.

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| MilestoneStartRun | Milestone Start Workflow 실행 이력 |

---

## MilestoneStartRun

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| parent_milestone_id | FK → Milestone | 트리거가 된 상위 Milestone |
| child_milestone_id | FK → Milestone | 시작 대상 하위 Milestone |
| status | enum | IN_PROGRESS / COMPLETED / FAILED |
| triggered_at | timestamp | |
| completed_at | timestamp (nullable) | |
