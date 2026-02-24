# Task: Mobile (Flutter)

> **참조 문서**: [PRD](../PRD.md) | [RULE](../RULE.md) | [DOC_MAP](../DOC_MAP.md) | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) | [07-platform-flutter](../rules/07-platform-flutter.md)

---

> **문서 작성**: 각 Step의 `선행 문서` 확인 후 작업, `후속 문서`를 `doc/`에 산출. [13-document-guide](../rules/13-document-guide.md) 준수.

---

## 1. 작업 개요

| 항목 | 내용 |
|------|------|
| 기술 스택 | Flutter 3.x, Dart 3.x, Riverpod 3.0+, Dio, GoRouter |
| 개발 기간 | 12주 (PRD §12, Week 11~12 집중) |
| Step 구조 | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) 필수 10개 필드 준수 |

---

## 2. Step 목록 (총 20단계)

---

### Step 01: 프로젝트·폴더 구조

| 필드 | 내용 |
|------|------|
| **Step Name** | 프로젝트·폴더 구조 |
| **Step Goal** | Flutter 프로젝트와 core/, features/, shared/ 폴더 구조를 생성한다. |
| **Input** | Flutter SDK, PRD §8.2 |
| **Scope** | 포함: frontend-mobile/ 생성, core/, features/, shared/ / 제외: 비즈니스 코드 |
| **Instructions** | 1) flutter create 2) core/(network, storage, router, config, error) 3) features/(auth, posts, travels, chat) 4) shared/(widgets, theme, utils) |
| **Output Format** | `frontend-mobile/` 디렉터리, `lib/` 폴더 구조 |
| **Constraints** | 07-platform-flutter §7.3.4 |
| **Done When** | `flutter run` 실행 시 기본 앱 표시 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-flutter §7.3.4 |
| **선행 문서** | doc/pre-dev/04-architecture, doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 02: Secure Storage·환경 설정

| 필드 | 내용 |
|------|------|
| **Step Name** | Secure Storage·환경 설정 |
| **Step Goal** | flutter_secure_storage와 환경별 설정(--dart-define-from-file)을 구성한다. |
| **Input** | Step 01 |
| **Scope** | 포함: FlutterSecureStorage 래퍼, config/env.dev.json, env.prod.json, AppConfig / 제외: 토큰 저장 로직 |
| **Instructions** | 1) flutter_secure_storage 의존성 2) core/storage/ SecureStorage 래퍼 3) --dart-define-from-file, String.fromEnvironment 4) .gitignore env.*.json |
| **Output Format** | core/storage/, core/config/, config/env.*.json |
| **Constraints** | 07-platform-flutter §7.3.1, §7.3.11 |
| **Done When** | SecureStorage read/write, 환경별 API URL 로드 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-flutter §7.3.1, §7.3.11 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 03: Dio·인터셉터·AppException

| 필드 | 내용 |
|------|------|
| **Step Name** | Dio·인터셉터·AppException |
| **Step Goal** | Dio 공통 인스턴스, AuthInterceptor, AppException 계층을 구현한다. |
| **Input** | Step 01~02 |
| **Scope** | 포함: DioClient, AuthInterceptor, AppException, DioException 매핑 / 제외: API 함수 |
| **Instructions** | 1) Dio BaseOptions (baseUrl, timeout) 2) AuthInterceptor (토큰 주입·갱신) 3) AppException (Network, Unauthorized, Server, NotFound) 4) exception_mapper |
| **Output Format** | core/network/, core/error/ |
| **Constraints** | 07-platform-flutter §7.3.2, §7.3.3 |
| **Done When** | Dio 요청 시 토큰 주입, DioException → AppException 변환 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-flutter §7.3.2, §7.3.3 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 04: GoRouter·인증 리다이렉트

| 필드 | 내용 |
|------|------|
| **Step Name** | GoRouter·인증 리다이렉트 |
| **Step Goal** | GoRouter 설정과 미인증 시 로그인 리다이렉트를 구현한다. |
| **Input** | Step 01, AuthNotifier(가칭) |
| **Scope** | 포함: GoRouter, redirect (미인증→/auth/login) / 제외: 화면 구현 |
| **Instructions** | 1) go_router 의존성 2) app_router, routes 3) redirect: authState 기반 4) /auth/login, /home 등 경로 |
| **Output Format** | core/router/app_router.dart |
| **Constraints** | 07-platform-flutter §7.3.12 |
| **Done When** | 미인증 시 /auth/login, 인증 시 /home 이동 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-flutter §7.3.12 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 05: Auth API·AuthNotifier

