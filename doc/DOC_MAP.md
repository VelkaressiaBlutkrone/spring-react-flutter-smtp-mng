# 문서-Task Step 매핑 가이드

> **참조**: [13-document-guide](rules/13-document-guide.md) | [TASK](TASK.md)

---

## 1. doc/ 폴더 구조

```
doc/
├── pre-dev/          # 개발 전 문서
│   ├── 01-proposal.md
│   ├── 02-project-plan.md
│   ├── 03-requirements.md
│   ├── 04-architecture.md
│   ├── 05-erd.md
│   └── 06-api-spec.md
├── during-dev/       # 개발 중 문서
│   ├── 07-detailed-design.md
│   ├── 08-work-log.md
│   ├── 09-change-request.md
│   └── 10-test-case.md
├── post-dev/         # 개발 후 문서
│   ├── 11-test-report.md
│   ├── 12-user-manual.md
│   ├── 13-deployment-guide.md
│   └── 14-operations-manual.md
├── tasks/            # Task 상세
├── rules/            # RULE 상세
└── DOC_MAP.md        # 본 문서
```

---

## 2. Server Step ↔ 문서 매핑

| Step | 선행 문서 (Pre) | 후속 문서 (Post) |
|------|-----------------|------------------|
| 01 | 01-proposal, 02-project-plan, 03-requirements | 03-requirements 보완, 05-erd 초안, 06-api-spec 초안 |
| 02 | 04-architecture | TECH-STACK.md, 패키지 구조 문서 |
| 03 | 05-erd | 05-erd (물리 ERD), 테이블 정의서 |
| 04 | — | .env.example, run-dev.ps1 |
| 05~07 | — | 08-work-log (매일) |
| 08~09 | 06-api-spec (Auth·User·Profile) | 06-api-spec 갱신 |
| 10~12 | 06-api-spec (Social) | 07-detailed-design (Social), 08-work-log |
| 13~14 | 06-api-spec (Chat) | 07-detailed-design (Chat), 08-work-log |
| 15~17 | 06-api-spec (Travel) | 07-detailed-design (Travel), 08-work-log |
| 18 | — | 08-work-log |
| 19 | 10-test-case | 06-api-spec (Swagger), 10-test-case 갱신 |
| 20 | — | 13-deployment-guide, README |

---

## 3. Frontend Step ↔ 문서 매핑

| Step | 선행 문서 (Pre) | 후속 문서 (Post) |
|------|-----------------|------------------|
| 01~04 | 04-architecture, 06-api-spec (Base) | 08-work-log |
| 05~07 | 06-api-spec (Auth) | 08-work-log |
| 08~10 | 06-api-spec (Social) | 08-work-log |
| 11~13 | 06-api-spec (Travel) | 08-work-log |
| 14~16 | 06-api-spec (Chat) | 08-work-log |
| 17~20 | — | 08-work-log, 12-user-manual 초안(20) |

---

## 4. Mobile Step ↔ 문서 매핑

| Step | 선행 문서 (Pre) | 후속 문서 (Post) |
|------|-----------------|------------------|
| 01~04 | 04-architecture, 06-api-spec (Base) | 08-work-log |
| 05~07 | 06-api-spec (Auth) | 08-work-log |
| 08~11 | 06-api-spec (Social) | 08-work-log |
| 12~14 | 06-api-spec (Travel) | 08-work-log |
| 15~17 | 06-api-spec (Chat) | 08-work-log |
| 18~20 | — | 08-work-log, 12-user-manual 초안(20) |

---

## 5. 문서 작성 시점 요약

| 문서 | 작성 시점 | 담당 |
|------|-----------|------|
| 01-proposal | 프로젝트 착수 전 | PM/기획 |
| 02-project-plan | Step 01 전 | PM |
| 03-requirements | Step 01 전·Step 01에서 보완 | 기획·개발 |
| 04-architecture | Step 01~02 | 아키텍트 |
| 05-erd | Step 01~03 | 백엔드 |
| 06-api-spec | Step 01~19 (점진적) | 백엔드·협의 |
| 07-detailed-design | Step 10~17 (모듈별) | 개발 |
| 08-work-log | 매일 (Step 04~20) | 개발 |
| 09-change-request | 변경 발생 시 | PM·개발 |
| 10-test-case | Step 19 전·동시 | QA·개발 |
| 11-test-report | Step 19~20 후 | QA |
| 12-user-manual | Step 20 근처 | 개발·기획 |
| 13-deployment-guide | Step 20 | DevOps·개발 |
| 14-operations-manual | Step 20 후 | 운영·개발 |
