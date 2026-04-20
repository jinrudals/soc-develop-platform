# Feature Specification: Project Service Module

**Feature Branch**: `001-project-service`  
**Created**: 2026-04-20  
**Status**: Draft  
**Input**: User description: "project service 부터 만들어볼꺼야. Project 서비스에 대한 문서는 docs/services/informative/project 에 있고, 대상은 modules/project 에 만들면돼. 우선은 modules/project 자체를 개발하기 위해서 사용될 문서만 modules/project/docs 에 만들어보자"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Project 생성 및 기본 정보 관리 (Priority: P1)

Manager 또는 Project Leader가 새로운 SoC 프로젝트를 플랫폼에 등록하고, 프로젝트 이름·설명 등 기본 정보를 관리한다. 프로젝트는 모든 하위 정보(멤버, 마일스톤, 레포지토리 등)의 기준점이 되므로, 가장 먼저 생성되어야 한다.

**Why this priority**: 프로젝트가 없으면 다른 모든 관리 대상(멤버, 마일스톤, 레포지토리 등)을 등록할 수 없다. 플랫폼의 출발점이다.

**Independent Test**: 프로젝트를 생성한 뒤 목록 조회 및 단건 조회가 가능한지 확인하는 것만으로 이 스토리는 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** 이름이 "SoC-A"인 프로젝트가 존재하지 않을 때, **When** Manager가 이름 "SoC-A"로 프로젝트를 생성하면, **Then** 프로젝트가 저장되고 조회 가능해진다.
2. **Given** 이름이 "SoC-A"인 프로젝트가 이미 존재할 때, **When** 동일한 이름으로 프로젝트 생성을 시도하면, **Then** 중복 오류가 반환된다.
3. **Given** 프로젝트가 존재할 때, **When** 프로젝트 이름 또는 설명을 수정하면, **Then** 변경 내용이 반영된다.
4. **Given** 아무 프로젝트나 존재할 때, **When** 전체 프로젝트 목록을 조회하면, **Then** 모든 프로젝트가 반환된다.

---

### User Story 2 - 프로젝트 멤버 및 역할 관리 (Priority: P2)

Manager가 프로젝트에 참여하는 인원을 등록하고, 각 인원에게 Manager / Project Leader / Engineer 역할을 부여한다. 한 사람은 하나의 프로젝트 내에서 하나의 역할만 갖는다.

**Why this priority**: 멤버 정보는 이후 Module Owner 지정, 워크플로우 권한 등에서 참조되므로 프로젝트 생성 직후 설정이 필요하다.

**Independent Test**: 멤버를 추가·변경·제거한 뒤 멤버 목록을 조회하여 결과를 확인하는 것만으로 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** 플랫폼에 등록된 사용자가 있을 때, **When** Manager가 해당 사용자를 프로젝트에 Engineer로 추가하면, **Then** 멤버 목록에 반영된다.
2. **Given** 멤버가 이미 프로젝트에 등록되어 있을 때, **When** 동일 사용자를 다시 추가하려 하면, **Then** 중복 오류가 반환된다.
3. **Given** 멤버가 존재할 때, **When** 역할을 Engineer → Project Leader로 변경하면, **Then** 변경된 역할이 반영된다.
4. **Given** 멤버가 존재할 때, **When** 해당 멤버를 프로젝트에서 제거하면, **Then** 멤버 목록에서 삭제된다.

---

### User Story 3 - Milestone 생성 및 일정 관리 (Priority: P3)

Project Leader가 프로젝트의 Milestone(예: ML1, ML2)을 등록하고 시작일·종료일 등 일정을 관리한다. Milestone은 계층 구조(이전 Milestone 기반)를 가질 수 있다.

**Why this priority**: Milestone 정보는 Inspection, Workflow 도메인에서 참조된다. 프로젝트 멤버 설정 이후 정의된다.

**Independent Test**: Milestone을 생성·조회하고 parent 관계가 올바른지 확인하는 것만으로 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** 프로젝트와 MilestoneLevel이 존재할 때, **When** Project Leader가 Milestone(이름, 레벨, 시작일/종료일)을 생성하면, **Then** 해당 Milestone이 프로젝트 하위에 조회된다.
2. **Given** 프로젝트 내 동일 이름의 Milestone이 존재할 때, **When** 동일 이름으로 Milestone을 추가하면, **Then** 중복 오류가 반환된다.
3. **Given** Milestone A가 존재할 때, **When** Milestone B를 A를 parent로 설정하여 생성하면, **Then** B의 parent는 A를 가리킨다.
4. **Given** Milestone이 존재할 때, **When** 시작일·종료일을 수정하면, **Then** 변경 내용이 반영된다.

