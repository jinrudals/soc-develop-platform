# Verification Service

## 개요

Functional RTL 및 DFTed RTL에 대해 기능, 전력, 성능 검증을 수행하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.

## 타입

**Check** — RTL을 입력으로 받아 검증 결과(PASS / FAIL)를 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| RTL Module | 검증 대상 RTL Module (Functional RTL 또는 DFTed RTL) |
| 버전 | 검증 대상 stage tag 또는 release tag |

## 출력

| 항목 | 설명 |
|---|---|
| 검증 결과 | 검증 항목별 PASS / FAIL 및 상세 log |

## 검증 항목

| 항목 | 설명 |
|---|---|
| Functional 검증 | RTL의 기능이 스펙과 일치하는지 확인 |
| Power 검증 | 전력 소모가 목표 범위 내인지 확인 |
| Performance 검증 | 성능(속도, 처리량 등)이 목표 범위 내인지 확인 |

## FAIL 조건

Inspection Domain의 Quality Policy 기준에 따라 결정된다. (Project / Milestone별로 상이할 수 있음)

## 실행 조건

- IP / HPDF / Subsystem Level: RTL Repository에 새로운 stage tag가 생성된 경우
- SoC Top Level: RTL release tag가 생성된 경우

## 관련 팀

- [Verification 팀](../../../teams/Verification.md)
