# PD After P&R Service

## 개요

PD P&R Service가 생성한 Layout에 대해 검사를 수행하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.

## 타입

**Check** — Layout을 입력으로 받아 검사 결과(PASS / FAIL)를 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| Layout | PD P&R Service가 생성한 Layout 데이터 |
| P&R Report | PD P&R Service가 생성한 결과 리포트 |

## 출력

| 항목 | 설명 |
|---|---|
| 검사 결과 | 항목별 PASS / FAIL 및 상세 log |

## 검사 항목

| 항목 | 설명 |
|---|---|
| Timing | Final Timing 충족 여부 |
| DRC | Design Rule Check 통과 여부 |
| LVS | Layout vs Schematic 일치 여부 |

## FAIL 조건

Inspection Domain의 Quality Policy 기준에 따라 결정된다. (Project / Milestone별로 상이할 수 있음)

## 실행 조건

- PD P&R Service가 PASS로 완료된 경우

## 관련 팀

- [PD 팀](../../../teams/PD.md)
