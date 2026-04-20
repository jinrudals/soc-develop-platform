# Job Runner (Infrastructure)

## 개요

Execution Domain 서비스들과 외부 실행 도구(Jenkins / Airflow 등) 사이의 **매개체**다.
비즈니스 로직을 가지지 않으며, 신호를 받아 전달하고 결과를 돌려주는 역할만 한다.

Domain이 아닌 **Infrastructure**로 분류한다.

## 역할

```
Execution Service
      │ 실행 시그널
      ▼
  Job Runner         ← 매개체 (비즈니스 로직 없음)
      │ 매칭된 pipeline job 트리거
      ▼
Pipeline Tool        ← 회사/환경마다 다를 수 있음
      │ 실행
      ▼
  별도 실행 제품      ← 실제 EDA Tool 수행
      │ 결과 콜백
      ▼
  Job Runner
      │ 결과 전달
      ▼
Execution Service
```

## 하는 것 / 하지 않는 것

| 하는 것 | 하지 않는 것 |
|---|---|
| Execution Service 시그널 수신 | PASS / FAIL 판단 |
| 매칭된 pipeline job 트리거 | 실행 결과 해석 |
| 실행 상태 콜백 전달 | 비즈니스 규칙 적용 |
| raw 결과 전달 | 실제 EDA Tool 수행 |

## Pipeline Tool 추상화

각 회사 / 환경마다 사용하는 pipeline 도구가 다를 수 있다.
Job Runner는 특정 도구에 종속되지 않도록 추상화된 인터페이스를 제공한다.

```
Job Runner
    │
    ├── Jenkins Adapter
    ├── Airflow Adapter
    ├── GitLab CI Adapter
    └── (기타 회사별 Custom Adapter)
```

Execution Domain은 어떤 pipeline 도구를 쓰는지 알지 못하며, 알 필요도 없다.

## 연관

| 대상 | 관계 |
|---|---|
| Execution Domain | 시그널을 수신하고 결과를 반환하는 대상 |
| Pipeline Tool | 트리거하는 외부 실행 도구 (회사마다 다를 수 있음) |
| 별도 실행 제품 | 실제 EDA Tool을 수행하는 외부 시스템 |
