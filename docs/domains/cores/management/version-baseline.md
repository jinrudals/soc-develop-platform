# Version / Baseline 관리 도메인

## 개요

공식 버전(Baseline)과 Repository 간 버전 의존성을 관리하는 도메인이다.
원래는 사람이 수동으로 추적하던 영역이며, 이 플랫폼이 자동화하는 핵심 가치 중 하나다.

**주 관리자: 시스템 (자동)**

---

## 이 도메인이 답하는 질문

- 현재 어떤 tag가 공식 기준선(Baseline)인가?
- stage / main / release 의 관계는 어떻게 되는가?
- 상위 Repository는 하위 Repository의 어떤 버전을 참조하는가?
- 어떤 변경이 이 tag에 포함됐는가?

---

## 핵심 개념

| 개념 | 설명 |
|---|---|
| RepositoryTag | Repository의 특정 시점을 가리키는 tag. 공식 기준선의 단위이다. |
| TagType | tag의 역할 구분 (stage / main / release). stage → main → release 순으로 승격된다. |
| RepositoryTagChange | tag 생성 또는 승격 이력. 어떤 변경이 포함됐는지 추적한다. |
| RepositoryTagChangeType | 변경의 종류 구분 (신규 생성 / 승격 / 롤백 등). |
| VersionDependency | 상위 Repository가 참조하는 하위 Repository의 버전 정보. branch(stage / main / release)별로 관리된다. |

---

## Tag 승격 흐름

```
develop branch
    │
    │ tag 생성
    ▼
Stage Tag ──────────────────────────────► 검증 / QC 시작 기준
    │
    │ 검증 완료 후 승격
    ▼
Main Tag ───────────────────────────────► 팀 내 릴리즈 기준
    │
    │ 최종 확정 후 승격
    ▼
Release Tag ────────────────────────────► 공식 Baseline (타 팀 참조 기준)
```

---

## 자동화 가치

이 도메인이 관리하는 정보는 원래 설계자가 수동으로 문서나 스프레드시트에 기록하던 것이다.
플랫폼은 Git webhook과 CI/CD 이벤트를 수신하여 이를 자동으로 추적하고,
다른 도메인(Execution, Workflow)이 이 정보를 기준으로 동작할 수 있도록 제공한다.

---

## 연관 도메인

| 도메인 | 관계 |
|---|---|
| SCM | Repository와 branch 정책 정보를 참조한다. |
| RTL | RTL Module 단위로 버전 의존성을 해석하는 데 참조한다. |
| Execution | tag 생성 이벤트를 수신하여 실행 대상 버전을 결정한다. |
| Workflow | tag 생성 / 승격 이벤트를 수신하여 다음 단계 진입 여부를 판단한다. |