| 필드 | 내용 |
|------|------|
| **Step Name** | Auth API·AuthNotifier |
| **Step Goal** | Auth API(login, register, refresh)와 AuthNotifier(Riverpod)를 구현한다. |
| **Input** | Step 03~04, PRD §5.1 |
| **Scope** | 포함: AuthRepository, AuthNotifier(keepAlive), Secure Storage 토큰 저장 / 제외: UI |
| **Instructions** | 1) features/auth/data/ AuthRepository 2) AuthNotifier @Riverpod(keepAlive: true) 3) login, register, refresh 4) SecureStorage 토큰 저장 |
| **Output Format** | features/auth/data/, features/auth/presentation/ |
| **Constraints** | 06-auth-token §6.1.6, 07-platform-flutter §7.3.5 |
| **Done When** | login→토큰 저장→API 호출, refresh 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.1, 06-auth-token, 07-platform-flutter §7.3.5 |
| **선행 문서** | doc/pre-dev/06-api-spec (Auth) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 06: Splash·로그인·회원가입 화면

| 필드 | 내용 |
|------|------|
| **Step Name** | Splash·로그인·회원가입 화면 |
| **Step Goal** | Splash, Login, Register 화면을 구현한다. |
| **Input** | Step 04~05, PRD §8.2 |
| **Scope** | 포함: SplashScreen, LoginScreen, RegisterScreen / 제외: 프로필 |
| **Instructions** | 1) SplashScreen 2) LoginScreen (AuthNotifier 연동) 3) RegisterScreen 4) /auth/login, /auth/register |
| **Output Format** | features/auth/presentation/ |
| **Constraints** | 07-platform-flutter §7.3.6 (200줄 이하) |
| **Done When** | Splash→Login→Register→로그인 성공 시 Main 이동 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §8.2 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 07: Main·BottomNav

| 필드 | 내용 |
|------|------|
| **Step Name** | Main·BottomNav |
| **Step Goal** | Main 화면과 BottomNav(Feed, Travel, Chat, Profile)를 구현한다. |
| **Input** | Step 04~06 |
| **Scope** | 포함: MainScreen, BottomNavigationBar 4탭 / 제외: 각 탭 내용 |
| **Instructions** | 1) MainScreen 2) BottomNav: Feed, Travel, Chat, Profile 3) 각 탭 placeholder |
| **Output Format** | MainScreen |
| **Constraints** | PRD §8.2 |
| **Done When** | 탭 전환 시 해당 화면 표시 |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §8.2 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 08: Posts API·PostRepository

| 필드 | 내용 |
|------|------|
| **Step Name** | Posts API·PostRepository |
| **Step Goal** | Posts API와 PostRepository를 구현한다. |
| **Input** | Step 03, PRD §5.3 |
| **Scope** | 포함: PostRepository, getList, getById, create / 제외: UI |
| **Instructions** | 1) features/posts/data/ PostRepository 2) Dio 연동 3) DTO (freezed 또는 json_serializable) |
| **Output Format** | features/posts/data/ |
| **Constraints** | 07-platform-flutter §7.3.8 |
| **Done When** | PostRepository 메서드 호출 시 API 연동 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §5.3, 07-platform-flutter §7.3.8 |
| **선행 문서** | doc/pre-dev/06-api-spec (Social) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 09: Feed·PostCard·PostDetail

| 필드 | 내용 |
|------|------|
| **Step Name** | Feed·PostCard·PostDetail |
| **Step Goal** | Feed 화면, PostCard, PostDetail 화면을 구현한다. |
| **Input** | Step 07~08 |
| **Scope** | 포함: FeedScreen, PostListNotifier, PostCard, PostDetailScreen, CachedNetworkImage, pull-to-refresh / 제외: 게시물 작성 |
| **Instructions** | 1) PostListNotifier @riverpod 2) FeedScreen 3) PostCard (CachedNetworkImage) 4) PostDetailScreen 5) RefreshIndicator, isRefreshing |
| **Output Format** | features/posts/presentation/ |
| **Constraints** | 07-platform-flutter §7.3.5, §7.3.9 |
| **Done When** | 피드 목록·상세·pull-to-refresh 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | 07-platform-flutter §7.3.5, §7.3.9 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 10: 게시물 작성

