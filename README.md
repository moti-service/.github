# Moti

**모두의 여행, 모두를 위한 여행 서비스 - Moti**

Moti는 개인 맞춤형 여행 루트를 추천하고, 사용자의 여행 스타일에 따라 자유롭게 계획을 구성할 수 있는 플랫폼입니다. 본 조직(`organization`)의 모든 서비스는 확장성과 유지보수성을 고려한 구조를 목표로 합니다.

## 🛠 기술 스택 및 아키텍처

### 🔹 Frontend
- **Framework**: React Native
- **패키지 구성**: Vite
- **상태 관리**: Redux Toolkit, React Query
- **Router**: 
- **Styling**: styled-components

### 🔹 Backend
- **Framework**: Spring Boot
- **Core Stack**:
  - JPA (주요 CRUD 및 ORM 처리)
  - MyBatis (복잡 쿼리 및 튜닝이 필요한 곳에 사용)
  - QueryDSL
- **Service-to-Service 통신**:
  - Kafka (비동기 메시징 및 이벤트 기반 아키텍처)
- **인증 및 사용자 관리**:
  - OAuth2 (Google, Kakao 등 외부 소셜 로그인 연동 포함)
  - Redis (세션 및 인증 토큰 저장소로 사용)
- **API Gateway**: Spring Cloud Gateway
- **Configuration Management**: Spring Cloud Config 또는 Git 설정 중심 구성 서버

### 🔹 인프라 & 배포
- **Container Orchestration**: AWS EKS (Elastic Kubernetes Service)
- **CI/CD**: GitHub Actions + Jenkins
- **API 문서화**: Swagger / Springdoc OpenAPI
- **Database**: PostgreSQL or MySQL (확정된 DB 명시)
- **Redis**: 세션, 캐시, 인증 정보 저장

## 🌐 공통 관리 요소
- **로깅**: ELK(Stack) or CloudWatch
- **모니터링**: Prometheus + Grafana
- **에러 추적**: Sentry or New Relic

## 📦 기타 고려사항
- 사용자 유형별 권한 관리(Role Hierarchy)
- 여행 루트 추천 시 AI 도입 고려 중
- 프로젝트 간 통합 또는 공유 컴포넌트 개발을 위한 공통 모듈(repository) 구성
- 서비스별 Helm Chart 기반 배포 템플릿 구성 가능
- **Mobile 대응**: React Native

---

## 🔍 향후 보완 또는 논의가 필요한 부분

| 항목 | 확인 여부 | 비고 |
|------|----------|------|
| DB 확정 | ☐ | PostgreSQL? MySQL? H2 for dev? |
| 인증 프로세스 흐름 | ☐ | 로그인 → 토큰 발급 → Redis 저장 → 인증? |
| Kafka 주제 구조(topic) 설계 | ☐ | 예: user.signup, route.update |
| CI/CD 툴 | ☐ | GitHub Actions + ArgoCD로 진행할지 |
| 공통 서비스 분리 여부 | ☐ | 인증, 사용자, 알림 등 분리 예정 여부 |
| 메시지 큐 이외의 동기 통신 방식 | ☐ | FeignClient 또는 WebClient 병행 여부 |
| 모듈간 공통 DTO 또는 유틸리티 분리 여부 | ☐ | 공유 모듈 또는 패키지로 추출 고려 |

---

## 🧩 예시 서비스 구성도

```text
[Client]
   |
[API Gateway]
   ├── [Auth Service] --- Redis
   ├── [User Service] --- PostgreSQL
   ├── [Route Service] --- Kafka ↔ 다른 서비스
   ├── [Place Service] --- 외부 API 연동
   └── [Recommendation Service] (AI 기반)
```
