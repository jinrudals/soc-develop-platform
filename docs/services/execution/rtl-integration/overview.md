# RTL Integration Service

## 개요

Engineer가 작성한 conf(설정 파일)를 기반으로 RTL을 자동으로 생성하는 Service이다.

다른 Execution Service들이 EDA tool(Synopsys, Cadence 등)을 사용하는 것과 달리,
이 Service는 Chisel3 / RocketChip 등의 **RTL 생성 tool 연동**이 필요하다.
해당 tool 연동 자체가 미구현 상태이므로 추후 추가될 예정이다.

## 타입

**Transform** — conf를 입력으로 받아 RTL을 산출물로 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| RTL Module | 생성 대상 RTL Module |
| conf | Engineer가 작성한 RTL 생성 설정 파일 |

## 출력

| 항목 | 설명 |
|---|---|
| Generated RTL | 자동 생성된 RTL 소스코드 |

## FAIL 조건

- RTL 생성 중 오류가 발생하여 산출물이 정상적으로 생성되지 않은 경우

## 실행 조건

- Engineer가 conf를 작성/변경하고 실행을 요청한 경우

## 관련 팀

- [RTL 팀](../../../teams/RTL.md)
