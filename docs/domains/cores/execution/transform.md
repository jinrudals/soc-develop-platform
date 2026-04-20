# Transform 서브도메인

## 개요

입력 버전(Artifact)을 받아 새로운 산출물 또는 새로운 버전을 생성하는 서브도메인이다.

**핵심 책임: 새로운 Baseline 후보를 만든다**

생성된 산출물은 Version / Baseline 도메인에 등록 요청되며, 다음 단계의 입력이 된다.
lineage(무엇을 입력으로 무엇을 만들었는가)가 핵심 정보다.

---

## 이 서브도메인이 답하는 질문

- 어떤 버전을 입력으로 무엇을 생성했는가?
- 생성된 산출물은 무엇인가?
- 입력과 출력 간 lineage는 어떻게 되는가?
- 생성이 정상적으로 완료됐는가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| TransformSpec | 어떤 입력으로 어떤 변환을 수행할지 정의한 명세 |
| TransformRun | TransformSpec 기반으로 실제 수행된 실행 단위 |
| InputArtifact | 변환의 입력이 되는 버전 또는 산출물 참조 |
| OutputArtifact | 변환 결과로 생성된 산출물 |
| OutputVersionCandidate | 생성된 산출물을 Version / Baseline 도메인에 등록 요청하기 위한 후보 |

---

## 서비스 목록

| 서비스 | 입력 | 출력 | 관련 팀 |
|---|---|---|---|
| IP Generation | IP Generator + conf | Generated IP RTL | 설계팀 |
| IP Onboarding | External IP | 사내 규칙에 맞게 정규화된 IP RTL | 설계팀 |
| RTL Integration | RTL Module conf + 하위 repo 버전 | Integrated RTL (SoC TOP) | RTL팀 |
| DFT Insertion | Functional RTL + DFT conf | DFTed RTL | DFT팀 |
| SDC | Functional / DFTed RTL + SDC conf | SDC 파일 | SDC팀 |
| PI Synthesis | RTL + SDC + synthesis conf | Synthesized Netlist | PI팀 |
| PD P&R | Synthesized Netlist + PD conf | Layout (GDS 등) | PD팀 |

---

## FAIL 조건

산출물이 정상적으로 생성되지 않은 경우 FAIL로 판단한다.
품질 판단(PASS/FAIL)은 Evaluation 서브도메인이 담당한다.

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| Version / Baseline | 생성된 산출물을 새 버전 후보로 등록 요청한다 |
| Evaluation | Transform 완료 후 결과물에 대한 QC를 Evaluation이 수행한다 |
| RTL | RTL Module / Hierarchy 정보를 실행 대상 결정에 참조한다 |
| SCM | Repository 정보를 입력 버전 결정에 참조한다 |
