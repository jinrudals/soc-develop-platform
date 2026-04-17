# PD P&R Service

## 개요

Synthesized Netlist에 대해 Place-and-Route(P&R)를 수행하여 Layout을 생성하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.

## 타입

**Transform** — Synthesized Netlist를 입력으로 받아 Layout을 산출물로 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| Synthesized Netlist | PI Synthesis Service가 생성한 Netlist |
| 버전 | 사용할 Netlist 버전 |

## 출력

| 항목 | 설명 |
|---|---|
| Layout | P&R 결과 Layout 데이터 |
| P&R Report | Timing, DRC, LVS 등 P&R 결과 리포트 |

## FAIL 조건

- P&R 중 오류가 발생하여 Layout이 정상적으로 생성되지 않은 경우

## 실행 조건

- PI After Synthesis Service가 PASS로 완료된 경우

## 후속 작업

PD P&R 완료 후 **PD After P&R Service**를 수행하여야 한다.

## 관련 팀

- [PD 팀](../../../teams/PD.md)
