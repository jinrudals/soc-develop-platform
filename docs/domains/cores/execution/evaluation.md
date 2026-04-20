# Evaluation 서브도메인

## 개요

입력 버전(Artifact)을 받아 검사 / 검증 / 시뮬레이션을 수행하고, 결과를 report하는 서브도메인이다.
원본 산출물을 변경하지 않는다.

**핵심 책임: 기존 Baseline 후보를 평가한다**

Quality Policy 기준에 따라 PASS / FAIL을 판단하며, 결과는 다음 의사결정의 근거가 된다.

---

## 이 서브도메인이 답하는 질문

- 이 버전은 품질 기준을 충족하는가?
- 어떤 항목이 PASS / FAIL 됐는가?
- 검증 결과의 근거(Evidence)는 무엇인가?
- 어떤 Policy 기준으로 판단했는가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| EvaluationSpec | 어떤 입력에 대해 어떤 검사를 수행할지 정의한 명세 |
| EvaluationRun | EvaluationSpec 기반으로 실제 수행된 실행 단위 |
| CheckItem | 서비스별 평가 항목 |
| QualityPolicy | Project + MilestoneLevel + 대상 조합에 대해 PL이 정의한 PASS 판정 기준 |
| CheckResult | 각 CheckItem에 대한 PASS / FAIL 결과 |
| Evidence | 결과의 근거가 되는 로그, 수치, 파형 등 |
| Report | EvaluationRun 전체 결과를 정리한 최종 보고서 |

---

## 서비스 목록

| 서비스 | 검사 대상 | 주요 검사 항목 | 관련 팀 |
|---|---|---|---|
| RTL Quality Check | RTL Module | Compile / RTL-LINT / CDC·RDC / Synthesizable 여부 | RTL팀 |
| Verification | Functional RTL / DFTed RTL | 기능 / 전력 / 성능 / DFT 검증 (pre-GLS / post-GLS 포함) | Verification팀 |
| PI QC | Synthesized Netlist | Synthesis 결과 품질 검사 | PI팀 |
| PD QC | Layout (P&R 결과) | P&R 결과 품질 검사 / DRC / LVS | PD팀 |

---

## FAIL 조건

Quality Policy 기준을 충족하지 못한 경우 FAIL로 판단한다.
Policy는 Project + MilestoneLevel + 검사 대상 조합으로 PL이 사전 정의한다.

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| Version / Baseline | 평가 대상 버전 정보를 참조한다 |
| Transform | Transform 완료 후 결과물을 평가하는 순서로 연결된다 |
| Project | Quality Policy의 범위인 Project / MilestoneLevel 정보를 참조한다 |
