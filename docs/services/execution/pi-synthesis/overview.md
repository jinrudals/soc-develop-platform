# PI Synthesis Service

## 개요

Functional RTL 또는 DFTed RTL에 대해 Synthesis를 수행하여 Synthesized Netlist를 생성하는 Service이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.
PI 팀은 HPDF 단위로 업무를 진행한다.

## 타입

**Transform** — RTL을 입력으로 받아 Synthesized Netlist를 산출물로 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| RTL Module | Synthesis 대상 RTL Module (Functional RTL 또는 DFTed RTL), HPDF 단위 |
| 버전 | 사용할 RTL의 release tag |
| SDC | Synthesis용 타이밍 제약 파일 |

## 출력

| 항목 | 설명 |
|---|---|
| Synthesized Netlist | Synthesis 결과 Netlist |
| Synthesis Report | Timing, Area 등 Synthesis 결과 리포트 |

## FAIL 조건

- Synthesis 중 오류가 발생하여 Netlist가 정상적으로 생성되지 않은 경우

## 실행 조건

- Functional / DFTed RTL release tag가 생성된 경우
- SDC 작업이 완료된 경우

## 후속 작업

PI Synthesis 완료 후 **PI After Synthesis Service**를 수행하여야 한다.

## 관련 팀

- [PI 팀](../../../teams/PI.md)
- [SDC 팀](../../../teams/SDC.md)
