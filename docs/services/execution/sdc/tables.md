# SDC Service — Tables

## 개요

Functional 또는 DFTed HPDF RTL Module을 입력으로 받아 SDC 타이밍 제약 파일을 생성한다.
HPDF 단위로 개별 실행된다.

## 트리거 조건

| 조건 | 설명 |
|---|---|
| Functional HPDF RTL release tag 생성 | Functional RTL이 확정된 경우 |
| DFTed HPDF RTL output tag 생성 | DFT Insertion 완료 후 |
| SDCConf 변경 | 해당 HPDF Module의 SDC 생성 설정이 변경됨 |

## 테이블 목록

| 테이블 | 설명 |
|---|---|
| SDCConf | HPDF Module별 SDC 생성 설정 (immutable 버전 관리) |
| Execution | SDC 생성 실행 단위 |

---

## SDCConf

conf 변경 시 새 row를 생성하는 immutable 구조. conf 변경도 Execution 트리거 조건이 된다.
conf 파일은 별도의 conf repository에서 관리되며, 특정 tag를 참조한다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| tag_id | FK → RepositoryTag | conf가 위치한 repository의 특정 tag |
| created_at | timestamp | |

---

## Execution

입력 tag는 Functional HPDF RTL tag 또는 DFT Insertion의 output_tag_id 모두 가능하다.
둘 다 RepositoryTag 참조이므로 `tag_id` 하나로 커버된다.

| 필드 | 타입 | 설명 |
|---|---|---|
| id | PK | |
| rtl_module_id | FK → RTLModule | 대상 HPDF Module |
| tag_id | FK → RepositoryTag | Functional 또는 DFTed HPDF RTL tag |
| sdc_conf_id | FK → SDCConf (nullable) | 사용된 conf 버전 |
| status | enum | PENDING / RUNNING / COMPLETED / FAILED |
| result | enum | SUCCESS / FAILED (nullable) |
| output_tag_id | FK → RepositoryTag (nullable) | 생성된 SDC 파일 세트의 tag |
| executed_at | timestamp | |
