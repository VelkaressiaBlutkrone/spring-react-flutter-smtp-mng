# Task: Frontend (React Web)

> **참조 문서**: [PRD](../PRD.md) | [RULE](../RULE.md) | [DOC_MAP](../DOC_MAP.md) | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) | [07-platform-react](../rules/07-platform-react.md)

---

> **문서 작성**: 각 Step의 `선행 문서` 확인 후 작업, `후속 문서`를 `doc/`에 산출. [13-document-guide](../rules/13-document-guide.md) 준수.

---

## 1. 작업 개요

| 항목 | 내용 |
|------|------|
| 기술 스택 | React, Axios, React Router, Zustand, React Query |
| 개발 기간 | 12주 (PRD §12, Week 11~12 집중) |
| Step 구조 | [04-quality §4.3.2](../rules/04-quality.md#432-task-문서-작성-구조-doctaskmd) 필수 10개 필드 준수 |

---

## 2. Step 목록 (총 20단계)

---

### Step 01: 프로젝트 생성·폴더 구조

| 필드 | 내용 |
|------|------|
| **Step Name** | 프로젝트 생성·폴더 구조 |
| **Step Goal** | React 프로젝트와 Feature-based + Atomic Design 폴더 구조를 생성한다. |
| **Input** | Vite 또는 CRA, PRD §8.1 |
| **Scope** | 포함: frontend/ 생성, api/, components/(atoms/molecules/organisms), pages/, store/, hooks/, utils/, types/ / 제외: 비즈니스 코드 |
| **Instructions** | 1) Vite + React + TypeScript 프로젝트 생성 2) 07-platform-react §7.2.3 폴더 구조 적용 3) 기본 라우트 스켈레톤 |
| **Output Format** | `frontend/` 디렉터리, `src/` 폴더 구조 |
| **Constraints** | 07-platform-react §7.2.3 |
| **Done When** | `npm run dev` 실행 시 빈 앱 표시 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-react §7.2.3 |
| **선행 문서** | doc/pre-dev/04-architecture, doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 02: 스타일·ESLint·Prettier

| 필드 | 내용 |
|------|------|
| **Step Name** | 스타일·ESLint·Prettier |
| **Step Goal** | Tailwind CSS, clsx, tailwind-merge, cn() 유틸과 ESLint·Prettier를 설정한다. |
| **Input** | Step 01 프로젝트 |
| **Scope** | 포함: Tailwind, clsx, tailwind-merge, cn(), ESLint, Prettier / 제외: 컴포넌트 스타일 |
| **Instructions** | 1) Tailwind CSS 설치·설정 2) clsx, tailwind-merge, cn() 유틸 3) ESLint extends (prettier 마지막) 4) react/prop-types off |
| **Output Format** | tailwind.config, postcss.config, utils/cn.ts, .eslintrc, .prettierrc |
| **Constraints** | 07-platform-react §7.2.6, §7.2.7 |
| **Done When** | cn() 사용 가능, ESLint·Prettier 동작 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-react §7.2.6, §7.2.7 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 03: 공통 Axios·토큰 인터셉터

| 필드 | 내용 |
|------|------|
| **Step Name** | 공통 Axios·토큰 인터셉터 |
| **Step Goal** | 공통 Axios 인스턴스와 토큰 자동 주입·401/403 처리 인터셉터를 구현한다. |
| **Input** | Step 01, Backend API Base URL |
| **Scope** | 포함: apiClient, request 인터셉터(Bearer), response 인터셉터(401→login, 403→처리) / 제외: authStore |
| **Instructions** | 1) axios.create(baseURL, withCredentials) 2) request: Authorization Bearer 주입 3) response: 401 리다이렉트, 403 처리 |
| **Output Format** | `api/client.ts` |
| **Constraints** | 07-platform-react §7.2.2, 06-auth-token |
| **Done When** | apiClient로 요청 시 토큰 자동 주입, 401 시 로그인 페이지 이동 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-react §7.2.2 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 04: 인증 상태·ErrorBoundary

