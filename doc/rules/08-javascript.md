# 8. 프론트엔드 JavaScript/TypeScript 코딩 규칙 (2026 ver.)

> 프론트엔드 코드 작성 시 본 장(8)을 기본으로 참조한다. React·Vue·Next.js 등 **프레임워크별 세부 규칙은 별도 문서**에서 정의한다. 원본: [RULE.md](../RULE.md) 8장.

---

## TypeScript 기본 원칙 (2026)

- **신규 프로젝트는 TypeScript를 기본으로 한다.** 2025~2026년 대부분의 프론트엔드 프로젝트가 TS 기반이므로 JS-only 프로젝트는 레거시로 간주.
- **ESLint + Prettier + typescript-eslint** 조합 필수. ESLint 9+ **flat config** 사용 권장.
- **tsconfig.json** strict 모드 기본 활성화 권장:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitThis": true,
    "useUnknownInCatchVariables": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "target": "ES2025",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

- **any 타입** 절대 사용 금지. 가능하면 **unknown** 사용 후 타입 가드 필수.
- **as any** 강제 캐스팅 금지. 타입이 불확실한 경우 unknown + 타입 좁히기 패턴 사용.
- **public API**(함수 매개변수·반환값, exported 타입)에는 명시적 타입 어노테이션 적극 권장.
- 타입 추론을 최대한 활용하되, 가독성과 유지보수성을 위해 팀 컨벤션에 따라 명시 여부 결정.

---

## 8.1 변수 & 상수 선언 규칙 (Variables & Constants)

| # | Rule | 비고 |
|---|------|------|
| 1 | **var는 절대 사용하지 않는다** (ES6 이후 완전 금지) | |
| 2 | 재할당이 필요 없는 모든 변수는 **const**로 선언한다. (기본 원칙: const > let) | |
| 3 | 재할당이 반드시 필요한 경우에만 **let**을 사용한다. (for 루프의 i, j 등은 let 허용, for...of/forEach 내부에서는 const 추천) | |
| 4 | const와 let은 **사용 직전에 선언**한다. (hoisting 문제 최소화) | |
| 5 | 같은 스코프 내에서 const를 let보다 위에 선언한다. (가독성) | |
| 6 | **전역 변수는 절대 사용하지 않는다.** (window.xxx, globalThis.xxx 직접 할당 금지. 필요 시 모듈 스코프 또는 별도 상태 관리 도구 사용) | |
| 7 | **네이밍**: 변수/함수 → camelCase, 상수(불변) → UPPER_SNAKE_CASE, 타입·인터페이스·제네릭 → PascalCase, enum → PascalCase (멤버 UPPER_SNAKE_CASE). **private 필드**: **`#privateField`** 권장 (진짜 private) | 예: `const MAX_UPLOAD_SIZE = 10 * 1024 * 1024;` |

---

## 8.2 함수 선언 및 사용 규칙 (Functions)

| # | Rule | 비고 |
|---|------|------|
| 1 | **네이밍**: camelCase, 동사 + 목적어 형태 권장. (좋음: fetchUserData, calculateTotalPrice, handleError / 금지: fn, doIt, x) | |
| 2 | **화살표 함수**를 기본으로 사용 (특히 콜백, 짧은 함수). `function name() {}`은 this 바인딩 필요·재귀 함수·생성자에만 제한적 사용 | |
| 3 | **async 함수**는 반드시 async 키워드 명시 | |
| 4 | 함수는 **한 가지 역할만** 수행. **20~25줄 이상**이면 책임 분리·리팩토링 검토 | |
| 5 | 매개변수 기본값 적극 활용: `function greet(name = 'Guest') {}` | |
| 6 | 반환은 early return 또는 단일 return 중 팀 컨벤션 따름 | |
| 7 | 익명 함수는 거의 사용하지 않는다. (map, filter 등 짧은 화살표 함수는 예외 허용) | |

---

## 8.3 비동기 처리 규칙 (Async/Await 중심)

