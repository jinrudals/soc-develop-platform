# SoC Develop Platform

SoC Develop Platform은 SoC Fabless, DSP, 또는 IP Vendor 와 같은 회사들이 프로젝트의 현황을 파악하고,
업무를 사람 중심에서 시스템 중심으로 전환하는 것을 목표로 하는 플랫폼이다.

사람 중심 운영 방식에서는 개발자의 작업 환경과 툴 버전에 종속되며, 진행 상황을 개발자의 보고에만 의존하게 되는 문제가 발생한다.
SoC Develop Platform은 이러한 의존성을 시스템으로 대체하여 객관적이고 일관된 프로젝트 관리를 가능하게 한다.

## 문서 구조

| 경로 | 설명 |
|---|---|
| `docs/teams/*.md` | 각 팀의 역할 및 업무 정의 |
| `docs/domains/*.md` | DDD Domain 정의 |
| `docs/services/<domain>/*.md` | 각 Domain의 Service 정의 |

## Domain 구조

[docs/domains/overview.md](docs/domains/overview.md) 참고.

| Domain | 문서 |
|---|---|
| Project | [docs/domains/project.md](docs/domains/project.md) |
| Version | [docs/domains/version.md](docs/domains/version.md) |
| Job Runner | [docs/domains/job-runner.md](docs/domains/job-runner.md) |
| Execution | [docs/domains/execution.md](docs/domains/execution.md) |
| Inspection | [docs/domains/inspection.md](docs/domains/inspection.md) |
| Workflow | [docs/domains/workflow.md](docs/domains/workflow.md) |

## 관련 팀

| 팀 | 문서 |
|---|---|
| PM 팀 | [docs/teams/PM.md](docs/teams/PM.md) |
| RTL 설계팀 | [docs/teams/RTL.md](docs/teams/RTL.md) |
| Verification 팀 | [docs/teams/Verification.md](docs/teams/Verification.md) |
| DFT 팀 | [docs/teams/DFT.md](docs/teams/DFT.md) |
| SDC 팀 | [docs/teams/SDC.md](docs/teams/SDC.md) |
| PI 팀 | [docs/teams/PI.md](docs/teams/PI.md) |
| PD 팀 | [docs/teams/PD.md](docs/teams/PD.md) |