| 필드 | 내용 |
|------|------|
| **Step Name** | 인증 상태·ErrorBoundary |
| **Step Goal** | Zustand authStore와 ErrorBoundary를 구성한다. |
| **Input** | Step 01~03 |
| **Scope** | 포함: authStore(accessToken, user), 토큰 저장(메모리 또는 HttpOnly 쿠키), ErrorBoundary / 제외: 로그인 UI |
| **Instructions** | 1) Zustand authStore 2) 토큰 저장: 메모리 또는 HttpOnly 쿠키(localStorage 금지) 3) ErrorBoundary 루트 배치 |
| **Output Format** | store/authStore.ts, components/ErrorBoundary.tsx |
| **Constraints** | 06-auth-token §6.1.6, 07-platform-react §7.2.1, §7.2.10 |
| **Done When** | authStore 상태 변경·ErrorBoundary 폴백 표시 확인 |
| **Duration** | 0.5일 |
| **RULE Reference** | 07-platform-react §7.2.9, §7.2.10, 06-auth-token |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 05: Auth API 모듈

| 필드 | 내용 |
|------|------|
| **Step Name** | Auth API 모듈 |
| **Step Goal** | login, register, refresh API 함수를 apiClient 기반으로 구현한다. |
| **Input** | Step 03~04, PRD §5.1 |
| **Scope** | 포함: auth.login, auth.register, auth.refresh / 제외: UI |
| **Instructions** | 1) api/auth.ts 2) login, register, refresh 함수 3) apiClient 사용 |
| **Output Format** | `api/auth.ts` |
| **Constraints** | 07-platform-react §7.2.2 |
| **Done When** | login·register·refresh 호출 시 Backend 연동 |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §5.1 |
| **선행 문서** | doc/pre-dev/06-api-spec (Auth) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 06: 로그인·회원가입 페이지

| 필드 | 내용 |
|------|------|
| **Step Name** | 로그인·회원가입 페이지 |
| **Step Goal** | /login, /register 페이지와 인증 라우트 가드를 구현한다. |
| **Input** | Step 04~05, PRD §8.1 |
| **Scope** | 포함: LoginPage, RegisterPage, 인증 가드(미인증 시 /login) / 제외: 프로필 |
| **Instructions** | 1) LoginPage, RegisterPage 2) authStore 연동 3) React Router 가드 4) dangerouslySetInnerHTML 금지 |
| **Output Format** | pages/LoginPage.tsx, RegisterPage.tsx, 라우트 설정 |
| **Constraints** | 07-platform-react §7.2.1 |
| **Done When** | 로그인·회원가입 후 인증 상태 반영, 미인증 시 /login 리다이렉트 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §8.1 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 07: 프로필 페이지

| 필드 | 내용 |
|------|------|
| **Step Name** | 프로필 페이지 |
| **Step Goal** | 프로필 조회·수정 페이지를 구현한다. |
| **Input** | Step 05~06, PRD §5.2 |
| **Scope** | 포함: ProfilePage, GET/PUT profiles API / 제외: 친구·팔로우 UI |
| **Instructions** | 1) api/profiles.ts 2) ProfilePage (조회·수정) 3) /profile/:userId 라우트 |
| **Output Format** | api/profiles.ts, pages/ProfilePage.tsx |
| **Constraints** | 07-platform-react §7.2.4 (150줄 이하) |
| **Done When** | 프로필 조회·수정 동작 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §5.2 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 08: Posts API·피드 페이지

