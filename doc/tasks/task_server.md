# Task: Server (Spring Boot Backend)

> **참조 문서**: [PRD](../PRD.md) | [RULE](../RULE.md) | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) | [07-platform-spring](../rules/07-platform-spring.md) | [06-auth-token](../rules/06-auth-token.md) | [DOC_MAP](../DOC_MAP.md)

---

> **문서 작성**: 각 Step의 `선행 문서` 확인 후 작업, `후속 문서`를 `doc/`에 산출. [13-document-guide](../rules/13-document-guide.md) 준수.

---

## 1. 작업 개요

| 항목 | 내용 |
|------|------|
| 기술 스택 | Spring Boot 3.3+, Java 21, Spring Security 6, JPA + QueryDSL, Spring WebSocket (STOMP) |
| 개발 기간 | 12주 (PRD §12 기준) |
| Step 구조 | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) 필수 10개 필드 준수 |

---

## 2. Step 목록 (총 20단계)

---

### Step 01: 기획·설계 검토

| 필드 | 내용 |
|------|------|
| **Step Name** | 기획·설계 검토 |
| **Step Goal** | PRD 요구사항·ERD·API 설계를 검토하고 개발 가능한 수준으로 정리한다. |
| **Input** | PRD.md, ERD 설계 초안 |
| **Scope** | 포함: 요구사항 목록, ERD 테이블 정의, API 엔드포인트 목록 / 제외: 상세 구현 |
| **Instructions** | 1) PRD §2 기능 목록과 §5 API 명세 대조 2) PRD §3 ERD와 도메인 매핑 검토 3) 누락·모호 사항 문서화 |
| **Output Format** | `doc/` 내 설계 검토 메모 또는 기존 PRD 보완 |
| **Constraints** | DDD Bounded Context(PRD §3.1) 준수 |
| **Done When** | 요구사항·ERD·API가 개발 착수 가능한 수준으로 정리됨 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §2, §3, §5 |
| **선행 문서** | doc/pre-dev/01-proposal, 02-project-plan, 03-requirements |
| **후속 문서** | doc/pre-dev/03-requirements 보완, 05-erd 초안, 06-api-spec 초안 |

---

### Step 02: 프로젝트·패키지 구조 셋업

| 필드 | 내용 |
|------|------|
| **Step Name** | 프로젝트·패키지 구조 셋업 |
| **Step Goal** | Spring Boot 프로젝트와 DDD 스타일 패키지 구조를 생성한다. |
| **Input** | Step 01 산출물, Spring Initializr 또는 템플릿 |
| **Scope** | 포함: backend/ 모듈, 패키지(user/social/travel/chat) / 제외: 비즈니스 코드 |
| **Instructions** | 1) Spring Boot 3.3+ 프로젝트 생성 2) libs.versions.toml 또는 TECH-STACK.md 작성 3) 패키지 구조 설계 (domain, application, infrastructure) |
| **Output Format** | `backend/` 디렉터리, `build.gradle`/`pom.xml`, 패키지 구조 |
| **Constraints** | RULE 03-technical, PRD §3.1 Bounded Context |
| **Done When** | `./mvnw spring-boot:run` 또는 `./gradlew bootRun` 실행 가능 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-spring, 03-technical |
| **선행 문서** | doc/pre-dev/04-architecture |
| **후속 문서** | TECH-STACK.md, 패키지 구조 문서 |

---

### Step 03: DB 스키마·마이그레이션

| 필드 | 내용 |
|------|------|
| **Step Name** | DB 스키마·마이그레이션 |
| **Step Goal** | PRD ERD에 따른 테이블을 Flyway/Liquibase로 생성한다. |
| **Input** | PRD §3.2 ERD, Step 02 프로젝트 |
| **Scope** | 포함: users, profiles, friendships, follows, posts, posts_media, comments, hashtags, post_hashtags, travels, itineraries, itinerary_items, budgets, budget_expenses, reviews, destinations, chat_rooms, room_members, messages / 제외: 인덱스 최적화(후속) |
| **Instructions** | 1) Flyway 또는 Liquibase 의존성 추가 2) V1__init.sql 생성 3) PRD §3.2 ERD 테이블 DDL 작성 4) 마이그레이션 실행 검증 |
| **Output Format** | `src/main/resources/db/migration/V1__*.sql` |
| **Constraints** | 정규화·외래키 제약, PRD §3.4 인덱스 전략 |
| **Done When** | 마이그레이션 실행 후 DB에 테이블 생성 확인 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §3.2, §3.4 |
| **선행 문서** | doc/pre-dev/05-erd |
| **후속 문서** | doc/pre-dev/05-erd (물리 ERD), 테이블 정의서 |

