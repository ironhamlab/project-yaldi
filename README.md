# Yaldi

**AI 기반 ERD 설계 및 실시간 협업 플랫폼**

</br>

Yaldi는 GPT-4o를 활용한 Multi-Agent 시스템으로 ERD를 자동 생성하고, 팀원들과 실시간으로 협업하며 데이터베이스 스키마를 설계할 수 있는 웹 기반 플랫폼입니다.

</br>

---

## 목차

1. [주요 기능](#주요-기능)
2. [기술 스택](#기술-스택)
3. [시스템 아키텍처](#시스템-아키텍처)
4. [시작하기](#시작하기)
5. [프로젝트 구조](#프로젝트-구조)
6. [개발 환경 설정](#개발-환경-설정)
7. [배포](#배포)
8. [API 문서](#api-문서)
9. [모니터링](#모니터링)
10. [라이선스](#라이선스)

</br></br>

---

## 주요 기능

</br>

### 1. AI 기반 ERD 자동 생성
- **Multi-Agent System**: 5개의 전문 AI 에이전트가 협업하여 ERD 생성
  - Domain Analyst: 도메인 분석
  - Schema Architect: 스키마 설계
  - Validator: 검증
  - Optimizer: 최적화
  - Meta Agent: 전체 조율
- **GPT-4o 기반**: SSAFY GMS API를 활용한 고품질 ERD 생성
- **다양한 입력 지원**: 자연어 설명, 기존 SQL, 요구사항 문서

</br>

### 2. 실시간 협업
- **동시 편집**: 여러 사용자가 동시에 ERD를 편집
- **WebSocket 기반**: STOMP 프로토콜을 통한 실시간 동기화
- **실시간 알림**: SSE를 통한 이벤트 스트리밍
- **뷰어 모드**: 읽기 전용 실시간 뷰어 제공

</br>

### 3. 버전 관리
- **스냅샷 기반**: ERD 변경 이력을 스냅샷으로 저장
- **롤백 기능**: 이전 버전으로 복원 가능
- **버전 비교**: 버전 간 차이점 시각화
- **벡터 검색**: 유사한 ERD 버전 검색

</br>

### 4. AI 기반 상담 챗봇
- **컨텍스트 인식**: Graph RAG를 활용한 지능형 상담
- **전문 에이전트**: 도메인별 전문가 에이전트
- **의도 분류**: 사용자 질문 의도 자동 파악

</br>

### 5. 고급 검색
- **전문 검색**: Elasticsearch 기반 한글 형태소 분석
- **벡터 유사도 검색**: pgvector를 활용한 의미 기반 검색
- **그래프 기반 검색**: Neo4j Graph RAG

</br>

### 6. Mock 데이터 생성
- **AI 기반**: 스키마에 맞는 현실적인 테스트 데이터 자동 생성
- **다양한 데이터베이스 지원**: PostgreSQL, MySQL

</br>

### 7. SQL Export
- **다중 DB 지원**: PostgreSQL, MySQL DDL 생성
- **타입 매핑**: 데이터베이스별 최적화된 타입 변환
- **관계 정의**: Foreign Key, Index 자동 생성

</br></br>

---

## 기술 스택

### Front-End
| 카테고리 | 기술 | 버전 |
|---------|------|------|
| 프레임워크 | React | 19.1.1 |
| 언어 | TypeScript | 5.9.3 |
| 빌드 도구 | Vite | 7.1.7 |
| 상태 관리 | Zustand | 5.0.8 |
| 라우팅 | React Router DOM | 7.9.4 |
| 스타일링 | Tailwind CSS | 3.4.18 |
| HTTP 클라이언트 | Axios | 1.12.2 |
| 실시간 통신 | STOMP.js + SockJS | 7.2.1 / 1.6.1 |

</br>

### Back-End
| 카테고리 | 기술 | 버전 |
|---------|------|------|
| 프레임워크 | Spring Boot | 3.5.7 |
| 언어 | Java | 21 (LTS) |
| 보안 | Spring Security + JWT + OAuth2 | - |
| ORM | Spring Data JPA | - |
| 실시간 통신 | Spring WebSocket + STOMP | - |
| 메시지 브로커 | Redis Pub/Sub | - |
| 이벤트 스트리밍 | Apache Kafka | 3.8.1 |
| API 문서 | Springdoc OpenAPI | 2.8.4 |
| 파일 저장소 | AWS S3 | 2.20.0 |

</br>

### AI/ML
| 카테고리 | 기술 | 버전 |
|---------|------|------|
| 웹 프레임워크 | FastAPI | 0.109.0 |
| LLM | OpenAI GPT-4o | 1.10.0 |
| AI 프레임워크 | LangChain + LangGraph | 0.1.0+ / 0.0.26+ |
| 언어 | Python | 3.11 |

</br>

### Database
| 데이터베이스 | 버전 | 포트 | 용도 |
|-------------|------|------|------|
| PostgreSQL (pgvector) | 16 | 5432 | 메인 RDBMS + 벡터 검색 |
| Redis | 7-alpine | 6379 | 캐싱, 세션, Pub/Sub |
| Elasticsearch | 8.18.0 | 9200/9300 | 전문 검색 엔진 |
| Neo4j | 5.15 | 7474/7687 | Graph RAG |

</br>

### Infrastructure
- **Docker & Docker Compose**: 컨테이너 오케스트레이션
- **Prometheus + Grafana**: 모니터링 및 시각화
- **Nginx**: 리버스 프록시 및 정적 파일 서빙
- **Jenkins**: CI/CD 파이프라인

</br></br>

---

## 시스템 아키텍처

```
┌──────────────────────────────────────────────────────────────┐
│           Front-End (React 19 + TypeScript + Vite)            │
│   ┌──────────┬──────────┬──────────┬──────────┬──────────┐   │
│   │ Zustand  │ Tailwind │  React   │  Axios   │  STOMP   │   │
│   │  State   │   CSS    │  Router  │   HTTP   │   +SSE   │   │
│   └──────────┴──────────┴──────────┴──────────┴──────────┘   │
└────────────────────────┬─────────────────────────────────────┘
                         │ REST API / WebSocket / SSE
┌────────────────────────┴─────────────────────────────────────┐
│              Back-End (Spring Boot 3.5 + Java 21)             │
│  ┌──────────┬──────────┬──────────┬──────────┬──────────┐    │
│  │   JPA    │  Redis   │Elastic-  │WebSocket │  Kafka   │    │
│  │   ORM    │ Caching  │  search  │  STOMP   │ Streams  │    │
│  └──────────┴──────────┴──────────┴──────────┴──────────┘    │
│  ┌────────────────────────────────────────────────────────┐  │
│  │     Domain-Driven Design + Clean Architecture          │  │
│  │   (Domain / Infrastructure / Global Layers)            │  │
│  └────────────────────────────────────────────────────────┘  │
└─────┬────────┬─────────┬─────────┬──────────┬────────────────┘
      │        │         │         │          │
┌─────▼──┐ ┌──▼───┐ ┌───▼────┐ ┌──▼───┐  ┌──▼─────┐
│Postgre-│ │Redis │ │Elastic-│ │Kafka │  │   AI   │
│SQL     │ │      │ │search  │ │      │  │ FastAPI│
│+pgvector│ └──────┘ └────────┘ └──────┘  └────┬───┘
└────────┘                                      │
                                          ┌─────▼────┐
                                          │  Neo4j   │
                                          │ (Graph)  │
                                          └──────────┘
```


</br>

### 아키텍처 특징

#### 1. Clean Architecture + DDD
- **Domain Layer**: 핵심 비즈니스 로직 (User, Team, Project, ERD, Version 등)
- **Infrastructure Layer**: 외부 시스템 연동 (Database, Kafka, Redis, S3 등)
- **Global Layer**: 횡단 관심사 (Security, Exception, Logging 등)

#### 2. Event-Driven Architecture
- **Kafka**: 비동기 이벤트 스트리밍
- **Redis Pub/Sub**: WebSocket 메시지 브로드캐스팅
- **서비스 간 느슨한 결합**

#### 3. Multi-Agent AI System
- **LangGraph**: 그래프 기반 워크플로우 오케스트레이션
- **5개 전문 에이전트**: 각 단계별 최적화된 LLM 프롬프트
- **Graph RAG**: Neo4j 기반 지식 그래프 검색

#### 4. Real-time Communication
- **WebSocket + STOMP**: 양방향 실시간 통신
- **SSE**: 서버→클라이언트 단방향 스트리밍
- **Redis Pub/Sub**: 분산 환경에서 메시지 동기화

</br></br></br>

---

## 프로젝트 구조

```
project-yaldi/
├── Front-End/yaldi/          # React 프론트엔드
│   ├── src/
│   │   ├── apis/            # API 클라이언트
│   │   ├── components/      # React 컴포넌트
│   │   ├── hooks/           # 커스텀 훅 (15개)
│   │   ├── pages/           # 페이지 컴포넌트
│   │   ├── services/        # 비즈니스 로직
│   │   ├── stores/          # Zustand 상태 관리 (7개)
│   │   └── types/           # TypeScript 타입 정의
│   ├── package.json
│   └── vite.config.ts
│
├── Back-End/yaldi/           # Spring Boot 백엔드
│   ├── src/main/java/com/yaldi/
│   │   ├── domain/          # 도메인 레이어 (16개 도메인)
│   │   │   ├── auth/        # 인증/인가
│   │   │   ├── user/        # 사용자 관리
│   │   │   ├── team/        # 팀 관리
│   │   │   ├── project/     # 프로젝트 관리
│   │   │   ├── erd/         # ERD 핵심 도메인
│   │   │   ├── erd_generation/  # AI ERD 생성
│   │   │   ├── version/     # 버전 관리
│   │   │   ├── datamodel/   # 데이터 모델
│   │   │   ├── comment/     # 댓글
│   │   │   ├── notification/  # 알림
│   │   │   ├── consultation/  # AI 상담
│   │   │   ├── search/      # 검색
│   │   │   └── viewer/      # 뷰어
│   │   ├── infra/           # 인프라 레이어
│   │   │   ├── persistence/ # JPA 엔티티 및 레포지토리
│   │   │   ├── websocket/   # WebSocket 설정
│   │   │   ├── kafka/       # Kafka 이벤트 핸들러
│   │   │   ├── redis/       # Redis 설정
│   │   │   ├── elasticsearch/  # 검색 엔진
│   │   │   └── s3/          # 파일 저장소
│   │   └── global/          # 글로벌 레이어
│   │       ├── config/      # 스프링 설정
│   │       ├── security/    # 보안 설정
│   │       └── exception/   # 예외 처리
│   ├── build.gradle
│   └── Dockerfile
│
├── AI/yaldi/                 # FastAPI AI 서비스
│   ├── api/v1/              # API 엔드포인트
│   │   ├── erd_generation.py      # ERD 생성 API
│   │   ├── consultation.py        # 상담 챗봇 API
│   │   ├── mockdata.py            # Mock 데이터 생성
│   │   ├── search_embedding.py    # 검색 임베딩
│   │   └── version_embedding.py   # 버전 임베딩
│   ├── agents/              # Multi-Agent 시스템
│   │   ├── erd_generation/  # ERD 생성 에이전트
│   │   └── consultation/    # 상담 에이전트
│   ├── workflows/           # LangGraph 워크플로우
│   ├── prompts/             # LLM 프롬프트 템플릿
│   ├── rag/                 # RAG 구현
│   │   ├── graph_rag.py     # Neo4j Graph RAG
│   │   └── vector_search.py # pgvector 검색
│   ├── requirements.txt
│   └── Dockerfile
│
├── docs/                     # 문서
│   ├── tech-stack.md        # 기술 스택 상세
│   ├── ERD_SQL_Export_Guide.md  # SQL Export 가이드
│   └── API연동규격서_v1.3.0/    # API 명세서
│
├── exec/                     # 배포 관련
│   └── docker-compose.yml   # 전체 서비스 오케스트레이션
│
└── README.md                # 이 파일
```


</br></br>

---

## 주요 기능별 기술 스택

| 기능 | 기술 스택 |
|------|-----------|
| **AI ERD 생성** | GPT-4o + LangGraph + LangChain |
| **실시간 협업** | WebSocket (STOMP) + Redis Pub/Sub |
| **전문 검색** | Elasticsearch 8.18 + 한글 형태소 분석 |
| **벡터 검색** | PostgreSQL pgvector + OpenAI Embeddings |
| **Graph RAG** | Neo4j + APOC + Graph Data Science |
| **버전 관리** | PostgreSQL + Vector Similarity |
| **인증/인가** | Spring Security + JWT + OAuth2 |
| **이벤트 스트리밍** | Apache Kafka 3.8 (KRaft) |
| **캐싱** | Redis 7 + Spring Cache |
| **파일 저장** | AWS S3 |
| **모니터링** | Prometheus + Grafana |
| **API 문서** | Springdoc OpenAPI (Swagger) |


</br>

---

## 핵심 기술 특징

### 1. Multi-Agent AI System
```
User Input → Meta Agent → Domain Analyst → Schema Architect → Validator → Optimizer → Final ERD
                  ↓                                                              ↑
            LangGraph Workflow ────────────────────────────────────────────────┘
```

</br>

### 2. Graph RAG Architecture
```
User Query → Intent Classification → Vector Search (pgvector) → Graph Traversal (Neo4j)
                                                                        ↓
                                              Context Enrichment → LLM Response
```

</br>

### 3. Real-time Collaboration Flow
```
User A ──→ WebSocket (STOMP) ──→ Spring Boot ──→ Redis Pub/Sub ──→ WebSocket ──→ User B
                                       ↓
                                   PostgreSQL (영구 저장)
```

</br>

### 4. Event-Driven Architecture
```
ERD 변경 → Domain Event → Kafka Topic → Event Consumer → [Notification / Search Index / Version Snapshot]
```

</br></br></br>

---

## 데이터베이스 스키마

주요 도메인 엔티티:

- **users**: 사용자 정보
- **teams**: 팀/워크스페이스
- **projects**: 프로젝트
- **erds**: ERD 메타데이터
- **tables**: ERD 테이블
- **columns**: 테이블 컬럼
- **relationships**: 테이블 간 관계
- **versions**: ERD 버전 스냅샷
- **comments**: 협업 댓글
- **notifications**: 사용자 알림
- **erd_generation_requests**: AI 생성 요청 이력

Flyway 마이그레이션 스크립트는 `Back-End/yaldi/src/main/resources/db/migration/` 디렉토리에 있습니다.


</br></br></br>

---

## 보안

### 인증 방식
- **JWT Token**: Stateless 인증
- **Session Cookie**: 웹 UI 인증 (withCredentials: true)
- **OAuth2 Social Login**: Google, GitHub

</br>

### 보안 기능
- CORS 설정
- CSRF 보호
- XSS 방지 (Content Security Policy)
- SQL Injection 방지 (Prepared Statements)
- 비밀번호 암호화 (BCrypt)
- JWT 서명 검증
- API Rate Limiting

</br>

### 환경별 설정
- **개발 환경**: `application-dev.yml`
- **프로덕션 환경**: `application-prod.yml`
- **테스트 환경**: `application-test.yml`


</br></br></br>

---

## 성능 최적화

### Front-End
- **Code Splitting**: React Router 기반 페이지 단위 분할
- **Tree Shaking**: Vite 빌드 최적화
- **Lazy Loading**: React.Suspense
- **Image Optimization**: html-to-image 캐싱
- **Memoization**: React.memo, useMemo, useCallback

</br>

### Back-End
- **Database Connection Pooling**: HikariCP (최대 200 커넥션)
- **Query Optimization**: JPA N+1 문제 해결 (Fetch Join)
- **Caching**: Redis 캐싱 레이어
- **Index Optimization**: PostgreSQL 인덱스 전략
- **Async Processing**: Kafka 이벤트 비동기 처리

</br>

### AI Service
- **Connection Pooling**: PostgreSQL, MySQL 비동기 드라이버
- **LLM Caching**: 동일 프롬프트 결과 캐싱
- **Batch Processing**: 임베딩 배치 처리
- **Workers**: Uvicorn 멀티 워커 (2개)