| # | Rule | 비고 |
|---|------|------|
| 1 | **비동기 처리 기본 방식은 async/await**. `.then().catch()` 체인은 최대한 피한다. 콜백 스타일(callback hell)은 절대 사용하지 않는다. | |
| 2 | Promise를 반환하는 함수는 **await 없이 호출하지 않는다**. (fire-and-forget 금지) | |
| 3 | 병렬 비동기 처리 시 **Promise.all** 적극 활용. 부분 실패 허용 시 **Promise.allSettled** 사용 고려 | `const results = await Promise.allSettled([...])` |
| 4 | fetch 직접 사용할 때 **AbortController** 필수 활용. 타임아웃·취소 wrapper 권장 | |
| 5 | HTTP 클라이언트 라이브러리 사용 시 **ky** 또는 **ofetch** 권장 (fetch 기반). axios는 레거시 프로젝트에서만 유지 | |

---

## 8.4 예외 및 오류 처리 규칙 (Error Handling)

| # | Rule | 비고 |
|---|------|------|
| 1 | **async 함수 내에서는 반드시 try-catch 사용** | |
| 2 | **커스텀 에러 클래스** 적극 활용 (도메인별 구분: AuthError, ValidationError, NetworkError 등) | |
| 3 | **최상위 레벨**에서 전역 에러 핸들링: **unhandledrejection** 이벤트 리스너 등록 + 모니터링 도구(Sentry, Datadog 등) 연결 | |
| 4 | **console.*** 메서드는 개발 환경에서만 사용. 프로덕션 빌드에서는 제거 권장 (eslint-plugin-no-console + 빌드 시 strip 조합) | |
| 5 | **Promise rejection은 반드시 catch 처리**. 예상 가능한 에러(404, 401 등)는 사용자 친화적 메시지로 변환. 예상치 못한 에러는 "알 수 없는 오류가 발생했습니다" + 재시도 제공 | |

---

## 8.5 네이밍 컨벤션 전체 요약

| 대상 | 규칙 | 예시 |
|------|------|------|
| 변수·함수 | camelCase | fetchUserData, handleSubmit |
| 상수(불변) | UPPER_SNAKE_CASE | MAX_UPLOAD_SIZE, API_BASE_URL |
| 타입·인터페이스 | PascalCase | UserProfile, ApiResponse |
| enum | PascalCase (멤버 UPPER) | enum UserRole { ADMIN, GUEST } |
| private 필드 | **#privateField** 권장 | #internalCache |
| 파일명 | kebab-case 또는 camelCase (팀 컨벤션) | user-service.ts, apiClient.ts |

---

## 8.6 금지 패턴 (Do Not)

- ❌ **var** 사용
- ❌ **.then().catch()** 체인 남용 (async/await 우선)
- ❌ **전역 변수** 직접 할당 (window.xxx 등)
- ❌ **fire-and-forget** (await 누락)
- ❌ **any 타입** 사용 또는 **as any** 강제 캐스팅
- ❌ **콜백 지옥** (callback hell)

---

## 8.7 추천 라이브러리 & 패턴 (2026)

| 용도 | 추천 (2026 우선순위) |
|------|----------------------|
| HTTP 클라이언트 | **ky** 또는 **ofetch** (fetch 기반) > axios (레거시) |
| 데이터 검증 | **Zod** (스키마 기본) |
| 에러 모니터링 | Sentry, Datadog |
| 상태 관리 | Zustand ≈ Jotai > Redux Toolkit (레거시) |
| 린트·포맷 | ESLint 9+ flat config + Prettier + typescript-eslint 필수 |
| AI 코드 생성 | Cursor / GitHub Copilot 등 사용 시: 생성 코드 반드시 타입 검사·리팩토링·리뷰 필수 |

---

## 8.8 추가 권장 사항 (2026 트렌드)

### ESLint 추천 플러그인 적극 활성화

- `@typescript-eslint/no-floating-promises`
- `@typescript-eslint/no-misused-promises`
- `@typescript-eslint/prefer-ts-expect-error`
- `no-console` (dev 제외)

### 기타

- **private 필드** `#` 문법 적극 사용 (브라우저 지원율 95% 이상)
- **Zod**를 API 응답·입력 검증의 표준으로 활용 (parse → safeParse 패턴 권장)
- **AI 도구** 활용 시: 생성된 코드는 반드시 타입 강화, 불필요한 any 제거, ESLint 통과 확인 후 커밋

---

## 참고 문서

- [07-platform-react.md](07-platform-react.md) — React 프레임워크별 세부 규칙
- [07-platform-flutter.md](07-platform-flutter.md) — Flutter (Dart 원칙 유사 적용)
