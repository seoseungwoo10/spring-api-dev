아래는 `Chapter06` 소스코드 전체 구조와 주요 컴포넌트, 동작 방식에 대한 요약 가이드입니다.

---

# Chapter06 프로젝트 구조 및 가이드

## 1. 프로젝트 개요

- **프레임워크**: Spring Boot 3.x
- **빌드 도구**: Gradle (또는 변환 시 Maven)
- **주요 기능**: RESTful API, JWT 기반 인증/인가, OpenAPI(Swagger) 기반 코드 생성, JPA, H2 DB, Flyway 마이그레이션

---

## 2. 주요 디렉터리 및 파일 구조

```
src/
 └─ main/
     ├─ java/com/packt/modern/api/
     │   ├─ controller/      // REST API 컨트롤러
     │   ├─ entity/          // JPA 엔티티
     │   ├─ exception/       // 커스텀 예외
     │   ├─ model/           // DTO 및 API 모델
     │   ├─ repository/      // JPA 리포지토리
     │   ├─ security/        // 보안 설정 및 JWT 관련
     │   ├─ service/         // 비즈니스 로직
     │   └─ Application.java // 메인 클래스
     └─ resources/
         ├─ api/             // OpenAPI 스펙, config, ignore 파일
         ├─ application.yml  // Spring Boot 설정
         └─ db/migration/    // Flyway 마이그레이션 스크립트
```

---

## 3. 주요 컴포넌트 설명

### 3.1. 보안(Security)

- `security/SecurityConfig.java`
    - Spring Security 기반의 JWT 인증/인가 설정
    - H2 콘솔, 인증/회원가입/토큰 갱신 등 일부 엔드포인트는 공개
    - 나머지 API는 인증 필요, `/api/v1/addresses/**`는 ADMIN 권한 필요
    - JWT 토큰 검증 및 커스텀 권한 매핑

### 3.2. 인증/인가

- `controller/AuthController.java`
    - 회원가입, 로그인, 토큰 갱신, 로그아웃 등 인증 관련 API 제공
    - 비밀번호 암호화 및 검증, JWT 토큰 발급/갱신/폐기

### 3.3. 비즈니스 로직

- `service/`
    - 사용자, 상품, 주문 등 도메인별 서비스 구현
    - DB 접근은 JPA 리포지토리(`repository/`)를 통해 수행

### 3.4. 데이터베이스

- `entity/`
    - JPA 엔티티 클래스 정의
- `repository/`
    - Spring Data JPA 기반 CRUD 리포지토리
- `resources/db/migration/`
    - Flyway를 통한 DB 마이그레이션 스크립트 관리
- H2 인메모리 DB 사용(개발/테스트용)

### 3.5. OpenAPI/Swagger

- `resources/api/openapi.yaml`
    - API 명세서(OpenAPI 3.0)
- 빌드시 OpenAPI Generator로 API 인터페이스, 모델, 유틸 자동 생성

---

## 4. 빌드 및 실행

### 4.1. Gradle 빌드

```bash
./gradlew clean build
```

### 4.2. 실행

```bash
./gradlew bootRun
```

### 4.3. H2 콘솔

- `http://localhost:8080/h2-console`
- JDBC URL: `jdbc:h2:mem:testdb`

---

## 5. 테스트

- JUnit 기반 테스트 코드 작성
- `./gradlew test`로 실행

---

## 6. 기타

- `application.yml`에서 DB, JWT, 기타 환경설정 관리
- 예외 처리, 유효성 검증, CORS 등 실무에 필요한 설정 포함

---

이 가이드는 전체적인 구조와 각 컴포넌트의 역할, 빌드 및 실행 방법을 요약한 것입니다.  
자세한 API 명세는 `openapi.yaml` 또는 Swagger UI를 참고하세요.

