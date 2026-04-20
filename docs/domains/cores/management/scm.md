# SCM 관리 도메인

## 개요

저장소와 branch 정책을 관리하는 도메인이다.
어떤 Repository를 사용하고, 각 Repository에 어떤 branch 전략을 적용할지를 정의한다.
프로젝트에 Repository를 연결하는 것도 이 도메인의 책임이다.

**주 관리자: 설계팀**

---

## 이 도메인이 답하는 질문

- 어떤 Repository를 사용하는가?
- Repository의 종류(Type)는 무엇인가?
- 어떤 branch 전략을 사용하는가?
- Milestone별 branch naming rule은 무엇인가?
- 이 프로젝트는 어떤 Repository를 포함하는가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| Repository | RTL 소스코드 저장소. 하나의 Repository가 여러 프로젝트에서 사용될 수 있다. |
| RepositoryType | Repository의 종류 (RTL / Verification / Conf 등). 각 Execution 서비스가 관심 가질 타입을 결정하는 기준이다. |
| BranchType | branch의 역할 구분 (develop / stage / main 등). |
| RepositoryBranchConfig | Repository에 적용되는 branch 전략 설정. Milestone별 naming rule을 포함한다. |
| ProjectRepository | 프로젝트와 Repository의 연결 관계. 설계팀이 프로젝트에 Repository를 등록한다. |

---

## ProjectRepository 소유 근거

"어떤 Repository를 이 프로젝트에서 사용할 것인가"를 결정하는 주체가 설계팀이므로,
Project 도메인이 아닌 SCM 도메인이 소유한다.

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| Project | ProjectRepository를 통해 프로젝트와 연결된다. |
| RTL | Repository 안에 RTL Module이 존재한다. |
| Version / Baseline | Repository의 branch 정책을 기반으로 버전 상태를 추적한다. |