---

### User Story 4 - Repository 등록 및 Branch 패턴 설정 (Priority: P4)

Project Leader 또는 Engineer가 프로젝트에서 사용하는 Git Repository를 플랫폼에 등록하고, 각 BranchType(MAIN, STAGE, DEVELOP 등)별 branch 이름 패턴을 설정한다.

**Why this priority**: Repository 정보는 RTL Module 등록 및 Version 도메인에서 참조된다.

**Independent Test**: Repository를 등록하고 branch 패턴을 설정한 뒤 조회하는 것만으로 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** RepositoryType과 BranchType이 존재할 때, **When** Repository(이름, URL, 유형)를 등록하면, **Then** 플랫폼에 저장되고 조회 가능해진다.
2. **Given** 동일 URL의 Repository가 이미 존재할 때, **When** 같은 URL로 다시 등록하면, **Then** 중복 오류가 반환된다.
3. **Given** Repository가 등록되었을 때, **When** DEVELOP BranchType에 `{milestone_name}/develop` 패턴을 설정하면, **Then** 해당 설정이 저장된다.
4. **Given** Repository가 프로젝트에 연결되었을 때, **When** 프로젝트의 Repository 목록을 조회하면, **Then** 연결된 Repository가 모두 반환된다.

---

### User Story 5 - RTL Module 등록 및 계층 구조 관리 (Priority: P5)

Engineer가 Repository 내 RTL Module을 플랫폼에 등록하고, Module 간 부모-자식 계층 관계(Hierarchy)를 설정한다. 공통 IP가 여러 상위 Module에서 사용될 수 있으므로 DAG 구조를 지원한다.

**Why this priority**: RTL Module 정보는 Execution, Version 도메인에서 참조되므로 Repository 등록 이후 필요하다.

**Independent Test**: RTL Module을 등록하고 Hierarchy를 설정한 뒤 조회하는 것만으로 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** Repository와 RTLModuleType이 존재할 때, **When** RTL Module(이름, RTL 코드명, 유형)을 등록하면, **Then** Repository 하위 Module 목록에 반영된다.
2. **Given** 동일 Repository 내 같은 이름의 Module이 존재할 때, **When** 동일 이름으로 등록하면, **Then** 중복 오류가 반환된다.
3. **Given** Module A와 Module B가 존재할 때, **When** A를 B의 부모로 Hierarchy를 설정하면, **Then** A→B 관계가 저장된다.
4. **Given** A→B→C 관계가 존재할 때, **When** C를 A의 부모로 설정(사이클 생성 시도)하면, **Then** 사이클 오류가 반환된다.

---

### User Story 6 - Module Owner 지정 (Priority: P6)

Manager 또는 Project Leader가 프로젝트 내 각 RTL Module에 담당자(Engineer)를 지정한다. 프로젝트마다 담당자가 다를 수 있으며, 한 Module에 여러 담당자를 지정할 수 있다.

**Why this priority**: Module Owner 정보는 Inspection 및 Workflow 도메인의 책임자 식별에 사용된다.

**Independent Test**: Module Owner를 지정·조회·제거하는 것만으로 독립적으로 검증된다.

**Acceptance Scenarios**:

1. **Given** 프로젝트, RTL Module, 프로젝트 멤버가 존재할 때, **When** 해당 멤버를 Module Owner로 지정하면, **Then** Module Owner 목록에 반영된다.
2. **Given** 동일 (프로젝트, Module, 사용자) 조합의 Owner가 이미 존재할 때, **When** 다시 지정하면, **Then** 중복 오류가 반환된다.
3. **Given** Module Owner가 존재할 때, **When** 해당 Owner를 제거하면, **Then** 목록에서 삭제된다.

---

### Edge Cases

