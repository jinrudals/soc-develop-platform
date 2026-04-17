# SoC Develop Platform — Architecture

## 개요

플랫폼 전체 구성을 개념 수준에서 기술한다. 구체적인 구현 기술(tool, vendor)은 명시하지 않는다.

---

## 전체 구성도

```
┌─────────────────────────────────────────────────────────────────────┐
│                          Web Browser                                │
│                      (SPA / Single Page App)                        │
└─────────────────────────────┬───────────────────────────────────────┘
                              │ HTTPS
┌─────────────────────────────▼───────────────────────────────────────┐
│                       Edge Layer                                    │
│                                                                     │
│   ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐  │
│   │  L7 Load        │   │  Reverse        │   │  API            │  │
│   │  Balancer       │──▶│  Proxy          │──▶│  Gateway        │  │
│   │                 │   │                 │   │                 │  │
│   │ · traffic       │   │ · TLS           │   │ · 토큰 검증     │  │
│   │   분산          │   │   termination   │   │ · 라우팅        │  │
│   │ · health check  │   │ · 요청 정규화   │   │ · rate limit    │  │
│   └─────────────────┘   └─────────────────┘   └────────┬────────┘  │
└──────────────────────────────────────────────────────── │ ──────────┘
                                                          │
              ┌───────────────────────────────────────────┤
              │                                           │
┌─────────────▼──────────┐           ┌────────────────────▼──────────┐
│   SSO / Identity       │           │       Service Layer            │
│   Provider             │           │                                │
│                        │           │  (각 Domain Service들)         │
│ · 사용자 인증          │◀─────────▶│                                │
│ · 토큰 발급 (JWT)      │  token     │  서비스는 토큰을 직접 검증하지│
│ · Single Sign-On       │  검증 위임 │  않고 API Gateway에 위임한다  │
└────────────────────────┘           └────────────────────┬──────────┘
                                                          │
                                     ┌────────────────────▼──────────┐
                                     │     Event Broker / MQ          │
                                     │                                │
                                     │ · 서비스 간 비동기 통신        │
                                     │ · 도메인 이벤트 Pub / Sub      │
                                     └────────────────────┬──────────┘
                                                          │
                                     ┌────────────────────▼──────────┐
                                     │    외부 실행 환경              │
                                     │    (Job Runner 경유)           │
                                     │                                │
                                     │  · EDA Tool 실행 환경          │
                                     │  · 파이프라인 오케스트레이터   │
                                     └───────────────────────────────┘
```

---

## Edge Layer

세 가지 역할은 논리적으로 구분되며, 물리적으로는 같은 컴포넌트가 담당할 수 있다.

| 역할 | 책임 |
|---|---|
| **L7 Load Balancer** | 인입 트래픽을 여러 인스턴스로 분산. health check 기반 장애 인스턴스 제외 |
| **Reverse Proxy** | TLS termination, 요청 정규화, 정적 파일 서빙. 내부 서비스 구조를 클라이언트로부터 은닉 |
| **API Gateway** | 토큰 유효성 검증, 서비스 라우팅, rate limiting. 인증 실패 시 SSO로 리다이렉트 |

---

## SSO / Identity Provider

플랫폼 전체의 인증을 단일 지점에서 처리한다.

- 사용자는 플랫폼 최초 접근 시 Identity Provider로 리다이렉트되어 인증을 수행한다.
- 인증 성공 시 JWT 토큰을 발급받고, 이후 모든 API 요청에 토큰을 첨부한다.
- API Gateway가 토큰 유효성 검증을 담당하며, 각 Service는 토큰 검증 로직을 보유하지 않는다.
- 권한(role / permission) 관리는 플랫폼 내 Project Service에서 담당한다. (SSO는 인증만)

---

## Service Layer

Domain 단위로 Service를 구성한다. 각 Service는 독립적으로 배포 가능하며 자체 DB를 가진다.

```
┌──────────────────────────────────────────────────────────────┐
│                        Service Layer                         │
│                                                              │
│  ┌──────────────┐  ┌──────────────┐                         │
│  │  Project     │  │  Version     │  ← Informative Domain   │
│  │  Service     │  │  Service     │    (사용자가 직접 관리)  │
│  └──────────────┘  └──────────────┘                         │
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  RTL Quality │  │ Verification │  │ DFT Insertion│       │
│  │  Check       │  │              │  │              │  ...  │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  SDC         │  │ PI Synthesis │  │ PD P&R       │  ...  │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│                              ← Execution Domain             │
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  Job Runner  │  │  Workflow    │  │  Inspection  │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│  ← Infrastructure  ← Workflow Domain  ← Inspection Domain   │
└──────────────────────────────────────────────────────────────┘
```

---

## Event Broker / Message Queue

서비스 간 직접 호출을 최소화하고 이벤트 기반 비동기 통신을 사용한다.

| 이벤트 흐름 | 설명 |
|---|---|
| Version Service → Execution Services | tag 생성 이벤트를 수신하여 실행 트리거 결정 |
| Execution Services → Job Runner | 실행 요청을 비동기로 전달 |
| Job Runner → Execution Services | 실행 상태 변경 및 raw 결과 콜백 |
| Execution Services → Workflow Service | 실행 완료 이벤트 전달 |
| Workflow Service → Execution Services | 다음 단계 실행 지시 |

동기 호출(REST / gRPC)은 조회성 요청(Quality Policy 조회, RTLModule 정보 조회 등)에만 사용한다.

---

## Frontend (SPA)

- 단일 페이지 애플리케이션으로 구성한다.
- API Gateway를 통해 모든 Service와 통신한다.
- SSO 인증 흐름은 브라우저 리다이렉트 방식으로 처리한다.
- 주요 화면:

| 영역 | 설명 |
|---|---|
| Project 관리 | Project / Milestone / Repository / RTLModule 등록 및 조회 |
| 실행 현황 | 각 Execution Service의 실행 상태 및 결과 조회 |
| 품질 현황 | Inspection 결과, Quality Policy 기준 대비 현황 |
| Workflow | Milestone 진행 상태 및 이벤트 흐름 시각화 |

---

## 외부 실행 환경

EDA Tool 실행은 플랫폼 내부에서 직접 수행하지 않는다.
모든 실행 요청은 **Job Runner Service**를 통해 외부 실행 환경(파이프라인 오케스트레이터 / CI 서버)으로 위임된다.

```
Execution Service
      │
      │ 실행 요청 (job_type, payload)
      ▼
Job Runner Service
      │
      │ job 호출
      ▼
파이프라인 오케스트레이터 / CI 서버
      │
      │ EDA Tool 실행
      ▼
EDA Tool (Synthesis, P&R, DRC 등)
      │
      │ 결과 콜백
      ▼
Job Runner Service → Execution Service
```
