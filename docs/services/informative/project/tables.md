# Project Service — Tables

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| Project | SoC 프로젝트 |
| MilestoneLevel | Milestone 단계 유형 (ML1, ML2, ML3 ...) |
| Milestone | 프로젝트 Milestone |
| ProjectMemberRole | 프로젝트 내 역할 유형 |
| ProjectMember | 프로젝트 참여 인원 및 역할 |
| RepositoryType | Repository 유형 |
| BranchType | Branch 유형 |
| Repository | Git Repository |
| RepositoryBranchConfig | Repository별 Branch 패턴 설정 |
| ProjectRepository | Project ↔ Repository 매핑 |
| RTLModuleType | RTL Module 유형 |
| RTLModule | RTL Module |
| RTLHierarchy | RTL Module 간 연결 관계 (DAG) |
| ModuleOwner | 프로젝트별 RTL Module 담당자 |

---

## Project

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | |
| description | string | |

| Unique | `name` |
|---|---|

---

## MilestoneLevel

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: ML1, ML2, ML3 |
| description | string | |

| Unique | `name` |
|---|---|

---

## Milestone

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| project_id | FK → Project | |
| parent_id | FK → Milestone (nullable) | 기반이 되는 이전 Milestone. null이면 최초 Milestone |
| level_id | FK → MilestoneLevel | |
| name | string | branch 이름 패턴에도 사용됨 |
| description | string | |
| start_date | date | |
| end_date | date | |

| Unique | `(project_id, name)` |
|---|---|

---

## ProjectMemberRole

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: Manager, Project Leader, Engineer |
| description | string | |

| Unique | `name` |
|---|---|

---

## ProjectMember

| 필드 | 타입 | 설명 |
|---|---|---|
| project_id | FK → Project | |
| user_id | FK → Platform User | |
| role_id | FK → ProjectMemberRole | |

| Unique | `(project_id, user_id)` — 프로젝트 내 한 사람은 하나의 역할 |
|---|---|

---

## RepositoryType

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: RTL, Shared Script |
| description | string | |

| Unique | `name` |
|---|---|

---

## BranchType

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: MAIN, STAGE, DEVELOP |
| description | string | |

| Unique | `name` |
|---|---|

---

## Repository

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| type_id | FK → RepositoryType | |
| name | string | |
| url | string | Git URL |
| description | string | |

| Unique | `url` |
|---|---|

---

## RepositoryBranchConfig

Repository 생성 시 BranchType별 branch 패턴을 함께 설정한다.
`{milestone_name}` 을 placeholder로 사용하면 Version Service가 실제 Milestone name으로 치환한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| repository_id | FK → Repository | |
| branch_type_id | FK → BranchType | |
| pattern | string | 예: `main`, `stage`, `{milestone_name}/develop` |

| Unique | `(repository_id, branch_type_id)` |
|---|---|

---

## ProjectRepository

| 필드 | 타입 | 설명 |
|---|---|---|
| project_id | FK → Project | |
| repository_id | FK → Repository | |

| Unique | `(project_id, repository_id)` |
|---|---|

---

## RTLModuleType

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| name | string | 예: IP, HPDF, Subsystem |
| description | string | |

| Unique | `name` |
|---|---|

---

## RTLModule

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| repository_id | FK → Repository | |
| type_id | FK → RTLModuleType | |
| name | string | 플랫폼에서 부르는 이름 |
| rtl_module_name | string | 실제 RTL 코드 내 module 이름 |
| description | string | |

| Unique | `(repository_id, name)` |
|---|---|

---

## RTLHierarchy

공통 IP가 여러 상위 Module에서 사용될 수 있으므로 DAG(Directed Acyclic Graph) 구조를 허용한다.
Cycle 방지는 애플리케이션 레벨에서 처리한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| parent_module_id | FK → RTLModule | |
| child_module_id | FK → RTLModule | |

| Unique | `(parent_module_id, child_module_id)` |
|---|---|

---

## ModuleOwner

프로젝트마다 RTL Module 담당자가 다를 수 있으며, 한 Module에 여러 담당자가 있을 수 있다.

| 필드 | 타입 | 설명 |
|---|---|---|
| project_id | FK → Project | |
| rtl_module_id | FK → RTLModule | |
| user_id | FK → Platform User | |

| Unique | `(project_id, rtl_module_id, user_id)` |
|---|---|
