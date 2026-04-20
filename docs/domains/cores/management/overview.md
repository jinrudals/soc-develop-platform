# 관리 도메인 Overview

SoC Develop Platform의 관리 도메인은 플랫폼 전체의 기반 정보를 정의하고 유지하는 영역이다.
Execution, Workflow 등 모든 도메인이 이 관리 도메인의 정보를 참조하여 동작한다.

---

## 도메인 구성

| 도메인 | 주 관리자 | 한 줄 설명 |
|---|---|---|
| [Project](project.md) | PM | 프로젝트 운영 자체를 관리 |
| [SCM](scm.md) | 설계팀 | 저장소와 branch 정책을 관리 |
| [RTL](rtl.md) | 설계팀 | 설계 구조(Module, Hierarchy)를 관리 |
| [Version / Baseline](version-baseline.md) | 시스템 (자동) | 공식 버전과 Repository 간 의존성을 관리 |

---

## 도메인 관계도

```
┌─────────────────────────────────────────────────────────────┐
│  Project                                                    │
│  Project / Milestone / ProjectMember / ProjectMemberRole    │
└───────────────────────────┬─────────────────────────────────┘
                            │ 프로젝트 범위 제공
          ┌─────────────────┼─────────────────┐
          ▼                 ▼                 ▼
┌─────────────────┐ ┌───────────────┐        │
│  SCM            │ │  RTL          │        │
│  Repository     │ │  RTLModule    │        │
│  BranchConfig   │ │  RTLHierarchy │        │
│  ProjectRepo    │ │  ModuleOwner  │        │
└────────┬────────┘ └───────┬───────┘        │
         │                  │                │
         └──────────┬───────┘                │
                    ▼                        │
         ┌──────────────────────┐            │
         │  Version / Baseline  │◄───────────┘
         │  RepositoryTag       │  Milestone 기준 branch 생성
         │  VersionDependency   │
         │  TagChange           │
         └──────────┬───────────┘
                    │ 이벤트 발행
          ┌─────────┴──────────┐
          ▼                    ▼
   Execution Domain      Workflow Domain
```

---

## 1. Project 도메인

**주 관리자: PM**

SoC 프로젝트의 운영 자체를 관리한다. 어떤 프로젝트가 존재하고, 어떤 단계로 진행되며, 누가 참여하는지를 정의한다.

| 개념 | 설명 |
|---|---|
| Project | SoC 개발 프로젝트 단위 |
| Milestone | 프로젝트의 목표 단계. 일정과 목표 버전 정보 포함 |
| ProjectMember | 프로젝트 참여 인원 |
| ProjectMemberRole | 참여 인원의 역할 (Manager / Project Leader / Engineer 등) |

이 도메인이 답하는 질문:
- 어떤 프로젝트가 있는가?
- 각 프로젝트의 Milestone은 어떻게 구성되는가?
- 누가 참여하는가? 역할은 무엇인가?

---

## 2. SCM 도메인

**주 관리자: 설계팀**

저장소와 branch 정책을 관리한다. 프로젝트에 어떤 Repository를 사용하고, 어떤 branch 전략을 적용할지를 정의한다.

| 개념 | 설명 |
|---|---|
| Repository | RTL 소스코드 저장소 |
| RepositoryType | Repository의 종류 (RTL / Verification / Conf 등) |
| BranchType | branch의 역할 구분 (develop / stage / main 등) |
| RepositoryBranchConfig | Repository별 branch 전략 설정. Milestone별 naming rule 포함 |
| ProjectRepository | 프로젝트와 Repository의 연결 관계 (설계팀이 등록) |

이 도메인이 답하는 질문:
- 어떤 Repository를 사용하는가?
- 어떤 branch 전략을 사용하는가?
- Milestone별 branch naming rule은 무엇인가?

---

## 3. RTL 도메인

**주 관리자: 설계팀**

설계 구조 자체를 관리한다. 어떤 RTL Module이 존재하고, Module 간 관계가 어떻게 구성되며, 누가 담당하는지를 정의한다.

| 개념 | 설명 |
|---|---|
| RTLModule | Repository 안의 설계 단위 (IP / HPDF / Subsystem 등) |
| RTLModuleType | RTL Module의 종류 구분 |
| RTLHierarchy | RTL Module 간 계층 관계. SoC TOP까지의 integration 구조 |
| ModuleOwner | RTL Module 담당자 |

이 도메인이 답하는 질문:
- 어떤 RTL Module이 있는가?
- Module 간 관계는 어떻게 되는가?
- 누가 어느 Module을 담당하는가?

---

## 4. Version / Baseline 도메인

**주 관리자: 시스템 (자동)**

공식 버전(Baseline)과 Repository 간 버전 의존성을 관리한다.
원래 사람이 수동으로 추적하던 영역이며, 플랫폼이 자동화하는 핵심 가치 중 하나다.

| 개념 | 설명 |
|---|---|
| RepositoryTag | Repository의 특정 시점을 가리키는 공식 기준선 단위 |
| TagType | tag의 역할 구분 (stage / main / release) |
| RepositoryTagChange | tag 생성 또는 승격 이력 |
| RepositoryTagChangeType | 변경의 종류 구분 |
| VersionDependency | 상위 Repository가 참조하는 하위 Repository의 버전 정보 |

Tag 승격 흐름:
```
Stage Tag → (검증 완료) → Main Tag → (최종 확정) → Release Tag
```

이 도메인이 답하는 질문:
- 현재 어떤 tag가 공식 Baseline인가?
- stage / main / release 관계는 어떻게 되는가?
- 상위 Repository는 하위 Repository의 어떤 버전을 참조하는가?
- 어떤 변경이 이 tag에 포함됐는가?
