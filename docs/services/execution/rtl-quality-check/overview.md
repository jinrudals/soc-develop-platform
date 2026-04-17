# RTL Quality Check Service

## 개요

생성된 RTL Module에 대해 EDA tool 기반의 Quality Check를 수행하는 Service이다.
검증(Verification)이 아닌 **RTL 자체의 품질 확인** 영역이다.
EDA tool(Synopsys, Cadence 등)을 사용하여 수행한다.

## 타입

**Check** — RTL을 입력으로 받아 품질 검사 결과(PASS / FAIL)를 생성한다.

## 입력

| 항목 | 설명 |
|---|---|
| RTL Module | 검사 대상 RTL Module |
| 버전 | 검사 대상 stage tag 또는 release tag |

## 출력

| 항목 | 설명 |
|---|---|
| Check 결과 | 항목별 PASS / FAIL 및 상세 log |

## Check 항목

| 항목 | 설명 |
|---|---|
| Compile | RTL 컴파일 가능 여부 |
| RTL-LINT | 코딩 규칙 및 잠재적 오류 검사 |
| RTL-CDC | Clock Domain Crossing 검사 |
| RTL-RDC | Reset Domain Crossing 검사 |
| Synthesizable | 합성 가능 여부 확인 |

## FAIL 조건

Inspection Domain의 Quality Policy 기준에 따라 결정된다. (Project / Milestone별로 상이할 수 있음)

## 실행 조건

- RTL Repository에 새로운 stage tag가 생성된 경우
- DFT Insertion Service 완료 후 (DFTed RTL에 대한 재검사)

## 관련 팀

- [RTL 팀](../../../teams/RTL.md)