---

### Step 04: 환경 변수·시크릿 설정

| 필드 | 내용 |
|------|------|
| **Step Name** | 환경 변수·시크릿 설정 |
| **Step Goal** | DB·JWT 등 민감 정보를 환경 변수로 주입하고 하드코딩을 제거한다. |
| **Input** | Step 02 프로젝트, application.yml |
| **Scope** | 포함: DB_URL, DB_USER, DB_PASSWORD, JWT_SECRET 등 / 제외: 애플리케이션 비즈니스 설정 |
| **Instructions** | 1) application.yml에 `spring.datasource.url: ${DB_URL}` 등 치환 2) .env 또는 run-dev.ps1 예시 작성 3) .gitignore에 .env 추가 |
| **Output Format** | application.yml, .env.example, run-dev.ps1(선택) |
| **Constraints** | RULE 7.1.1, 01-security 비밀정보 하드코딩 금지 |
| **Done When** | 환경 변수 없이 실행 시 실패, 환경 변수 주입 시 정상 기동 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-spring §7.1.1, 01-security |
| **선행 문서** | — |
| **후속 문서** | .env.example, run-dev.ps1 (선택) |

---

### Step 05: JWT 인증·토큰 프로바이더

| 필드 | 내용 |
|------|------|
| **Step Name** | JWT 인증·토큰 프로바이더 |
| **Step Goal** | JWT 서명·검증·클레임 검증을 수행하는 TokenProvider를 구현한다. |
| **Input** | Step 04 환경, JWT 라이브러리(jjwt 등) |
| **Scope** | 포함: Access/Refresh 토큰 발급, 검증, alg allow-list, iss/aud, jti / 제외: 필터·Security 설정 |
| **Instructions** | 1) JwtTokenProvider 구현 2) alg allow-list, iss/aud 검증 3) jti 포함, Access 15분 이하, Refresh Redis 저장(선택) 4) Revocation(블랙리스트) 구조 설계 |
| **Output Format** | `auth/` 패키지 내 JwtTokenProvider, TokenProperties |
| **Constraints** | 06-auth-token §6.1 전부 |
| **Done When** | 토큰 발급·검증·만료 검증 단위 테스트 통과 |
| **Duration** | 2일 |
| **RULE Reference** | 06-auth-token §6.1, §6.5 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log (매일) |

---

### Step 06: JWT 필터·Security 설정

