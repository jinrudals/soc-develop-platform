# Version Domain

## 개요

각 Repository의 branch 상태, tag 이력, repo 간 버전 의존성을 관리하는 Domain이다.
**자주 변하는 동적 정보**를 다루며, 새로운 tag 생성 등의 상태 변화를 이벤트로 발행하여 다른 Domain을 트리거한다.

## 핵심 개념

| 개념 | 설명 |
|---|---|
| Branch State | Repository의 `develop` / `stage` / `main` 각 branch의 현재 상태. |
| Stage Tag | `stage` branch에 생성된 tag. 시스템 테스트 대상 버전을 나타낸다. |
| Main Tag | `main` branch에 생성된 tag. 테스트 완료 후 승격된 버전을 나타낸다. |
| Release Tag | 최종 release로 지정된 tag. |
| Version Dependency | 상위 Repository가 참조하는 하위 Repository의 버전 정보. `develop` / `stage` / `main` 별로 관리된다. |

## 발행하는 이벤트

| 이벤트 | 설명 |
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

- [RTL 팀](../../teams/RTL.md)
- [PM 팀](../../teams/PM.md)
