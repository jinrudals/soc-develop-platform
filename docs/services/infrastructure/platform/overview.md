# Platform Service

## 개요

SoC Develop Platform 자체의 사용자, 역할, 권한을 관리하는 Service이다.
SoC 프로젝트 정보와는 독립적으로, **플랫폼 운영에 필요한 기반 정보**를 다룬다.

**업데이트 주체: 플랫폼 관리자**

## 관리 대상

| 대상 | 설명 |
|---|---|
| Platform User | 플랫폼에 접근할 수 있는 사용자 계정 관리 |
| Platform Role | 플랫폼 내 역할 정의. 예: System Admin, Project Leader, Engineer 등 |
| Permission | 역할별 기능 접근 권한 관리 |

## 연관 Domain

| Domain | 관계 |
|---|---|
| Project Domain | Platform User가 SoC 프로젝트에 Member로 등록될 때 참조된다. |

## 관련 팀

- [PM 팀](../../../teams/PM.md)
