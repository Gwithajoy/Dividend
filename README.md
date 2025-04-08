# 💰 Dividend Info Service (배당금 정보 서비스)

Spring Boot 기반의 배당금 정보 수집 및 제공 API 서버입니다.  
사용자는 회사명으로 배당금 데이터를 조회할 수 있고, 관리자는 기업을 등록해 배당 데이터를 저장할 수 있습니다.  
Yahoo Finance 웹사이트를 크롤링해 데이터를 수집하고, Redis 캐시를 활용하여 성능을 향상시켰습니다.

---

## 🧩 주요 기능

- 📈 회사별 배당금 정보 조회 (`/finance/dividend/{companyName}`)
- 🏢 기업 등록/삭제 및 자동완성 기능 (`/company`)
- 🧠 Redis 캐싱 + 스케줄링으로 주기적 배당 데이터 업데이트
- 🛡️ JWT 기반 회원가입 및 로그인 (`/signup`, `/signin`)
- 🔐 Spring Security + Role 기반 권한 관리 (`READ`, `WRITE`)
- 🧪 웹 크롤링 기반 Yahoo Finance 배당금 수집

---

## ⚙️ 기술 스택

| 영역 | 기술 |
|------|------|
| Language | Java 17 |
| Framework | Spring Boot 3.x |
| DB | H2 / MySQL |
| ORM | Spring Data JPA |
| Security | Spring Security + JWT |
| Caching | Redis |
| Scheduler | Spring Scheduling (`@Scheduled`) |
| Web Crawling | JSoup |
| Build Tool | Gradle

---

## 📂 주요 패키지 구조

```plaintext
com.example.dividend
 ├── web            # REST API 컨트롤러
 ├── service        # 비즈니스 로직
 ├── model          # DTO, Entity, 기타 모델
 ├── persist        # Repository 및 Entity 클래스
 ├── scheduler      # 배당 데이터 수집 스케줄러
 ├── scraper        # Yahoo Finance 크롤링
 ├── security       # JWT 토큰 발급 및 인증 필터
 └── exception      # 커스텀 예외 정의
```

---

## 🔐 JWT 인증 방식

- 로그인 성공 시 서버가 JWT 토큰을 발급
- 이후 요청 시 `Authorization: Bearer <JWT>` 헤더에 포함
- 사용자 권한에 따라 API 접근 제어 (`@PreAuthorize("hasRole('READ')")` 등)

---

## 🧪 스케줄링 기반 배당금 수집

- `@Scheduled(cron = "...")` 으로 설정된 메서드가 Yahoo Finance에서 주기적으로 데이터 수집
- 중복 데이터 방지 처리 포함
- 크롤링 간 3초 지연 (`Thread.sleep(3000)`)으로 API 차단 회피

---

## 🚀 API 요약

### 📊 배당 정보 조회

| 메서드 | 경로 | 설명 |
|--------|------|------|
| GET | `/finance/dividend/{companyName}` | 회사명으로 배당 정보 조회 |

### 🏢 회사 관리

| 메서드 | 경로 | 설명 |
|--------|------|------|
| POST | `/company` | 회사 등록 |
| DELETE | `/company/{ticker}` | 회사 삭제 |
| GET | `/company` | 모든 회사 조회 (페이징) |
| GET | `/company/autocomplete?keyword=` | 자동완성 키워드 조회 |

### 👤 회원 인증

| 메서드 | 경로 | 설명 |
|--------|------|------|
| POST | `/signup` | 회원가입 |
| POST | `/signin` | 로그인 (JWT 발급) |

---

## 🛠️ 실행 방법

1. `.env` 또는 `application.yml`에 아래 정보 추가

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/dividend
    username: your_user
    password: your_pw
  jwt:
    secret: your_jwt_secret

scheduler:
  scrap:
    yahoo: 0 0 1 * * *  # 매일 오전 1시
```

2. 실행

```bash
./gradlew bootRun
```

---

## 🧪 테스트

- Controller 및 Service 단위 테스트 포함
- Repository Mocking 기반 유닛 테스트 작성
- 예: `FinanceServiceTest`, `MemberServiceTest`, `CompanyControllerTest`

---

## 📬 문의

해당 프로젝트에 대한 문의는 아래 이메일로 연락주세요.

📧 gleewithajoy@gmail.com
```
