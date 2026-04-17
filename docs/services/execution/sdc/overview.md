# SDC Service

## 개요

Functional 또는 DFTed HPDF RTL Module을 입력으로 받아 Synthesis를 위한 타이밍 제약(SDC: Synopsys Design Constraints) 파일을 생성한다.
HPDF 단위로 개별 실행된다.

## 타입

**Transform** — RTL Module을 입력으로 받아 SDC 파일 세트를 생성하고 결과 tag를 반환한다.

## 입력

| 항목 | 설명 |
|---|---|
| RTL Module tag | Functional 또는 DFTed HPDF RTL tag |
| SDCConf | SDC 생성 설정 (conf repository의 특정 tag) |

## 출력

| 항목 | 설명 |
|---|---|
| SDC output tag | 생성된 SDC 파일 세트가 저장된 repository의 tag |

## FAIL 조건

SDC 파일이 정상적으로 생성되지 않은 경우 (Transform 타입)

## 실행 조건

- Functional HPDF RTL release tag 생성
- DFTed HPDF RTL output tag 생성
- SDCConf 변경

## 관련 팀

- [SDC 팀](../../../teams/SDC.md)
