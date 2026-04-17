# PI After Synthesis Service

## 개요

PI Synthesis Service가 생성한 Synthesized Netlist에 대해 검사를 수행하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.

## 타입

**Check** — Synthesized Netlist를 입력으로 받아 검사 결과(PASS / FAIL)를 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| Synthesized Netlist | PI Synthesis Service가 생성한 Netlist |
| Synthesis Report | PI Synthesis Service가 생성한 결과 리포트 |

## 출력

| 항목 | 설명 |
|---|---|
| 검사 결과 | 항목별 PASS / FAIL 및 상세 log |

## 검사 항목

| 항목 | 설명 |
|---|---|
| Timing | Setup / Hold Timing 충족 여부 |
| Area | 면적 목표 범위 충족 여부 |
| Power | 전력 목표 범위 충족 여부 |

## FAIL 조건

Inspection Domain의 Quality Policy 기준에 따라 결정된다. (Project / Milestone별로 상이할 수 있음)

## 실행 조건

- PI Synthesis Service가 PASS로 완료된 경우

## 관련 팀

- [PI 팀](../../../teams/PI.md)
