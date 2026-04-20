# Project 관리 도메인

## 개요

SoC 프로젝트의 운영 자체를 관리하는 도메인이다.
어떤 프로젝트가 존재하고, 어떤 단계로 진행되며, 누가 참여하는지를 정의한다.

**주 관리자: PM**

---

## 이 도메인이 답하는 질문

- 어떤 프로젝트가 있는가?
- 각 프로젝트의 Milestone은 어떻게 구성되는가?
- 누가 이 프로젝트에 참여하는가?
- 참여 인원의 역할은 무엇인가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| Project | SoC 개발 프로젝트 단위. 전체 개발 범위와 목표를 정의한다. |
| Milestone | 프로젝트의 목표 단계. 일정과 목표 버전 정보를 포함한다. |
| ProjectMember | 프로젝트에 참여하는 인원. |
| ProjectMemberRole | 참여 인원의 역할 (Manager / Project Leader / Engineer 등). 프로젝트마다 다를 수 있다. |

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| SCM | 프로젝트에 어떤 Repository가 연결되는지 SCM 도메인이 관리한다. |
| RTL | 프로젝트 범위 내 RTL Module 구조를 RTL 도메인이 관리한다. |
| Version / Baseline | Milestone 기준으로 branch 생성 및 버전 관리가 트리거된다. |
