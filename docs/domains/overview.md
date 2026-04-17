# Domain Overview

SoC Develop Platform의 전체 Domain 구조를 정의한다.

## Domain Map

```
┌─────────────────────────────────────────────────────┐
│  Project Domain                                     │
│  프로젝트, Repository, RTL Module, 인원 구성 (정적)    │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  Version Domain                                     │
│  branch 상태, tag 이력, repo 간 버전 의존성 (동적)      │
│                                    ↓ 이벤트 발행      │
└─────────────────────────────────────────────────────┘
                                     │
              ┌──────────────────────┤
              ↓                      ↓
┌──────────────────────┐   ┌──────────────────────────┐
│  Execution Domain    │   │  Workflow Domain          │
│  팀별 실행 Service    │   │  이벤트 → 후속 액션        │
│  (RTL QC, Verif ...) │   │  (CI/Automation Runner   │
│  ↓ Jenkins 요청       │   │   기반, 기능별 독립)       │
└──────────────────────┘   └──────────────────────────┘
              │
              ↓
┌─────────────────────────────────────────────────────┐
│  Job Runner Domain                                  │
│  Jenkins / Airflow 어댑터                             │
│  실행 상태(pending/running/completed) 및 결과 반환     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  Inspection Domain                                  │
│  Quality Policy 테이블 소유 및 판단 기준 제공 API       │
│  (Project + Milestone + Module + Check 항목 → 기준)   │
│  Redis 캐싱                                          │
└─────────────────────────────────────────────────────┘
```

## Domain 목록

### Core Domains

| Domain | 문서 | 설명 |
|---|---|---|
| Project | [cores/project.md](cores/project.md) | 프로젝트 구성 정보 (정적) |
| Version | [cores/version.md](cores/version.md) | branch/tag 상태 및 repo 간 의존성 (동적) |
| Execution | [cores/execution.md](cores/execution.md) | 팀별 실행 Service 집합 |
| Job Runner | [cores/job-runner.md](cores/job-runner.md) | Jenkins / Airflow 어댑터 |
| Inspection | [cores/inspection.md](cores/inspection.md) | Quality Policy 관리 및 판단 기준 제공 |
| Workflow | [cores/workflow.md](cores/workflow.md) | cross-team 실행 순서 및 조건 관리 |

### Utility Domains

추후 추가 예정. 예: Notification Domain 등.

## 관련 문서

- [팀 역할 정의](../teams/)
- [서비스 정의](../services/)
