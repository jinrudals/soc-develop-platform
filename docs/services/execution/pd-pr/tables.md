# PD P&R Service — Tables

## 개요

PI After Synthesis를 통과한 Synthesized Netlist를 입력으로 받아 Place-and-Route를 수행하고 Layout을 생성한다.
결과물이 매우 크므로 git symlink 방식으로 경로만 참조한다.
HPDF 단위로 개별 실행된다.

## 트리거 조건

| 조건 | 설명 |
|---|---|
| PI After Synthesis Execution 완료 (PASS) | Netlist 검사가 통과된 경우 |
| PDConf 변경 | 해당 HPDF Module의 P&R 설정이 변경됨 |

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| PDConf | HPDF Module별 P&R 설정 (immutable 버전 관리) |
| Execution | PD P&R 실행 단위 |

---

## PDConf

conf 변경 시 새 row를 생성하는 immutable 구조.
conf 파일은 별도의 conf repository에서 관리되며, 특정 tag를 참조한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| tag_id | FK → RepositoryTag | conf가 위치한 repository의 특정 tag |
| created_at | timestamp | |

---

## Execution

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| pi_after_synthesis_execution_id | FK → PI After Synthesis Execution | 검사 통과된 Netlist의 출처 |
| pd_conf_id | FK → PDConf (nullable) | 사용된 conf 버전 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | SUCCESS / FAILED (nullable) |
| output_path | string | Layout symlink 경로 |
| executed_at | timestamp | |