| 필드 | 내용 |
|------|------|
| **Step Name** | JWT 필터·Security 설정 |
| **Step Goal** | JWT 기반 인증 필터와 SecurityFilterChain, CORS를 구성한다. |
| **Input** | Step 05 프로바이더, Spring Security |
| **Scope** | 포함: JwtAuthenticationFilter, SecurityFilterChain, CorsConfigurationSource / 제외: 인가 세부 |
| **Instructions** | 1) JwtAuthenticationFilter 구현 (토큰 추출·검증·SecurityContext 설정) 2) SecurityFilterChain Bean 구성 3) permitAll: /api/auth/**, /api/public/** 4) CORS: CorsConfigurationSource Bean, 환경별 origins |
| **Output Format** | SecurityConfig, JwtAuthenticationFilter, CorsConfig |
| **Constraints** | 07-platform-spring §7.1.10, 06-auth-token |
| **Done When** | 인증 없이 보호 API 호출 시 401, 인증 후 200 |
| **Duration** | 1.5일 |
| **RULE Reference** | 07-platform-spring §7.1.10, 06-auth-token |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 07: GlobalExceptionHandler·공통 예외

| 필드 | 내용 |
|------|------|
| **Step Name** | GlobalExceptionHandler·공통 예외 |
| **Step Goal** | 모든 예외를 ErrorResponse 형식으로 통일 처리한다. |
| **Input** | Step 02 프로젝트 |
| **Scope** | 포함: ErrorResponse, BusinessException, GlobalExceptionHandler (Validation, Business, NotFound, Exception) / 제외: 도메인별 예외 클래스 |
| **Instructions** | 1) ErrorResponse record (code, message, timestamp, path, traceId, details) 2) BusinessException 정의 3) GlobalExceptionHandler @RestControllerAdvice 4) MethodArgumentNotValidException, ConstraintViolationException, BusinessException, Exception 처리 |
| **Output Format** | `common/exception/` 패키지 |
| **Constraints** | 07-platform-spring §7.1.2, RULE 02-function |
| **Done When** | Validation 오류·비즈니스 예외·일반 예외가 각각 일관된 형식으로 반환됨 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-spring §7.1.2, 02-function |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 08: User·Profile 엔티티·API

| 필드 | 내용 |
|------|------|
| **Step Name** | User·Profile 엔티티·API |
| **Step Goal** | User, Profile 엔티티와 CRUD API를 구현한다. |
| **Input** | Step 03 스키마, PRD §5.2 |
| **Scope** | 포함: User, Profile 엔티티, UserRepository, ProfileRepository, GET/PUT /api/users/me, GET/PUT /api/profiles/{userId}, /api/profiles/me / 제외: Auth API |
| **Instructions** | 1) User, Profile JPA 엔티티 2) Repository·Service·Controller 3) Java Record DTO, @Valid·@Validated 4) 401/403 구분 |
| **Output Format** | `user/`, `profile/` 패키지 |
| **Constraints** | 07-platform-spring §7.1.3, §7.1.5, §7.1.6, §7.1.7 |
| **Done When** | /api/users/me, /api/profiles/{id} 호출 시 정상 응답 |
| **Duration** | 2일 |
| **RULE Reference** | 07-platform-spring, PRD §5.2 |
| **선행 문서** | doc/pre-dev/06-api-spec (User·Profile) |
| **후속 문서** | doc/pre-dev/06-api-spec 갱신 |

---

### Step 09: Auth API (회원가입·로그인·토큰 갱신)

| 필드 | 내용 |
|------|------|
| **Step Name** | Auth API (회원가입·로그인·토큰 갱신) |
| **Step Goal** | 회원가입·로그인·토큰 갱신·로그아웃 API를 구현한다. |
| **Input** | Step 05~08 |
| **Scope** | 포함: POST /api/auth/register, login, refresh, logout / 제외: OAuth |
| **Instructions** | 1) AuthController, AuthService 2) register: 비밀번호 암호화·중복 검증 3) login: JWT 발급 4) refresh: Refresh Token 검증 후 Access 재발급 5) logout: Revocation(블랙리스트) |
| **Output Format** | AuthController, AuthService |
| **Constraints** | PRD §5.1, 06-auth-token |
| **Done When** | register→login→API 호출→refresh→logout 흐름 정상 동작 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.1, 06-auth-token |
| **선행 문서** | doc/pre-dev/06-api-spec (Auth) |
| **후속 문서** | doc/pre-dev/06-api-spec 갱신, doc/during-dev/08-work-log |

---

### Step 10: Post·Comment 엔티티·게시물 CRUD

| 필드 | 내용 |
|------|------|
| **Step Name** | Post·Comment 엔티티·게시물 CRUD |
| **Step Goal** | Post, PostMedia, Comment 엔티티와 게시물 CRUD·미디어 업로드 API를 구현한다. |
| **Input** | Step 03 스키마, PRD §5.3 |
| **Scope** | 포함: Post, PostMedia, Comment 엔티티, 게시물 CRUD, 미디어 업로드 / 제외: 피드·해시태그 |
| **Instructions** | 1) Post, PostMedia, Comment 엔티티 2) PostController CRUD, 계층형 URI 3) FileUploadService (multipart) 4) Service @Transactional(readOnly) |
| **Output Format** | `social/post/`, `social/comment/` 패키지 |
| **Constraints** | 07-platform-spring §7.1.3, §7.1.5 |
| **Done When** | 게시물 생성·조회·수정·삭제·미디어 업로드 동작 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.3, 07-platform-spring |
| **선행 문서** | doc/pre-dev/06-api-spec (Social) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 11: 댓글·해시태그·피드

| 필드 | 내용 |
|------|------|
| **Step Name** | 댓글·해시태그·피드 |
| **Step Goal** | 댓글 API, 해시태그, 피드(QueryDSL)를 구현한다. |
| **Input** | Step 10, PRD §5.3 |
| **Scope** | 포함: POST/GET comments, GET hashtags/trending, hashtags/{name}/posts, GET posts/feed / 제외: 친구·팔로우 |
| **Instructions** | 1) CommentController 2) Hashtag, PostHashtag 엔티티 3) HashtagController 4) PostRepositoryCustom QueryDSL 피드 조건 5) Page/Slice, 정렬 허용 필드 제한 |
| **Output Format** | CommentController, HashtagController, PostRepositoryCustom |
| **Constraints** | PRD §10.2, 07-platform-spring §7.1.8, RULE 03-technical N+1 |
| **Done When** | 댓글·해시태그·피드 API 호출 정상 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.3, §10.2, 07-platform-spring §7.1.8 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/07-detailed-design (Social), doc/during-dev/08-work-log |

---

### Step 12: 친구·팔로우·공개 범위

| 필드 | 내용 |
|------|------|
| **Step Name** | 친구·팔로우·공개 범위 |
| **Step Goal** | Friendship, Follow 엔티티와 API, 공개 범위(visibility) 제어를 구현한다. |
| **Input** | Step 10~11, PRD §5.2, §4.3 |
| **Scope** | 포함: 친구 요청/목록, 팔로우/언팔로우, visibility(PUBLIC/FRIENDS/PRIVATE) / 제외: 채팅 |
| **Instructions** | 1) Friendship, Follow 엔티티·Repository 2) FriendController, FollowController 3) PostService·TravelService visibility 검증 4) IDOR 방어 |
| **Output Format** | FriendController, FollowController, visibility 로직 |
| **Constraints** | RULE 01-security, appendix-a-asvs IDOR |
| **Done When** | 친구·팔로우 API 및 공개 범위에 따른 접근 제어 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §4.3, §5.2, 01-security |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 13: 채팅 엔티티·WebSocket 설정

| 필드 | 내용 |
|------|------|
| **Step Name** | 채팅 엔티티·WebSocket 설정 |
| **Step Goal** | ChatRoom, RoomMember, Message 엔티티와 WebSocket(STOMP) 구성을 완료한다. |
| **Input** | Step 03 스키마, PRD §6 |
| **Scope** | 포함: ChatRoom, RoomMember, Message 엔티티, WebSocketConfig, /ws/chat / 제외: REST API·메시지 처리 |
| **Instructions** | 1) ChatRoom, RoomMember, Message 엔티티 2) WebSocketConfig, STOMP 엔드포인트 3) /ws/chat, SockJS 4) HandshakeInterceptor JWT 검증 |
| **Output Format** | `chat/` 패키지, WebSocketConfig |
| **Constraints** | PRD §6, §10.3, 06-auth-token |
| **Done When** | WebSocket 연결 시 JWT 검증 후 구독 가능 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §6, 06-auth-token |
| **선행 문서** | doc/pre-dev/06-api-spec (Chat) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 14: 채팅 REST API·실시간 메시지

| 필드 | 내용 |
|------|------|
| **Step Name** | 채팅 REST API·실시간 메시지 |
| **Step Goal** | 채팅방 생성/목록, 메시지 히스토리 API와 실시간 메시지 송수신을 구현한다. |
| **Input** | Step 13 |
| **Scope** | 포함: POST/GET chat/rooms, GET rooms/{id}/messages, STOMP 메시지 송수신 / 제외: 알림 |
| **Instructions** | 1) ChatController REST API 2) ChatService 메시지 DB 저장 3) STOMP /topic/room/{roomId}, /app/chat.send |
| **Output Format** | ChatController, ChatService, StompController |
| **Constraints** | PRD §5.5, §6.2 |
| **Done When** | 채팅방 생성·목록·메시지 히스토리·실시간 송수신 동작 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.5, §6 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/07-detailed-design (Chat), doc/during-dev/08-work-log |

---

### Step 15: 여행·일정·목적지

| 필드 | 내용 |
|------|------|
| **Step Name** | 여행·일정·목적지 |
| **Step Goal** | Travel, Itinerary, ItineraryItem, Destination 엔티티와 CRUD·검색 API를 구현한다. |
| **Input** | Step 03 스키마, PRD §5.4 |
| **Scope** | 포함: Travel CRUD, Itinerary CRUD, Destination 검색 / 제외: 예산·날씨 |
| **Instructions** | 1) Travel, Itinerary, ItineraryItem, Destination 엔티티 2) TravelController, ItineraryController, DestinationController 3) 계층형 URI |
| **Output Format** | `travel/` 패키지 |
| **Constraints** | PRD §3.2, §5.4, 07-platform-spring §7.1.3 |
| **Done When** | 여행·일정·목적지 검색 API 정상 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec (Travel) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 16: 예산·지출·날씨

| 필드 | 내용 |
|------|------|
| **Step Name** | 예산·지출·날씨 |
| **Step Goal** | Budget, BudgetExpense 엔티티와 API, 날씨 연동을 구현한다. |
| **Input** | Step 15, PRD §7.2 |
| **Scope** | 포함: Budget CRUD, BudgetExpense, Weather API 연동 / 제외: 교통 |
| **Instructions** | 1) Budget, BudgetExpense 엔티티 2) BudgetController 3) 예산 추적 로직(planned - spent) 4) WeatherService (OpenWeather 등), Timeout·Retry |
| **Output Format** | BudgetController, BudgetService, WeatherService |
| **Constraints** | PRD §7.2, RULE 03-technical 외부 호출 Timeout |
| **Done When** | 예산·지출·날씨 API 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.4, §7.2, 03-technical |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 17: 리뷰·공유

| 필드 | 내용 |
|------|------|
| **Step Name** | 리뷰·공유 |
| **Step Goal** | Review 엔티티와 API, 여행 일정 공유(visibility)를 구현한다. |
| **Input** | Step 15, PRD §7.3 |
| **Scope** | 포함: Review CRUD, Travel visibility 공유 / 제외: 교통 정보 |
| **Instructions** | 1) Review 엔티티 2) ReviewController 3) TravelService visibility 공유 로직 |
| **Output Format** | ReviewController, TravelService 공유 로직 |
| **Constraints** | PRD §5.4, §7.3 |
| **Done When** | 리뷰 작성·조회, 여행 공유 설정 동작 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §5.4, §7.3 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/07-detailed-design (Travel), doc/during-dev/08-work-log |

---

### Step 18: Rate Limiting·로깅

| 필드 | 내용 |
|------|------|
| **Step Name** | Rate Limiting·로깅 |
| **Step Goal** | 공개 API Rate Limiting과 구조화 로깅(traceId/spanId)을 적용한다. |
| **Input** | Step 01~17 |
| **Scope** | 포함: Rate Limiting Filter/Interceptor, 429+Retry-After, SLF4J 파라미터화 로깅, traceId/spanId MDC / 제외: OpenTelemetry(선택) |
| **Instructions** | 1) Rate Limiting 적용 (공개 API) 2) 429 응답 시 Retry-After 헤더 3) SLF4J 로깅, `{}` 파라미터화 4) MDC traceId/spanId |
| **Output Format** | RateLimitFilter, LoggingConfig |
| **Constraints** | RULE 01-security §1.9, 09-observability |
| **Done When** | 과도 요청 시 429, 로그에 traceId 포함 |
| **Duration** | 1일 |
| **RULE Reference** | 01-security, 09-observability |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 19: API 문서·테스트

| 필드 | 내용 |
|------|------|
| **Step Name** | API 문서·테스트 |
| **Step Goal** | Swagger/OpenAPI 문서화와 핵심 로직 단위 테스트를 작성한다. |
| **Input** | Step 01~17 |
| **Scope** | 포함: Swagger 설정, 핵심 Service/Repository 테스트 / 제외: E2E |
| **Instructions** | 1) springdoc-openapi 또는 Swagger 설정 2) Given-When-Then, AssertJ, BDDMockito 3) @DisplayName 한글 4) Testcontainers 또는 H2 |
| **Output Format** | `src/test/`, `src/main/resources/` Swagger |
| **Constraints** | RULE 04-quality §4.2, §4.3.1 |
| **Done When** | Swagger UI 접근 가능, 핵심 테스트 통과 |
| **Duration** | 2일 |
| **RULE Reference** | 04-quality §4.2, §4.3.1 |
| **선행 문서** | doc/during-dev/10-test-case |
| **후속 문서** | doc/pre-dev/06-api-spec (Swagger), doc/during-dev/10-test-case 갱신 |

---

### Step 20: 환경 분리·배포 준비

| 필드 | 내용 |
|------|------|
| **Step Name** | 환경 분리·배포 준비 |
| **Step Goal** | dev/prod 환경별 설정 분리와 README·실행 방법을 정리한다. |
| **Input** | Step 01~19 |
| **Scope** | 포함: application-dev.yml, application-prod.yml, README 실행 방법 / 제외: CI/CD 파이프라인 |
| **Instructions** | 1) application-dev, application-prod 분리 2) CORS origins 환경별 설정 3) README 실행 방법·환경 변수 문서화 |
| **Output Format** | application-*.yml, README.md |
| **Constraints** | RULE 05-operation |
| **Done When** | dev/prod 프로파일로 기동 가능, README 따라 실행 가능 |
| **Duration** | 1일 |
| **RULE Reference** | 05-operation, PRD §13 |
| **선행 문서** | — |
| **후속 문서** | doc/post-dev/13-deployment-guide, README.md |

---

## 3. 규칙 준수 체크포인트

- [ ] 비밀정보 환경 변수 주입, 401/403 구분, 입력 검증, RULE 01-security
- [ ] JWT alg allow-list, iss/aud, jti, Revocation, 06-auth-token
- [ ] HTTP Method·계층형 URI, BusinessException, 트랜잭션 Service, RULE 02-function
- [ ] 04-quality §4.2 Given-When-Then, AssertJ, @DisplayName
