# SoC RTL 설계팀

## 개요

SoC RTL 설계팀은 SoC 프로젝트의 Functional RTL을 설계하고 구현하는 업무를 담당한다.

SoC는 여러 IP / HPDF / Subsystem 단위(이하 **RTL Module**)로 구성되며, 개발 편의성을 위해 하나 또는 그 이상의 Repository로 나누어 구현된다.

---

## 관리 대상

### 버전 관리 관점

모든 Repository는 3단계 Branch 전략을 사용한다.

| Branch | 설명 |
|---|---|
| `main` | 시스템 테스트 완료 후 Release Candidate로 등록된 상태. `stage` branch로부터 자동 생성되며, 어떤 `stage` commit(또는 tag)으로부터 생성되었는지 metadata가 필요하다. |
| `stage` | 개발이 어느 정도 완료되어 시스템 테스트가 필요한 버전. 테스트 완료 시 `main`으로 승격된다. |
| `develop` | 실제 개발이 진행되는 branch. 담당자는 direct push 또는 Merge Request를 통해 반영한다. |

> Branch 이름은 Repository마다 다를 수 있다.
> - Static 방식: `main`, `stage`, `develop`
> - Dynamic 방식: `main`, `${MILESTONE_NAME}/stage`, `${MILESTONE_NAME}/develop`

확인이 필요한 사항:
- 각 Repository에서 현재 `develop` / `stage` / `main` 으로 진행 중인 버전은 무엇인가
- 각 Repository가 참조하는 하위 Repository의 `stage` / `main` 버전은 무엇인가
- Milestone 기준으로 각 Repository의 최종 버전은 무엇인가

### 프로젝트 현황 관점

- 각 RTL Module의 `stage` 및 `main` 현황은 어떻게 되는가

---

## 시스템이 수행하여야 하는 것

시스템은 RTL Module 자체에 대한 **Quality Check**를 수행한다. (검증과는 구분됨)

> **검증**: RTL Module을 DUT(Device Under Test)로 간주하여 기능을 확인하는 작업
> **Quality Check**: EDA tool 기반의 품질 확인 작업

Quality Check 항목:

| 항목 | 설명 |
|---|---|
| Compile | RTL Module의 컴파일 가능 여부 확인 |
| RTL-LINT | 코딩 규칙 및 잠재적 오류 검사 |
| RTL-CDC/RDC | Clock Domain Crossing / Reset Domain Crossing 검사 |
| Synthesizable | 합성 가능 여부 확인 |
