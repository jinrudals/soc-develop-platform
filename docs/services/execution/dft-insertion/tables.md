# DFT Insertion Service — Tables

## 개요

Functional HPDF RTL Module의 stage tag 생성 또는 user conf 변경 시 DFT insertion을 수행한다.
conf는 변경될 때마다 새 row를 생성하는 immutable 구조로 관리하여, Execution이 어떤 conf로 수행됐는지 추적할 수 있다.
HPDF module 단위로 개별 실행되므로 batch 개념은 없다.

## 트리거 조건

| 조건 | 설명 |
|---|---|
| HPDF stage tag 생성 | HPDF RTL Module에 새로운 stage tag가 생성됨 |
| DFTConf 변경 | 해당 HPDF Module의 user conf가 변경됨 |

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| DFTConf | HPDF Module별 DFT 삽입 설정 (immutable 버전 관리) |
| Execution | DFT 삽입 실행 단위 |

---

## DFTConf

conf 변경 시 기존 row를 수정하지 않고 새 row를 생성한다.
Execution이 어떤 conf 버전으로 수행됐는지 추적할 수 있다.
conf 파일은 별도의 conf repository에서 관리되며, 특정 tag를 참조한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| tag_id | FK → RepositoryTag | conf가 위치한 repository의 특정 tag |
| created_at | timestamp | conf 변경 시 새 row 생성 |

---

## Execution

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| tag_id | FK → RepositoryTag | 실행 대상 stage tag |
| dft_conf_id | FK → DFTConf | 사용된 conf 버전 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | SUCCESS / FAILED (nullable) |
| output_rtl_module_id | FK → RTLModule (nullable) | 출력 대상 RTLModule. null이면 입력과 동일 |
| output_tag_id | FK → RepositoryTag (nullable) | 생성된 DFTed RTL tag |
| output_path | string | 생성된 DFTed RTL 경로 |
| executed_at | timestamp | |
