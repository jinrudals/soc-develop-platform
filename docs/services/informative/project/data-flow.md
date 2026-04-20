# Project Service — Data Flow

---

## Project

### 생성
- 프로젝트가 생성되면 모든 서비스에 broadcast된다.
- 모든 서비스는 Project 단위로 동작하므로, 생성 이벤트를 수신한 서비스는 해당 Project에 대한 내부 컨텍스트를 초기화한다.

### 변경
- 변경된 내용은 모든 서비스에 전달된다.

### 삭제
- 삭제는 없다. `status` 필드(예: ACTIVE / INACTIVE / COMPLETED)로 soft delete 처리한다.
- status 변경도 변경 이벤트로 전파된다.

---

## MilestoneLevel

### 생성 / 변경
- Execution Domain의 모든 서비스에 전달되어야 한다.
- Execution 서비스들은 MilestoneLevel 기준으로 QualityPolicy를 조회하여 성공/실패를 판단하기 때문이다.

---

## Milestone

### 생성
- Milestone 생성 자체는 외부에 전파할 필요 없다.

### 시작 (PLANNED → IN_PROGRESS)
- Version Service에 해당 Milestone의 branch 목록을 생성하도록 요청한다.
- `milestone-start` Workflow가 트리거된다.

---

## Repository

### 추가 / 변경
- 모든 서비스에 broadcast된다. 이벤트에는 `RepositoryType`이 포함된다.
- 수신 측 서비스가 자신이 tracking할 `RepositoryType`을 사전에 설정하고, 해당 타입의 이벤트만 처리한다.

| RepositoryType | 관심을 가질 서비스 (예시) |
|---|---|
| RTL | Version Service, RTL Quality Check, Verification, RTL Integration |
| Verification | Verification Service |
| Conf (DFT/PI/PD/SDC) | 해당 Execution Service |

> 라우팅 책임은 수신 측에 있다. Project Service는 타입 무관하게 전체 broadcast한다.

---

## RTLModule

### 추가 / 변경
- Version Service 및 모든 Execution Service에 broadcast된다.
- 각 서비스는 RTLModule을 실행 대상 단위로 사용하므로 모두 수신이 필요하다.

---

## RTLHierarchy

### 추가 / 변경
- Version Service 및 모든 Execution Service에 broadcast된다.
- RTLHierarchy는 CrossDependencyRule, 실행 순서 결정 등에 사용되므로 모두 수신이 필요하다.

---

## ProjectMember

### 추가 / 변경 / 제거
- 모든 서비스에 broadcast된다.
- 각 서비스는 프로젝트 내 역할(role) 정보를 수신하여 권한 판단에 활용할 수 있다.

---

## ModuleOwner

### 추가 / 변경 / 제거
- 모든 서비스에 broadcast된다.
- 주로 Notification 용도로 활용된다. Execution 결과 전달 시 담당자를 식별하기 위해 수신 측이 보관한다.
