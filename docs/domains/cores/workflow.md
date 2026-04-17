# Workflow Domain

## 개요

GitLab / GitHub 등 외부 시스템의 이벤트 또는 플랫폼 내부 이벤트 발생 시, 후속으로 수행해야 할 액션을 정의하고 실행하는 Domain이다.

별도의 Workflow 엔진을 두지 않는다.
**CI Runner / Automation Runner** 가 trigger-action 역할을 담당하며, 각 기능별 Workflow가 독립적으로 존재한다.

## 역할 구분

| 역할 | 담당 |
|---|---|
| 이벤트 트리거 감지 | CI Runner / Automation Runner (GitLab webhook 등) |
| 후속 액션 정의 | 각 Workflow Service (기능별로 독립) |
| 실행 이력 보관 | 각 Workflow Service가 자체 보관 |

## Workflow 목록

| Workflow | 트리거 | 액션 |
|---|---|---|
| Milestone Start | 상위 Milestone release tag 생성 | 하위 Milestone의 branch 생성 (GitLab), 상태 전환 |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Version Domain | branch / tag 생성 요청 |
| Project Domain | Milestone 상태 조회 및 전환 |