| 필드 | 내용 |
|------|------|
| **Step Name** | Posts API·피드 페이지 |
| **Step Goal** | posts API 모듈과 피드 페이지를 구현한다. |
| **Input** | Step 03, PRD §5.3 |
| **Scope** | 포함: api/posts.ts (feed, get, create, update, delete), FeedPage, React Query / 제외: 게시물 작성 폼 |
| **Instructions** | 1) api/posts.ts 2) useQuery feed 3) FeedPage, PostCard(Molecules) 4) 서버 상태 React Query |
| **Output Format** | api/posts.ts, pages/FeedPage.tsx, components/molecules/PostCard.tsx |
| **Constraints** | 07-platform-react §7.2.9 |
| **Done When** | 피드 목록 표시, 페이지네이션 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.3, 07-platform-react §7.2.9 |
| **선행 문서** | doc/pre-dev/06-api-spec (Social) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 09: 게시물 작성·상세

| 필드 | 내용 |
|------|------|
| **Step Name** | 게시물 작성·상세 |
| **Step Goal** | 게시물 작성 폼(텍스트·이미지)과 상세 페이지를 구현한다. |
| **Input** | Step 08, PRD SM-03 |
| **Scope** | 포함: PostCreateForm, PostDetailPage, 댓글 목록·작성 / 제외: 해시태그 |
| **Instructions** | 1) PostCreateForm (multipart) 2) PostDetailPage 3) CommentList, CommentForm 4) useMutation |
| **Output Format** | PostCreateForm, PostDetailPage, CommentList, CommentForm |
| **Constraints** | 07-platform-react §7.2.4 |
| **Done When** | 게시물 작성·상세·댓글 CRUD 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.3 |
| **선행 문서** | doc/pre-dev/06-api-spec (Social) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 10: 해시태그·친구·팔로우

| 필드 | 내용 |
|------|------|
| **Step Name** | 해시태그·친구·팔로우 |
| **Step Goal** | 해시태그 API·UI와 친구·팔로우 API·UI를 구현한다. |
| **Input** | Step 07~09, PRD §5.2, §5.3 |
| **Scope** | 포함: api/hashtags, api/friends, api/follows, HashtagTrending, 친구 목록·팔로우 버튼, 공개 범위 선택 / 제외: 채팅 |
| **Instructions** | 1) api/hashtags.ts, api/friends.ts, api/follows.ts 2) HashtagTrending, HashtagPostsPage 3) ProfilePage 내 친구·팔로우 4) PostCreateForm 공개 범위 |
| **Output Format** | api 모듈, HashtagTrending, ProfilePage 확장 |
| **Constraints** | PRD SM-02, SM-04, SM-05 |
| **Done When** | 해시태그 트렌드·클릭 시 게시물, 친구·팔로우·공개 범위 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.2, §5.3 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 11: Travels API·여행 목록

| 필드 | 내용 |
|------|------|
| **Step Name** | Travels API·여행 목록 |
| **Step Goal** | travels API 모듈과 여행 목록·생성 페이지를 구현한다. |
| **Input** | Step 03, PRD §5.4, §8.1 |
| **Scope** | 포함: api/travels.ts, TravelListPage, TravelCreatePage / 제외: 상세·일정 편집 |
| **Instructions** | 1) api/travels.ts 2) TravelListPage 3) TravelCreatePage 4) /travels, /travels/new 라우트 |
| **Output Format** | api/travels.ts, TravelListPage, TravelCreatePage |
| **Constraints** | 07-platform-react |
| **Done When** | 여행 목록·생성 동작 |
| **Duration** | 1일 |
| **RULE Reference** | PRD §5.4, §8.1 |
| **선행 문서** | doc/pre-dev/06-api-spec (Travel) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 12: 여행 상세·일정·예산

| 필드 | 내용 |
|------|------|
| **Step Name** | 여행 상세·일정·예산 |
| **Step Goal** | 여행 상세 페이지와 일정 편집·예산·지출 UI를 구현한다. |
| **Input** | Step 11, PRD TP-02, TP-05 |
| **Scope** | 포함: TravelDetailPage, ItineraryEditor, BudgetSection / 제외: 날씨·리뷰 |
| **Instructions** | 1) TravelDetailPage 2) ItineraryEditor (날짜별) 3) BudgetSection (예산·지출) |
| **Output Format** | TravelDetailPage, ItineraryEditor, BudgetSection |
| **Constraints** | PRD §8.1 |
| **Done When** | 여행 상세·일정·예산 표시·편집 동작 |
| **Duration** | 2일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 13: 목적지 검색·날씨·리뷰

