# Execution 도메인 Overview

SoC 개발 과정에서 각 팀이 수행하는 작업을 서비스 단위로 정의하고 실행하는 도메인이다.
원래 각 팀이 수동으로 수행하던 공정들을 플랫폼이 자동화·추적하는 영역이다.

---

## 서브도메인 구성

| 서브도메인 | 핵심 책임 | 문서 |
|---|---|---|
| [Transform](transform.md) | 새로운 Baseline 후보를 만든다 | [transform.md](transform.md) |
| [Evaluation](evaluation.md) | 기존 Baseline 후보를 평가한다 | [evaluation.md](evaluation.md) |

---

## Transform vs Evaluation

```
                  입력 버전
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
    Transform                Evaluation
    새 산출물 생성             검사 / 검증 수행
    새 버전 후보 등록          PASS / FAIL 판단
    lineage 연결              Report / Evidence 생성
          │                       │
          ▼                       ▼
  Version / Baseline         의사결정 근거 제공
  (다음 단계 입력)
```

| 구분 | Transform | Evaluation |
|---|---|---|
| 결과물 | 새 Artifact / 새 버전 | Report / CheckResult |
| 원본 변경 | O | X |
| FAIL 기준 | 산출물 생성 실패 | Quality Policy 미충족 |
| 다음 단계 | 생성물이 다음 입력이 됨 | 판단 결과가 의사결정에 반영됨 |

---

## Execution Control (공통 실행 기반)

Transform과 Evaluation 모두 공통적으로 아래 실행 생명주기를 공유한다.

```
요청 접수 → Run ID 발급 → PENDING → RUNNING → SUCCEEDED / FAILED
                                                      │
                                               결과 / Report 저장
```

| 개념 | 설명 |
|---|---|
| Run | 실행의 단위. Transform / Evaluation 모두 Run을 가진다 |
| RunStatus | PENDING / RUNNING / SUCCEEDED / FAILED / CANCELLED |
| InputReference | 실행에 사용된 입력 버전 / Artifact 참조 |
| OutputReference | 실행 후 생성된 산출물 참조 (Transform만 해당) |
| ReportReference | 실행 결과 Report 참조 (Evaluation만 해당) |

실제 실행(Jenkins / EDA / CLI 호출)은 Tool Adapter (Infrastructure)가 담당한다.
Execution 도메인은 "무엇을 실행할지"만 정의하며, "어떻게 실행할지"는 알지 않는다.

---

## Tool Adapter (Infrastructure)

Execution 도메인과 외부 실행 도구 사이를 연결하는 인프라 계층이다.
도메인이 아니므로 별도 도메인 문서가 아닌 인프라 문서로 관리한다.

```
Transform / Evaluation
        │ 실행 요청 (무엇을 실행할지)
        ▼
  Tool Adapter
        │ 번역 (어떻게 실행할지)
        ▼
Jenkins / EDA CLI / 사내 Tool
```

Jenkins를 다른 실행 환경으로 교체해도 Execution 도메인 코드는 변경되지 않는다.

---

## 서비스 전체 목록

### Transform 계열

| 서비스 | 관련 팀 |
|---|---|
| IP Generation | 설계팀 |
| IP Onboarding | 설계팀 |
| RTL Integration | RTL팀 |
| DFT Insertion | DFT팀 |
| SDC | SDC팀 |
| PI Synthesis | PI팀 |
| PD P&R | PD팀 |

### Evaluation 계열

| 서비스 | 관련 팀 |
|---|---|
| RTL Quality Check | RTL팀 |
| Verification | Verification팀 |
| PI QC | PI팀 |
| PD QC | PD팀 |

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| Version / Baseline | tag 이벤트를 수신하여 실행 대상 버전을 결정. Transform 완료 시 새 버전 후보 등록 요청 |
| Workflow | 실행 완료 이벤트를 수신하여 다음 단계 진입 여부를 판단 |
| RTL | RTL Module / Hierarchy 정보를 실행 대상 결정에 참조 |
| Project | Project / MilestoneLevel 정보를 Quality Policy 범위 결정에 참조 |
