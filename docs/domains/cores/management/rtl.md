# RTL 관리 도메인

## 개요

설계 구조 자체를 관리하는 도메인이다.
어떤 RTL Module이 존재하고, Module 간 관계가 어떻게 구성되며, 누가 각 Module을 담당하는지를 정의한다.

**주 관리자: 설계팀**

---

## 이 도메인이 답하는 질문

- 어떤 RTL Module이 있는가?
- Module의 종류(Type)는 무엇인가?
- Module 간 계층 관계는 어떻게 되는가?
- 누가 어느 Module을 담당하는가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| RTLModule | Repository 안에 존재하는 설계 단위. IP / HPDF / Subsystem 등이 해당된다. |
| RTLModuleType | RTL Module의 종류 구분. |
| RTLHierarchy | RTL Module 간의 연결 관계 및 계층 구조. SoC TOP까지의 integration 구조를 나타낸다. |
| ModuleOwner | RTL Module의 담당자. 한 Module에 여러 담당자가 있을 수 있으며, 한 사람이 여러 Module을 담당할 수 있다. |

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| SCM | RTL Module은 SCM 도메인의 Repository 안에 존재한다. |
| Version / Baseline | RTL Module 단위로 버전 의존성과 실행 기준이 결정된다. |
| Execution | RTL Module을 실행 대상 단위로 사용한다. |