| 필드 | 내용 |
|------|------|
| **Step Name** | 목적지 검색·날씨·리뷰 |
| **Step Goal** | 목적지 검색, 날씨 위젯, 리뷰 UI를 구현한다. |
| **Input** | Step 12, PRD §5.4 |
| **Scope** | 포함: DestinationSearch, WeatherWidget, ReviewSection / 제외: 교통 |
| **Instructions** | 1) DestinationSearch (API 연동) 2) WeatherWidget 3) ReviewSection (작성·조회) 4) TravelShareSettings |
| **Output Format** | DestinationSearch, WeatherWidget, ReviewSection |
| **Constraints** | PRD TP-01, TP-03, TP-06, TP-07 |
| **Done When** | 목적지 검색·날씨·리뷰·공유 설정 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §5.4 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 14: Chat API·채팅 목록

| 필드 | 내용 |
|------|------|
| **Step Name** | Chat API·채팅 목록 |
| **Step Goal** | chat API 모듈과 채팅 목록 페이지를 구현한다. |
| **Input** | Step 03, PRD §5.5 |
| **Scope** | 포함: api/chat.ts (rooms, messages), ChatListPage / 제외: 실시간 메시지 |
| **Instructions** | 1) api/chat.ts 2) ChatListPage 3) /chat 라우트 |
| **Output Format** | api/chat.ts, ChatListPage |
| **Constraints** | PRD §5.5 |
| **Done When** | 채팅방 목록 표시 |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §5.5 |
| **선행 문서** | doc/pre-dev/06-api-spec (Chat) |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 15: WebSocket·실시간 채팅

| 필드 | 내용 |
|------|------|
| **Step Name** | WebSocket·실시간 채팅 |
| **Step Goal** | SockJS + STOMP 클라이언트와 실시간 메시지 송수신을 구현한다. |
| **Input** | Step 14, PRD §6 |
| **Scope** | 포함: useWebSocket, stompClient, ChatRoomPage, 실시간 메시지 / 제외: 알림 |
| **Instructions** | 1) SockJS + STOMP 연결 2) useWebSocket 훅 3) ChatRoomPage 4) /topic/room/{roomId} 구독, /app/chat.send 발행 |
| **Output Format** | hooks/useWebSocket.ts, ChatRoomPage, ChatMessageList |
| **Constraints** | PRD §6.2 |
| **Done When** | 실시간 메시지 송수신 동작 |
| **Duration** | 1.5일 |
| **RULE Reference** | PRD §6 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 16: 검색 페이지

| 필드 | 내용 |
|------|------|
| **Step Name** | 검색 페이지 |
| **Step Goal** | 게시물·해시태그·목적지 통합 검색 페이지를 구현한다. |
| **Input** | Step 08~13, PRD CM-03 |
| **Scope** | 포함: SearchPage, SearchResults (게시물·해시태그·목적지) / 제외: 고급 검색 |
| **Instructions** | 1) SearchPage 2) 검색 API 연동 3) 탭 또는 통합 결과 |
| **Output Format** | SearchPage, SearchResults |
| **Constraints** | PRD §8.1 |
| **Done When** | 검색 시 게시물·해시태그·목적지 결과 표시 |
| **Duration** | 1일 |
| **RULE Reference** | PRD CM-03 |
| **선행 문서** | doc/pre-dev/06-api-spec |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 17: 공통 컴포넌트·레이아웃

