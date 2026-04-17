# Version Service — Tables

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| TagType | Tag 유형 |
| RepositoryTagChangeType | Tag 변경 유형 |
| RepositoryTag | Repository의 tag 이력 |
| RepositoryTagChange | Tag ↔ 변경 유형 매핑 |
| VersionDependency | Repository 간 버전 의존성 |

---

## TagType

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: STAGE, MAIN, RELEASE |
| description | string | |

| Unique | `name` |
|---|---|

---

## RepositoryTagChangeType

tag 생성 시 무엇이 변경되었는지를 나타낸다.
Workflow Domain이 이 정보를 기반으로 cascade 알림 대상을 결정한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: RTL_INTERFACE, RTL_LOGIC, CDC_RDC_DOC_INPUT, MANIFEST |
| description | string | |

| Unique | `name` |
|---|---|

### 변경 유형별 cascade 규칙

| 변경 유형 | 상위 repo 담당자 | Verification | SDC |
|---|---|---|---|
| RTL_INTERFACE | ✓ re-integration | ✓ | |
| RTL_LOGIC | | ✓ | |
| CDC_RDC_DOC_INPUT | | | ✓ |
| MANIFEST (child RTL_INTERFACE / RTL_LOGIC 변경) | | ✓ | |
| MANIFEST (child CDC_RDC_DOC_INPUT 변경) | | | ✓ |

> MANIFEST 변경 시 cascade는 VersionDependency를 통해 child tag의 RepositoryTagChangeType을 조회하여 결정한다.

---

## RepositoryTag

tag 간 lineage: `stage tag → main tag → release tag` (fast-forward 관계)

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| repository_id | FK → Repository | |
| tag_type_id | FK → TagType | STAGE / MAIN / RELEASE |
| branch_type_id | FK → BranchType | 어떤 branch 위의 tag인지 |
| milestone_id | FK → Milestone (nullable) | dynamic branch repo의 경우 |
| source_tag_id | FK → RepositoryTag (nullable) | 기반이 된 tag. null이면 최초 tag. FF 여부는 commit_hash로 git에서 확인 |
| name | string | 실제 git tag 이름 |
| commit_hash | string | git commit SHA |
| created_at | timestamp | |

| Unique | `(repository_id, name)` |
|---|---|

---

## RepositoryTagChange

하나의 tag에 여러 변경 유형이 동시에 존재할 수 있다.

| 필드 | 타입 | 설명 |
|---|---|---|
| tag_id | FK → RepositoryTag | |
| change_type_id | FK → RepositoryTagChangeType | |

| Unique | `(tag_id, change_type_id)` |
|---|---|

---

## VersionDependency

상위 Repository가 각 branch context별로 참조하는 하위 Repository의 tag 정보.

| 필드 | 타입 | 설명 |
|---|---|---|
| parent_repository_id | FK → Repository | 예: PERISubsystem |
| child_repository_id | FK → Repository | 예: I2C |
| branch_type_id | FK → BranchType | DEVELOP / STAGE / MAIN |
| milestone_id | FK → Milestone (nullable) | dynamic branch repo의 경우 |
| child_tag_id | FK → RepositoryTag | 참조하는 child repo의 tag |

| Unique | `(parent_repository_id, child_repository_id, branch_type_id, milestone_id)` |
|---|---|
