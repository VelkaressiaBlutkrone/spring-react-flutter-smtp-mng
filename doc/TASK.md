# TASK 요약

> **상세 문서**: [tasks/task_server.md](tasks/task_server.md) | [tasks/task_frontend.md](tasks/task_frontend.md) | [tasks/task_mobile.md](tasks/task_mobile.md)  
> **참조**: [PRD](PRD.md) | [RULE](RULE.md) | [DOC_MAP](DOC_MAP.md) | [13-document-guide](rules/13-document-guide.md)

---

## 1. 전체 개요

| 구분 | 기술 스택 | Step 수 | 개발 기간 |
|------|-----------|---------|-----------|
| **Server** | Spring Boot 3.3+, Java 21, JWT, JPA+QueryDSL, WebSocket | 20 | 12주 |
| **Frontend** | React, Axios, Zustand, React Query | 20 | 12주 (Week 11~12 집중) |
| **Mobile** | Flutter 3.x, Riverpod 3.0+, Dio, GoRouter | 20 | 12주 (Week 11~12 집중) |

---

## 2. Server (Backend) 요약

| 단계 | Step | 내용 | 예상 |
|------|------|------|------|
| 1 | 01~02 | 기획·설계, 프로젝트·패키지 구조 | 3일 |
| 2 | 03~04 | DB 스키마, 환경 변수·시크릿 | 2.5일 |
| 3 | 05~07 | JWT 인증·필터·Security, GlobalExceptionHandler | 4.5일 |
| 4 | 08~09 | User·Profile, Auth API | 4일 |
| 5 | 10~12 | Post·Comment·게시물, 댓글·해시태그·피드, 친구·팔로우 | 5.5일 |
| 6 | 13~14 | 채팅 엔티티·WebSocket, 채팅 REST·실시간 | 3.5일 |
| 7 | 15~17 | 여행·일정·목적지, 예산·날씨, 리뷰·공유 | 5.5일 |
| 8 | 18~20 | Rate Limiting·로깅, API 문서·테스트, 환경 분리 | 4일 |

**핵심 산출물**: `backend/`, API 명세(Swagger), ERD, 단위 테스트

---

## 3. Frontend (React Web) 요약

| 단계 | Step | 내용 | 예상 |
|------|------|------|------|
| 1 | 01~04 | 프로젝트·폴더, 스타일·ESLint, Axios·인터셉터, 인증·ErrorBoundary | 2일 |
| 2 | 05~07 | Auth API, 로그인·회원가입, 프로필 | 2.5일 |
| 3 | 08~10 | Posts·피드, 게시물 작성·상세, 해시태그·친구·팔로우 | 4일 |
| 4 | 11~13 | Travels API, 여행 상세·일정·예산, 목적지·날씨·리뷰 | 4.5일 |
| 5 | 14~16 | Chat API, WebSocket·실시간, 검색 | 3일 |
| 6 | 17~20 | 공통 컴포넌트, 랜딩·에러, 좋아요·환경 변수, 최종 점검 | 2.5일 |

**핵심 산출물**: `frontend/`, 피드·여행·채팅 화면, REST·WebSocket 연동

---

## 4. Mobile (Flutter) 요약

| 단계 | Step | 내용 | 예상 |
|------|------|------|------|
| 1 | 01~04 | 프로젝트·폴더, Secure Storage·환경, Dio·AppException, GoRouter | 2.5일 |
| 2 | 05~07 | Auth API·AuthNotifier, Splash·로그인·회원가입, Main·BottomNav | 3일 |
| 3 | 08~11 | Posts API, Feed·PostCard·PostDetail, 게시물 작성, 프로필 | 4일 |
| 4 | 12~14 | Travels API, 여행 상세·일정·예산, 목적지·날씨·리뷰 | 4.5일 |
| 5 | 15~17 | Chat API, WebSocket·실시간, 공통 위젯·테마 | 3일 |
| 6 | 18~20 | 에러·플랫폼 설정, 테스트·오프라인, 최종 점검 | 2일 |

**핵심 산출물**: `frontend-mobile/`, BottomNav 4탭, REST·WebSocket 연동

---

## 5. 의존 관계

```
Server Step 01~09 (인증·사용자·프로필) 완료
    ↓
Frontend/Mobile Step 05~07 (Auth·로그인·프로필) 연동 가능

Server Step 10~12 (소셜) 완료
    ↓
Frontend/Mobile Step 08~10 (피드·게시물·해시태그) 연동 가능

Server Step 13~14 (채팅) 완료
    ↓
Frontend/Mobile Step 14~16 (채팅) 연동 가능

Server Step 15~17 (여행) 완료
    ↓
Frontend/Mobile Step 11~14 (여행) 연동 가능
```

---

## 6. Step별 문서 작성 (13-document-guide)

각 Step에 **선행 문서**·**후속 문서** 필드가 추가됨. [DOC_MAP](DOC_MAP.md) 참조.

| 구분 | doc/ 폴더 | 문서 |
|------|------------|------|
| **개발 전** | pre-dev/ | 01-proposal, 02-project-plan, 03-requirements, 04-architecture, 05-erd, 06-api-spec |
| **개발 중** | during-dev/ | 07-detailed-design, 08-work-log, 09-change-request, 10-test-case |
| **개발 후** | post-dev/ | 11-test-report, 12-user-manual, 13-deployment-guide, 14-operations-manual |

- **선행 문서**: Step 시작 전 확인·작성할 문서
- **후속 문서**: Step 완료 후 `doc/`에 산출할 문서

---

## 7. Step 구조 (04-quality §4.3.2)

각 Step은 아래 10개 필드를 포함한다.

| 필드 | 설명 |
|------|------|
| Step Name | 단계 이름 |
| Step Goal | 완료 시 달성 목표 |
| Input | 필요한 입력 |
| Scope | 포함/제외 범위 |
| Instructions | 수행 작업 목록 |
| Output Format | 산출물 형태·위치 |
| Constraints | 제약(RULE·기술) |
| Done When | 완료 조건 |
| Duration | 예상 소요일 |
| RULE Reference | 참조 RULE 섹션 |
| 선행 문서 | Step 시작 전 확인·작성할 문서 (doc/ 경로) |
| 후속 문서 | Step 완료 후 doc/에 산출할 문서 |

---

## 8. 규칙 준수 체크포인트

| 구분 | 필수 항목 |
|------|-----------|
| **공통** | 비밀정보 외부 주입, 401/403 구분, 입력 검증 |
| **Server** | JWT alg allow-list·iss/aud·jti, GlobalExceptionHandler, 트랜잭션 Service |
| **Frontend** | dangerouslySetInnerHTML 금지, localStorage 토큰 금지, React Query·Zustand |
| **Mobile** | flutter_secure_storage, Dio 공통 인스턴스, Riverpod 3.0+ |

---

> **상세 Step별 Instructions·Output·Constraints**는 각 [tasks/](tasks/) 문서를 참조한다.