| 필드 | 내용 |
|------|------|
| **Step Name** | 게시물 작성 |
| **Step Goal** | 게시물 작성 화면(텍스트·이미지)을 구현한다. |
| **Input** | Step 08~09, PRD SM-03 |
| **Scope** | 포함: PostCreateScreen, 이미지 선택·업로드 / 제외: 해시태그 |
| **Instructions** | 1) PostCreateScreen 2) image_picker 3) multipart 업로드 |
| **Output Format** | PostCreateScreen |
| **Constraints** | 07-platform-flutter §7.3.6 |
| **Done When** | 게시물 작성·업로드 동작 |
| **Duration** | 1일 |
| **RULE Reference** | PRD SM-03 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 11: 프로필 화면

| 필드 | 내용 |
|------|------|
| **Step Name** | 프로필 화면 |
| **Step Goal** | 프로필 조회·수정 화면을 구현한다. |
| **Input** | Step 05~07, PRD §5.2 |
| **Scope** | 포함: ProfileScreen, GET/PUT profiles / 제외: 친구·팔로우 상세 |
| **Instructions** | 1) ProfileRepository 2) ProfileScreen (조회·수정) |
| **Output Format** | features/auth/ 또는 features/profile/ |
| **Constraints** | PRD §5.2 |
| **Done When** | 프로필 조회·수정 동작 |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §5.2 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 12: Travels API·여행 목록

| 필드 | 내용 |
|------|------|
| **Step Name** | Travels API·여행 목록 |
| **Step Goal** | Travels API, TravelRepository, TravelListScreen을 구현한다. |
| **Input** | Step 03, PRD §5.4 |
| **Scope** | 포함: TravelRepository, TravelListScreen / 제외: 상세·일정 |
| **Instructions** | 1) features/travels/data/ TravelRepository 2) TravelListScreen 3) TravelListNotifier |
| **Output Format** | features/travels/ |
| **Constraints** | 07-platform-flutter |
| **Done When** | 여행 목록 표시 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec (Travel) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 13: 여행 상세·일정·예산

| 필드 | 내용 |
|------|------|
| **Step Name** | 여행 상세·일정·예산 |
| **Step Goal** | TravelDetailScreen, ItineraryEditScreen, BudgetSection을 구현한다. |
| **Input** | Step 12, PRD TP-02, TP-05 |
| **Scope** | 포함: TravelDetailScreen, ItineraryEditScreen, BudgetSection / 제외: 날씨·리뷰 |
| **Instructions** | 1) TravelDetailScreen 2) ItineraryEditScreen 3) BudgetSection |
| **Output Format** | features/travels/presentation/ |
| **Constraints** | PRD §8.2 |
| **Done When** | 여행 상세·일정·예산 표시·편집 동작 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 14: 목적지 검색·날씨·리뷰

| 필드 | 내용 |
|------|------|
| **Step Name** | 목적지 검색·날씨·리뷰 |
| **Step Goal** | DestinationSearch, WeatherWidget, ReviewSection을 구현한다. |
| **Input** | Step 13, PRD §5.4 |
| **Scope** | 포함: DestinationSearchScreen, WeatherWidget, ReviewSection / 제외: 교통 |
| **Instructions** | 1) DestinationSearchScreen 2) WeatherWidget 3) ReviewSection |
| **Output Format** | features/travels/presentation/ |
| **Constraints** | PRD TP-01, TP-03, TP-07 |
| **Done When** | 목적지 검색·날씨·리뷰 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 15: Chat API·채팅 목록

| 필드 | 내용 |
|------|------|
| **Step Name** | Chat API·채팅 목록 |
| **Step Goal** | Chat API, ChatRepository, ChatListScreen을 구현한다. |
| **Input** | Step 03, PRD §5.5 |
| **Scope** | 포함: ChatRepository, ChatListScreen / 제외: 실시간 메시지 |
| **Instructions** | 1) features/chat/data/ ChatRepository 2) ChatListScreen 3) rooms, messages API |
| **Output Format** | features/chat/ |
| **Constraints** | PRD §5.5 |
| **Done When** | 채팅방 목록 표시 |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §5.5 |
| **선행 문서** | doc/pre-dev/06-api-spec (Chat) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 16: WebSocket·실시간 채팅

