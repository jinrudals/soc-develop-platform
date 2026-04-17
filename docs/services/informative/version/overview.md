# Version Service

## 개요

각 Repository의 branch 상태, tag 이력, repo 간 버전 의존성을 관리하는 Service이다.
**동적인 정보**를 다루며, 상태 변화 시 이벤트를 발행하여 다른 Domain을 트리거한다.

**업데이트 주체: 시스템** (Git webhook, CI/CD 등이 자동으로 업데이트)

## 관리 대상

| 대상 | 설명 |
|---|---|
| Branch State | Repository의 `develop` / `stage` / `main` 각 branch의 현재 상태 |
| Stage Tag | `stage` branch에 생성된 tag 이력 |
| Main Tag | `main` branch에 생성된 tag 이력 |
| Release Tag | 최종 release로 지정된 tag 이력 |
| Version Dependency | 상위 Repository가 참조하는 하위 Repository의 버전 정보 (`develop` / `stage` / `main` 별) |

## 발행하는 이벤트

| 이벤트 | 트리거 조건 |
|---|---|
| `StageTagCreated` | `stage` branch에 새로운 tag가 생성됨 |
| `MainTagCreated` | `main` branch에 새로운 tag가 생성됨 |
| `ReleaseTagCreated` | 새로운 release tag가 생성됨 |
| `VersionDependencyUpdated` | 상위 Repo가 참조하는 하위 Repo의 버전이 변경됨 |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Project Domain | Repository 목록을 참조한다. |
| Execution Domain | 이벤트를 수신하여 실행 대상 버전을 결정한다. |
| Workflow Domain | 이벤트를 수신하여 다음 단계 진입 가능 여부를 판단한다. |

## 관련 팀

- [RTL 팀](../../../teams/RTL.md)
- [PM 팀](../../../teams/PM.md)
