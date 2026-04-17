# SoC Verification 팀

## 개요

SoC Verification 팀은 SoC 프로젝트의 Functional RTL 및 DFTed RTL을 검증하기 위한 환경 구성, 검증 수행, 디버깅 업무를 담당한다.

검증 항목으로는 **Functional 검증**, **Power 검증**, **Performance 검증** 등이 포함된다.

---

## 검증 수행 시점

검증은 RTL Module의 성숙도에 따라 다음 시점에 수행한다.

| 대상 | 수행 시점 |
|---|---|
| IP / HPDF / Subsystem Level | `stage` 단계에서 수행 |
| SoC Top Level | RTL release 완료 후 수행 |

---

## 관리 대상

- 어떤 RTL Module의 어떤 버전(`stage`, `main`, 또는 release)을 기준으로 검증을 수행하는가
- 검증 환경 구성 현황은 어떻게 되는가
- 검증 진행도 및 결과는 어떻게 되는가
