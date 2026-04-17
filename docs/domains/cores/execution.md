# Execution Domain

## 개요

각 팀의 작업을 Service 단위로 정의하고, 실행을 요청하며, 결과를 수집·판단하는 Domain이다.
Execution Domain은 단일 서비스가 아니라 **팀/작업 단위의 여러 Service로 구성**된다.

각 Service는 다음 책임을 가진다.

1. **실행 대상 정의** — 어떤 Project / RTL Module에 대해 무엇을 실행해야 하는지 결정한다.
2. **실행 요청** — Job Runner Domain에 Jenkins job 실행을 요청한다.
3. **상태 수신** — Job Runner로부터 실행 상태(`PENDING` / `RUNNING` / `COMPLETED`)를 수신한다.
4. **결과 수집** — Job Runner로부터 raw 결과(log, 수치 데이터 등)를 수신한다.
5. **결과 판단** — Inspection Domain에서 해당 Project + Milestone 기준의 Quality Policy를 조회하여 `PASS` / `FAIL`을 결정한다.
6. **결과 저장** — 판단 결과 및 log를 저장한다.

## Service 타입

Execution Service는 두 가지 타입으로 구분된다.

| 타입 | 설명 | FAIL 조건 |
|---|---|---|
| **Transform** | 입력을 처리하여 새로운 산출물(RTL, Netlist 등)을 생성한다. | 산출물이 정상적으로 생성되지 않은 경우 |
| **Check** | 입력을 검사하여 결과(PASS / FAIL)를 생성한다. 새로운 산출물은 만들지 않는다. | Quality Policy 기준을 충족하지 못한 경우 |

## Service 목록

| Service | 타입 | 설명 | 관련 팀 |
|---|---|---|---|
| RTL Integration Service | Transform | engineer의 conf를 기반으로 RTL 자동 생성 | [RTL 팀](../../teams/RTL.md) |
| RTL Quality Check Service | Check | RTL Module에 대한 compile, RTL-LINT, RTL-CDC/RDC, synthesizable 검사 | [RTL 팀](../../teams/RTL.md) |
| Verification Service | Check | Functional / DFTed RTL에 대한 기능 / 전력 / 성능 검증 | [Verification 팀](../../teams/Verification.md) |
| DFT Insertion Service | Transform | Functional RTL + conf를 기반으로 DFTed RTL 생성. 완료 후 RTL Quality Check 재수행 필요 | [DFT 팀](../../teams/DFT.md) |
| SDC Service | Transform | Functional / DFTed RTL에 대한 SDC 타이밍 제약 파일 생성 | [SDC 팀](../../teams/SDC.md) |
| PI Synthesis Service | Transform | Functional / DFTed RTL에 대한 Synthesis 수행, Netlist 생성 | [PI 팀](../../teams/PI.md) |
| PI After Synthesis Service | Check | Synthesis 결과물에 대한 검사 | [PI 팀](../../teams/PI.md) |
| PD P&R Service | Transform | Synthesized Netlist에 대한 Place-and-Route 수행, Layout 생성 | [PD 팀](../../teams/PD.md) |
| PD After P&R Service | Check | P&R 결과물에 대한 검사 | [PD 팀](../../teams/PD.md) |

각 Service의 상세 정의는 [docs/services/execution/](../../services/execution/) 에서 관리한다.

## 연관 Domain

| Domain | 관계 |
|---|---|
| Version Domain | 이벤트를 수신하여 실행 시점과 대상 버전을 결정한다. |
| Workflow Domain | 이벤트를 수신하여 실행 시점을 결정하고, 실행 완료 후 결과를 전달한다. |
| Job Runner Domain | Jenkins job 실행 요청을 전달하고, 상태 및 raw 결과를 수신한다. |
| Inspection Domain | Quality Policy를 조회하여 결과 판단 기준을 얻는다. |
| Project Domain | 실행 대상인 RTL Module / Repository 정보를 참조한다. |
