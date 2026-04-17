# Inspection Domain

## 개요

각 팀의 PL(Project Leader)이 정의한 **Quality Policy**를 관리하고, Execution Service가 결과를 판단할 때 사용하는 기준을 제공하는 Domain이다.

Quality Policy는 단일 중앙 서비스로 관리되지 않는다.
각 Execution Service가 자신의 검사 항목(CheckItem)과 Quality Policy 테이블을 직접 소유하며, PL이 해당 서비스의 Policy를 설정한다.

## 핵심 개념

| 개념 | 설명 |
|---|---|
| CheckItem | 서비스별 평가 항목. 각 Execution Service 내부에 정의된다. |
| QualityPolicy | Project + MilestoneLevel + 대상 조합에 대해 PL이 정의한 PASS 판정 기준. 각 Execution Service 내부에 존재한다. |

## Quality Policy 위치

| Execution Service | Policy 소유 테이블 |
|---|---|
| RTL Quality Check | `QualityPolicy` (CriterionTarget 기준) |
| Verification | `QualityPolicy` (VerificationTarget 기준) |
| PI After Synthesis | `QualityPolicy` (RTLModule + CheckItem 기준) |
| PD After P&R | `QualityPolicy` (RTLModule + CheckItem 기준) |

## 캐싱

Quality Policy는 변경 빈도가 낮으므로 각 서비스에서 in-memory 캐싱으로 제공할 수 있다.
PL이 Policy를 수정하면 해당 캐시를 무효화한다.

## 연관 Domain

| Domain | 관계 |
|---|---|
| Execution Domain | 각 Check Service가 자신의 Quality Policy를 직접 조회하여 결과를 판단한다. |
| Project Domain | Policy 범위인 Project / MilestoneLevel / RTLModule 정보를 참조한다. |
