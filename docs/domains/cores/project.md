# Project Domain

## 개요

SoC 프로젝트의 구성 정보를 관리하는 Domain이다.
프로젝트에 참여하는 인원, Repository 구조, RTL Module 구성 등 **자주 변하지 않는 정적 정보**를 다룬다.
다른 모든 Domain이 이 Domain의 정보를 참조한다.

## 핵심 개념

| 개념 | 설명 |
|---|---|
| Project | SoC 개발 프로젝트 단위. 참여 인원, Milestone 목록, Repository 목록을 가진다. |
| Member | 프로젝트 참여 인원. 프로젝트마다 역할(Manager / Project Leader / Engineer)이 다를 수 있다. |
| Milestone | 프로젝트의 목표 단계. 일정과 목표 버전 정보를 포함한다. |
| Repository | RTL 소스코드 저장소. 하나의 Repository가 여러 프로젝트에서 사용될 수 있다. |
| RTL Module | Repository 안에 존재하는 IP / HPDF / Subsystem 단위. |
| RTL Hierarchy | RTL Module 간의 연결 관계도. |
| Module Owner | RTL Module의 담당자. 한 Module에 여러 담당자가 있을 수 있으며, 한 사람이 여러 Module을 담당할 수 있다. |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Version Domain | Repository 정보를 참조하여 버전 상태를 관리한다. |
| Execution Domain | RTL Module / Repository 정보를 참조하여 실행 대상을 결정한다. |
| Inspection Domain | Policy의 범위인 Project / Milestone 정보를 참조한다. |
| Workflow Domain | Milestone 정보를 참조하여 흐름을 구성한다. |

## 관련 팀

- [PM 팀](../../teams/PM.md)
- [RTL 팀](../../teams/RTL.md)
