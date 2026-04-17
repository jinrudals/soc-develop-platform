# Milestone Start Workflow

## 개요

상위 Milestone이 release되면 하위 Milestone을 시작하는 Workflow이다.
하위 Milestone의 branch를 상위 Milestone의 release tag 기반으로 생성하고, 상태를 전환한다.

## 트리거 조건

- 상위 Milestone(`parent_id`)의 release tag가 생성됨
- 하위 Milestone이 존재하며 상태가 `PLANNED`인 경우

## 처리 흐름

```
parent Milestone release tag 생성 감지 (Version Domain 이벤트)
    ↓
하위 Milestone 조회 (Project Service)
    ↓
하위 Milestone의 branch 생성 요청 (Version Service)
  - 각 Repository의 develop / stage / main branch를
    parent Milestone release tag 기반으로 생성
    ↓
하위 Milestone 상태 PLANNED → IN_PROGRESS 전환 (Project Service)
```

## 연관 Service

| Service | 역할 |
|---|---|
| Version Service | parent Milestone release 이벤트 수신, 새 Milestone branch 생성 |
| Project Service | 하위 Milestone 조회 및 상태 전환 |

## 관련 팀

- [PM 팀](../../../teams/PM.md)
