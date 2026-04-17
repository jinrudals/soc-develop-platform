# SoC DFT 팀

## 개요

SoC DFT(Design For Test) 팀은 SoC Functional RTL에 DFT 로직을 삽입하는 업무를 수행한다.

DFT 업무는 프로젝트의 진행 단계에 따라 수행 여부와 범위가 달라진다.

| 프로젝트 단계 | DFT 업무 현황 |
|---|---|
| 초기 단계 | DFT 업무가 없을 수 있음 |
| 중간 단계 | Functional RTL 변경과 함께 DFT 업무 병행 |
| 말기 단계 | Functional RTL 고정 후 DFT RTL 변경만 진행될 수 있음 |

---

## 관리 대상

- 어떤 RTL Module의 어떤 버전(`stage`, `main`, 또는 release)을 기준으로 DFT를 진행하는가
- DFT 진행도는 어떻게 되는가
