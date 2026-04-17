# Job Runner Service

## 개요

Execution Domain의 각 Service로부터 실행 요청을 받아 Jenkins(또는 Airflow 등) job을 호출하고,
실행 상태와 raw 결과를 요청한 Service에 돌려주는 Service이다.

모든 Execution Service가 Jenkins를 직접 호출하면 연결성이 복잡해지므로,
**Jenkins 호출을 단일 지점으로 집중**하기 위해 독립 Service로 분리한다.

**업데이트 주체: 시스템** (Execution Service의 요청에 의해 자동으로 동작)

## 처리 흐름

```
Execution Service  →  Job Request 전달
                   ←  Job Execution ID 반환

Job Runner  →  Jenkins / Airflow job 호출
           ←  상태 변경 수신 (PENDING → RUNNING → COMPLETED / FAILED)
           →  Execution Service에 상태 전달
           →  완료 시 Execution Service에 raw 결과 전달
```

## 관리 대상

| 대상 | 설명 |
|---|---|
| Job Request | Execution Service로부터 수신한 실행 요청 이력 |
| Job Execution | Jenkins / Airflow job 실행 단위. 상태 및 결과를 포함한다. |

## Job Execution 상태

| 상태 | 설명 |
|---|---|
| `PENDING` | 실행 요청이 접수되어 대기 중 |
| `RUNNING` | Jenkins / Airflow job이 실행 중 |
| `COMPLETED` | 정상적으로 완료됨. raw 결과를 반환한다. |
| `FAILED` | 실행 중 오류 발생 |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Execution Domain | Job Request를 수신하고, 실행 상태 및 raw 결과를 반환한다. |