| 필드 | 내용 |
|------|------|
| **Step Name** | WebSocket·실시간 채팅 |
| **Step Goal** | STOMP/WebSocket 연동과 ChatRoomScreen 실시간 메시지를 구현한다. |
| **Input** | Step 15, PRD §6 |
| **Scope** | 포함: stomp_dart_client 또는 web_socket_channel, ChatRoomScreen / 제외: 알림 |
| **Instructions** | 1) WebSocket 연결 2) /topic/room/{roomId} 구독 3) ChatRoomScreen 4) 메시지 송수신 |
| **Output Format** | core/network/websocket/, ChatRoomScreen |
| **Constraints** | PRD §6.2 |
| **Done When** | 실시간 메시지 송수신 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §6 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 17: 공통 위젯·테마

| 필드 | 내용 |
|------|------|
| **Step Name** | 공통 위젯·테마 |
| **Step Goal** | AppButton, AppTextField 등 공통 위젯과 테마를 구성한다. |
| **Input** | Step 01~16 |
| **Scope** | 포함: shared/widgets/, shared/theme/ / 제외: 페이지 전용 |
| **Instructions** | 1) AppButton, AppTextField 2) ThemeData, 색상, 텍스트 스타일 |
| **Output Format** | shared/widgets/, shared/theme/ |
| **Constraints** | 07-platform-flutter §7.3.4 |
| **Done When** | 공통 위젯 재사용, 테마 적용 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-flutter §7.3.4 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 18: 에러 처리·플랫폼 설정

| 필드 | 내용 |
|------|------|
| **Step Name** | 에러 처리·플랫폼 설정 |
| **Step Goal** | DioException→AppException 매핑, 사용자 친화적 에러 UI, Android/iOS 설정을 완료한다. |
| **Input** | Step 03, Step 17 |
| **Scope** | 포함: ErrorView, exception_mapper 보완, AndroidManifest, Info.plist / 제외: 상세 에러 메시지 |
| **Instructions** | 1) ErrorView 위젯 2) AsyncValue.error 시 ErrorView 3) Android usesCleartextTraffic(dev만) 4) iOS NSLocationWhenInUseUsageDescription |
| **Output Format** | ErrorView, AndroidManifest, Info.plist |
| **Constraints** | 07-platform-flutter §7.3.2, §7.3.7 |
| **Done When** | 에러 시 사용자 친화적 메시지, 플랫폼 권한 설정 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-flutter §7.3.2, §7.3.7 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 19: 테스트·오프라인(선택)

| 필드 | 내용 |
|------|------|
| **Step Name** | 테스트·오프라인(선택) |
| **Step Goal** | 테스트 폴더 구조와 Notifier 테스트를 작성한다. 오프라인 지원은 선택 적용한다. |
| **Input** | Step 01~18 |
| **Scope** | 포함: test/ lib 미러 구조, Notifier 테스트 / 제외: E2E |
| **Instructions** | 1) test/features/, test/core/ 2) ProviderContainer, FakeRepository 3) (선택) Isar/Hive 오프라인 캐시 |
| **Output Format** | test/ 디렉터리 |
| **Constraints** | 07-platform-flutter §7.3.10, §7.3.13 |
| **Done When** | 테스트 실행 통과, (선택) 오프라인 시 캐시 표시 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-flutter §7.3.10, §7.3.13 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 20: 최종 점검·규칙 준수

| 필드 | 내용 |
|------|------|
| **Step Name** | 최종 점검·규칙 준수 |
| **Step Goal** | RULE 준수 여부를 점검하고 누락 사항을 보완한다. |
| **Input** | Step 01~19, [부록 C 체크리스트](../rules/appendix-c-checklist.md) |
| **Scope** | 포함: Flutter 규칙 체크, 보안 체크 / 제외: 신규 기능 |
| **Instructions** | 1) flutter_secure_storage 사용 2) Dio 공통 인스턴스 3) Riverpod 3.0+ 4) 위젯 200줄 이하 |
| **Output Format** | 체크리스트 완료 |
| **Constraints** | RULE appendix-c |
| **Done When** | 체크리스트 항목 충족 |
| **Duration** | 0.5일 |
| **RULE Reference** | appendix-c-checklist |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log, doc/post-dev/12-user-manual 초안 |

---

## 3. 규칙 준수 체크포인트

- [ ] SharedPreferences 대신 flutter_secure_storage (토큰)
- [ ] Dio 공통 인스턴스, AuthInterceptor
- [ ] AppException, 사용자 친화적 에러 메시지
- [ ] Riverpod 3.0+ (@riverpod, AsyncNotifier)
- [ ] Feature-based 폴더, 위젯 200줄 이하
