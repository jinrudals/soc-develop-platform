# Job Runner Domain

## 개요

Execution Domain의 각 Service로부터 실행 요청을 받아 Jenkins(또는 Airflow 등) job을 호출하고,
실행 상태와 결과를 요청한 Service에 돌려주는 Domain이다.

모든 Execution Service가 Jenkins를 직접 호출하면 연결성이 복잡해지므로,
**Jenkins 호출을 단일 지점으로 집중**하기 위해 독립 Domain으로 분리한다.

## 핵심 개념

| 개념 | 설명 |
|---|---|
| Job Request | Execution Service로부터 전달받은 실행 요청. 실행할 job 종류, 파라미터, 요청 출처 Service 정보를 포함한다. |
| Job Execution | Job Request에 의해 생성된 Jenkins(또는 Airflow) job 실행 단위. |
| Execution Status | `PENDING` / `RUNNING` / `COMPLETED` / `FAILED` |
| Execution Result | job이 완료된 후 반환되는 raw 결과 (log, 수치 데이터 등). 판단(PASS/FAIL)은 요청한 Execution Service가 담당한다. |

## 처리 흐름

```
Execution Service  →  Job Request 전달
                   ←  Job Execution ID 반환

Job Runner  →  Jenkins / Airflow job 호출
           ←  상태 변경 수신 (PENDING → RUNNING → COMPLETED)
           →  Execution Service에 상태 전달
           →  완료 시 Execution Service에 raw 결과 전달
```

## 연관 Domain

| Domain | 관계 |
|---|---|
| Execution Domain | Job Request를 수신하고, 실행 상태 및 결과를 반환한다. |
