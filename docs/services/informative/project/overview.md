# Project Service

## 개요

SoC 프로젝트의 구성 정보를 관리하는 Service이다.
Project, Member, Milestone, Repository, RTL Module, RTL Hierarchy, Module Owner 등 **정적인 정보**를 다룬다.

**업데이트 주체: 사람** (Manager, Project Leader, Engineer 등이 직접 입력/수정)

## 관리 대상

| 대상 | 설명 |
|---|---|
| Project | 프로젝트 생성, 수정, 조회 |
| Member | 프로젝트 참여 인원 및 역할(Manager / Project Leader / Engineer) 관리 |
| Milestone | 프로젝트 Milestone 및 일정 관리 |
| Repository | 프로젝트에서 사용하는 Repository 등록 및 관리 |
| RTL Module | Repository 내 RTL Module 목록 관리 |
| RTL Hierarchy | RTL Module 간 연결 관계도 관리 |
| Module Owner | RTL Module 담당자 지정 및 관리 |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Version Domain | Repository / RTL Module 정보를 참조한다. |
| Execution Domain | RTL Module / Repository 정보를 참조하여 실행 대상을 결정한다. |
| Inspection Domain | Project / Milestone 정보를 기준으로 Quality Policy를 구성한다. |
| Workflow Domain | Milestone 정보를 참조하여 흐름을 구성한다. |

## 관련 팀

- [PM 팀](../../../teams/PM.md)
- [RTL 팀](../../../teams/RTL.md)
