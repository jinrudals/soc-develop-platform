# Version Service — Data Flow

## 특성

Project Service와 달리 Version Service는 **시스템이 자동으로 업데이트**하는 서비스다.
사람이 직접 데이터를 입력하지 않으며, 외부 이벤트(GitLab webhook)와 내부 명령(Workflow)에 의해 갱신된다.

---

## 입력 (무엇이 Version Service를 갱신하는가)

| 입력 | 출처 | 처리 |
|---|---|---|
| tag 생성 webhook | GitLab | RepositoryTag 기록 |
| branch 생성 webhook | GitLab | 내부 branch 상태 갱신 |
| branch 생성 요청 | milestone-start Workflow | 해당 Repository에 Milestone branch 생성 후 GitLab에 반영 |

---

## 출력 (Version Service가 발행하는 이벤트)

tag 생성이 감지되면 이벤트를 발행한다. Execution 서비스들은 이 이벤트를 수신하여 실행을 트리거한다.

| 이벤트 | 설명 | 주요 수신 서비스 |
|---|---|---|
| `TagCreated` | 새로운 tag가 생성됨 | RTL Quality Check, Verification, DFT Insertion, SDC, PI Synthesis, PD P&R 등 |

> TagCreated 이벤트에는 `tag_type`, `repository_id`, `rtl_module_id`, `change_types` 등이 포함된다.
> 각 Execution 서비스는 자신이 관심 있는 `tag_type` / `change_types` 조합을 기준으로 트리거 여부를 판단한다.

---

## Query 대상

Version Service는 다른 서비스들이 **필요할 때 조회**하는 대상이기도 하다.

| 조회 주체 | 조회 내용 |
|---|---|
| Execution 서비스 | 특정 RTLModule의 최신 tag, tag 간 lineage |
| Workflow | 특정 Milestone의 branch 생성 여부, release tag 존재 여부 |