| 필드 | 내용 |
|------|------|
| **Step Name** | 공통 컴포넌트·레이아웃 |
| **Step Goal** | Atoms, Molecules, Organisms와 반응형 레이아웃을 구성한다. |
| **Input** | Step 01~16 |
| **Scope** | 포함: Button, Input, Badge(Atoms), Header, Sidebar(Organisms), Layout / 제외: 페이지 전용 |
| **Instructions** | 1) atoms: Button, Input, Badge 2) organisms: Header, Sidebar 3) Layout (반응형) |
| **Output Format** | components/atoms/, molecules/, organisms/, Layout |
| **Constraints** | 07-platform-react §7.2.3 |
| **Done When** | 공통 컴포넌트 재사용, 레이아웃 적용 |
| **Duration** | 1일 |
| **RULE Reference** | 07-platform-react §7.2.3 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 18: 랜딩·에러 처리

| 필드 | 내용 |
|------|------|
| **Step Name** | 랜딩·에러 처리 |
| **Step Goal** | 랜딩 페이지와 전역 에러 처리(401/403, async try-catch)를 완성한다. |
| **Input** | Step 03~04, Step 17 |
| **Scope** | 포함: LandingPage (/), 401/403 전역 처리, async/await try-catch / 제외: 상세 에러 UI |
| **Instructions** | 1) LandingPage 2) apiClient 401/403 처리 보완 3) async 함수 try-catch 4) var 금지, const > let |
| **Output Format** | LandingPage, api/client.ts 보완 |
| **Constraints** | 08-javascript, 07-platform-react §7.2.2 |
| **Done When** | 랜딩 표시, 401/403 시 적절한 리다이렉트·메시지 |
| **Duration** | 0.5일 |
| **RULE Reference** | 08-javascript, 07-platform-react |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 19: 좋아요·북마크(선택)·환경 변수

| 필드 | 내용 |
|------|------|
| **Step Name** | 좋아요·북마크(선택)·환경 변수 |
| **Step Goal** | 좋아요·북마크 UI(선택)와 환경 변수 설정을 완료한다. |
| **Input** | Step 08~09, PRD §14 |
| **Scope** | 포함: LikeButton, BookmarkButton(선택), VITE_API_BASE_URL 등 .env / 제외: 필수 아님 |
| **Instructions** | 1) LikeButton, BookmarkButton (API 연동) 2) .env.example, VITE_* 변수 3) .gitignore .env |
| **Output Format** | LikeButton, BookmarkButton, .env.example |
| **Constraints** | RULE 보안 |
| **Done When** | 환경 변수로 API URL 설정, 좋아요/북마크 동작(선택) |
| **Duration** | 0.5일 |
| **RULE Reference** | PRD §14 |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log |

---

### Step 20: 최종 점검·규칙 준수

| 필드 | 내용 |
|------|------|
| **Step Name** | 최종 점검·규칙 준수 |
| **Step Goal** | RULE 준수 여부를 점검하고 누락 사항을 보완한다. |
| **Input** | Step 01~19, [부록 C 체크리스트](../rules/appendix-c-checklist.md) |
| **Scope** | 포함: React·JS 규칙 체크, 보안 체크 / 제외: 신규 기능 |
| **Instructions** | 1) dangerouslySetInnerHTML 미사용 2) localStorage 토큰 미사용 3) Functional Component, 150줄 이하 4) React Query·Zustand 사용 |
| **Output Format** | 체크리스트 완료 |
| **Constraints** | RULE appendix-c |
| **Done When** | 체크리스트 항목 충족 |
| **Duration** | 0.5일 |
| **RULE Reference** | appendix-c-checklist |
| **선행 문서** | — |
| **후속 문서** | doc/during-dev/08-work-log, doc/post-dev/12-user-manual 초안 |

---

## 3. 규칙 준수 체크포인트

- [ ] dangerouslySetInnerHTML 금지, 토큰 메모리/HttpOnly, localStorage 금지
- [ ] 공통 Axios + 토큰 인터셉터, var 금지, const > let
- [ ] async/await, try-catch, ErrorBoundary
- [ ] Functional Component, Props interface, 150줄 이하
- [ ] 서버 상태 React Query, 클라이언트 Zustand
