# JWT Authentication Study

Java · Spring Boot · Spring Security 6 · JWT 기반 인증·인가를 단계별로 학습한 실습 저장소입니다.

---

## 기술 스택

![Java](https://img.shields.io/badge/Java_17-007396?style=flat&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.3-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security_6-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![JWT](https://img.shields.io/badge/JWT_(jjwt_0.11.5)-000000?style=flat&logo=jsonwebtokens&logoColor=white)
![JPA](https://img.shields.io/badge/JPA/Hibernate-59666C?style=flat&logo=hibernate&logoColor=white)
![H2](https://img.shields.io/badge/H2-0D6EFD?style=flat&logo=h2&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat&logo=thymeleaf&logoColor=white)
![Gradle](https://img.shields.io/badge/Gradle-02303A?style=flat&logo=gradle&logoColor=white)

---

## 프로젝트 구조

```
src/
└── main/
    ├── java/com/web/demo/
    │   ├── TestApplication.java              # 메인 진입점
    │   ├── TokenAuthenticationFilter.java    # OncePerRequestFilter — 요청별 JWT 검증
    │   ├── MainController.java
    │   ├── config/
    │   │   ├── JwtProperties.java            # jwt.issuer / jwt.secretKey 바인딩
    │   │   ├── SecurityConfig.java           # FilterChain + 커스텀 필터 등록
    │   │   └── UserRole.java                 # 사용자 권한 Enum
    │   ├── controller/
    │   │   ├── UserController.java           # 회원가입·로그인·토큰 재발급 (/user/**)
    │   │   ├── PostController.java           # 게시글 CRUD (/post/**)
    │   │   ├── ReviewController.java         # 댓글 CRUD
    │   │   └── TokenTestController.java      # REST 토큰 재발급 API (/api/token)
    │   ├── dto/
    │   │   ├── UserForm.java
    │   │   ├── PostForm.java
    │   │   └── ReviewForm.java
    │   ├── entity/
    │   │   ├── SnsUser.java                  # 사용자 (username, password, email)
    │   │   ├── RefreshToken.java             # 리프레시 토큰 (userId 매핑)
    │   │   ├── Post.java                     # 게시글
    │   │   └── Review.java                   # 댓글
    │   ├── repository/                       # Spring Data JPA Repository 인터페이스
    │   ├── service/
    │   │   ├── TokenProvider.java            # 토큰 생성·서명·검증 (HS256)
    │   │   ├── TokenService.java             # Refresh → Access 토큰 재발급
    │   │   ├── UserService.java
    │   │   ├── PostService.java
    │   │   ├── ReviewService.java
    │   │   ├── SecurityService.java
    │   │   └── UtilService.java              # 쿠키 생성 유틸
    │   ├── exception/
    │   │   └── DataNotFoundException.java
    │   └── test/
    │       ├── CreateAccessTokenRequest.java
    │       └── CreateAccessTokenResponse.java
    └── resources/
        ├── application.properties
        ├── static/                           # AdminLTE 정적 파일 (dist/, plugins/)
        └── templates/                        # Thymeleaf 템플릿 (레이아웃 분리)
```

---

## 실행 방법

IntelliJ에서 프로젝트를 열어 실행합니다.

```bash
1. IntelliJ → File > Open → jwt 폴더 선택
2. build.gradle 인식 후 의존성 다운로드 대기
3. TestApplication.java 실행
4. http://localhost:8080 접속
```

> H2 인메모리 DB를 사용하므로 별도 DB 설치 없이 즉시 실행됩니다.  
> H2 콘솔: `http://localhost:8080/h2-console`  
> `application.properties`에서 `jwt.issuer` / `jwt.secretKey` 값을 환경에 맞게 설정하세요.

---

## 학습 내용

| 주제 | 주요 학습 내용 |
|------|---------------|
| JWT 구조 | Header·Payload·Signature 구성, typ/iss/iat/exp/sub/claim 필드 |
| 토큰 생성·검증 | `TokenProvider` — jjwt(HS256) 서명, `isValidToken()`, `getClaims()` |
| 이중 토큰 전략 | Access Token(단기) + Refresh Token(장기) 분리, DB 저장 기반 Refresh 검증 |
| 토큰 재발급 흐름 | Access 만료 → Refresh 유효성 확인 → 새 Access 발급 → 쿠키 갱신 |
| JWT 필터 구현 | `OncePerRequestFilter` 상속, `SecurityFilterChain`에 `addFilterBefore` 등록 |
| 쿠키 기반 전달 | `HttpServletResponse`로 `access_token` / `refresh_token` 쿠키 설정·만료 관리 |
| Spring Security 연동 | `SecurityConfig` FilterChain, `AuthenticationManager`, `BCryptPasswordEncoder` |
| JPA 엔티티 설계 | `SnsUser`, `RefreshToken`(userId 외래키), `Post`, `Review` |
| 게시판 CRUD | JPA + H2 + Thymeleaf, 게시글·댓글 Create/Read/Update/Delete |
| REST API | `TokenTestController` — `POST /api/token` 리프레시 기반 액세스 토큰 재발급 |

---

## 개발 환경

- **JDK 17** 이상 — [다운로드](https://download.oracle.com/java/17/archive/jdk-17.0.12_windows-x64_bin.exe)
- **IntelliJ IDEA** Ultimate (추천) / Community
- H2 인메모리 DB 내장 — 별도 설치 불필요