- 프로젝트 삭제 시 하위 멤버, Milestone, Repository 연결, RTL Module, Module Owner가 모두 함께 처리되어야 한다.
- 플랫폼에 등록되지 않은 사용자를 멤버로 추가하려 하면 오류가 반환된다.
- RTLHierarchy에서 존재하지 않는 Module을 parent 또는 child로 지정하면 오류가 반환된다.
- Milestone의 end_date가 start_date보다 앞설 경우 유효성 오류가 반환된다.
- Repository가 어떤 프로젝트에도 연결되지 않은 상태에서 RTL Module이 등록된 경우, 해당 Module은 조회는 가능하지만 프로젝트 컨텍스트 없이 사용된다.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: 시스템은 프로젝트를 생성·조회·수정할 수 있어야 한다. 프로젝트 이름은 플랫폼 전체에서 고유해야 한다.
- **FR-002**: 시스템은 MilestoneLevel을 사전 정의된 값(예: ML1, ML2, ML3)으로 관리해야 하며, 프로젝트별 Milestone을 생성·조회·수정할 수 있어야 한다.
- **FR-003**: 시스템은 프로젝트에 플랫폼 사용자를 멤버로 추가하고, Manager / Project Leader / Engineer 역할을 부여·변경·제거할 수 있어야 한다. 한 사람은 프로젝트 내에서 하나의 역할만 가진다.
- **FR-004**: 시스템은 Repository를 등록·조회·수정할 수 있어야 하며, Repository URL은 플랫폼 전체에서 고유해야 한다.
- **FR-005**: 시스템은 Repository별로 BranchType에 따른 branch 이름 패턴을 설정·조회할 수 있어야 한다. `{milestone_name}` placeholder를 지원한다.
- **FR-006**: 시스템은 프로젝트와 Repository를 다대다로 연결·해제할 수 있어야 한다.
- **FR-007**: 시스템은 Repository 내 RTL Module을 등록·조회·수정할 수 있어야 한다. 동일 Repository 내 Module 이름은 고유해야 한다.
- **FR-008**: 시스템은 RTL Module 간 부모-자식 계층 관계(Hierarchy)를 설정·조회·삭제할 수 있어야 하며, 사이클 생성을 방지해야 한다.
- **FR-009**: 시스템은 프로젝트별 RTL Module에 담당자(플랫폼 사용자)를 지정·조회·제거할 수 있어야 한다. 동일 (프로젝트, Module, 사용자) 조합은 중복 불가이다.
- **FR-010**: 각 참조 데이터(ProjectMemberRole, RepositoryType, BranchType, RTLModuleType)는 시스템이 초기값을 제공하며, 필요 시 관리자가 추가·수정할 수 있어야 한다.

### Key Entities

- **Project**: SoC 프로젝트. 이름(고유)과 설명을 가지며 모든 하위 정보의 기준점이다.
- **Milestone**: 프로젝트의 개발 단계 이정표. MilestoneLevel(ML1 등)과 연결되며, 선행 Milestone(parent)을 참조할 수 있다. 시작일·종료일을 가진다.
- **ProjectMember**: 프로젝트에 참여하는 사용자와 역할의 매핑. 프로젝트 내 한 사용자는 하나의 역할만 가진다.
- **Repository**: Git Repository. URL(고유)과 유형(RepositoryType)을 가진다. BranchType별 branch 패턴 설정을 포함한다.
- **RTLModule**: Repository 내 RTL 모듈. 플랫폼 이름과 실제 RTL 코드 내 이름을 구분하여 관리한다.
- **RTLHierarchy**: RTL Module 간 부모-자식 관계. 공통 IP의 재사용을 반영하여 DAG 구조를 허용한다.
- **ModuleOwner**: 프로젝트 컨텍스트 내 RTL Module 담당자. 프로젝트마다 담당자가 다를 수 있다.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 프로젝트 생성부터 멤버·Milestone·Repository·RTL Module까지 전체 초기 셋업을 30분 이내에 완료할 수 있다.
- **SC-002**: 관리 대상(프로젝트, 멤버, Milestone, Repository, Module 등) 목록 조회가 1초 이내에 반환된다.
- **SC-003**: 잘못된 입력(중복 이름, 사이클, 존재하지 않는 참조 등)에 대해 100% 명확한 오류 메시지가 반환된다.
- **SC-004**: Version, Execution, Inspection, Workflow 도메인이 Project Service 데이터를 참조할 때 필요한 모든 정보를 단일 조회로 얻을 수 있다.
- **SC-005**: RTL Module Hierarchy에서 사이클이 생성되는 요청은 100% 거부된다.

## Assumptions

- 플랫폼 사용자(User) 정보는 별도의 Auth/User 서비스에서 관리되며, Project Service는 user_id를 외래키로만 참조한다.
- MilestoneLevel(ML1, ML2 등), ProjectMemberRole(Manager, Project Leader, Engineer), RepositoryType, BranchType, RTLModuleType은 초기 데이터로 제공되며 운영 중에는 거의 변경되지 않는다.
- 프로젝트 삭제 기능은 v1 범위에서는 하드 딜리트로 처리하되, 연관 데이터의 무결성은 보장한다.
- RTLHierarchy의 사이클 방지는 애플리케이션 레벨에서 처리한다.
- 이 서비스는 내부 플랫폼 서비스로, 인증된 내부 사용자만 접근한다.
- `{milestone_name}` placeholder 치환은 Version Service의 책임이며, Project Service는 패턴 문자열을 그대로 저장한다.
