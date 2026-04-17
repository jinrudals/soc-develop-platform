# Job Runner Service — Tables

## 개요

Execution Domain의 각 Service로부터 실행 요청을 수신하고, Jenkins / Airflow job을 호출하며,
실행 상태와 raw 결과를 요청 Service에 반환한다.

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| JobRequest | Execution Service로부터 수신한 실행 요청 이력 |
| JobExecution | 실제 Jenkins / Airflow job 실행 단위 |

---

## JobRequest

Execution Service가 Job Runner에게 전달한 실행 요청을 기록한다.
요청 Service와 해당 Execution ID를 함께 저장하여 결과 콜백 대상을 식별한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| source_service | string | 요청 Service 식별자 (예: dft-insertion, pi-synthesis) |
| source_execution_id | bigint | 요청 Service의 Execution ID (콜백 대상 식별용) |
| job_type | string | 실행할 Jenkins job 이름 또는 Airflow DAG ID |
| payload | JSON | job 실행에 필요한 파라미터 |
| created_at | timestamp | |

---

## JobExecution

JobRequest 1건당 1개 이상의 JobExecution이 생성될 수 있다.
(인프라 오류로 인한 재시도 시 새 row 생성)

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| job_request_id | FK → JobRequest | 요청 출처 |
| runner_type | enum | JENKINS / AIRFLOW |
| external_id | string | Jenkins build number 또는 Airflow run ID |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| raw_result | JSON (nullable) | 완료 시 수신한 raw 결과 (log 경로, 수치 등) |
| started_at | timestamp (nullable) | |
| finished_at | timestamp (nullable) | |
