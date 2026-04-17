# DFT Insertion Service

## 개요

Functional RTL과 사용자 conf를 기반으로 DFT(Design For Test) 로직을 삽입하여 DFTed RTL을 생성하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.
DFTed RTL이 생성된 후에는 **RTL Quality Check Service를 재수행**하여야 한다.

## 타입

**Transform** — Functional RTL + conf를 입력으로 받아 DFTed RTL을 산출물로 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| Functional RTL | DFT 삽입 대상 RTL Module |
| 버전 | 사용할 Functional RTL의 stage / main / release tag |
| conf | DFT 삽입 설정 파일 |

## 출력

| 항목 | 설명 |
|---|---|
| DFTed RTL | DFT 로직이 삽입된 RTL 소스코드 |

## FAIL 조건

- DFT 삽입 중 오류가 발생하여 DFTed RTL이 정상적으로 생성되지 않은 경우

## 실행 조건

- Functional RTL의 버전이 확정(stage / main / release)된 경우
- 사용자 conf가 변경된 경우

## 후속 작업

DFT Insertion 완료 후 DFTed RTL에 대해 **RTL Quality Check Service**를 재수행하여야 한다.

## 관련 팀

- [DFT 팀](../../../teams/DFT.md)
