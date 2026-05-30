---
title: 커뮤니티 게시판 (Spring Boot 4 / Thymeleaf)
status: todo
labels: [ready-for-agent]
dependencies: []
---

# 커뮤니티 게시판 PRD

용어는 루트 `CONTEXT.md`의 글로서리(Member, Admin, Board, Post, Comment, Recommendation, View count)를 따른다.

## Problem Statement

회원들이 글을 쓰고, 서로의 글과 댓글에 의견을 남기고 반응(추천)할 수 있는 커뮤니티 공간이 없다.
또한 이를 올릴 Spring Boot 백엔드 프로젝트 골격 자체가 아직 없다.

## Solution

`back/` 폴더에 Spring Boot 4 기반 서버를 세우고, 서버사이드 렌더링(Thymeleaf)으로 동작하는
커뮤니티 게시판을 제공한다. 비로그인 방문자도 글·댓글을 읽을 수 있고, 로그인한 Member는
글/댓글을 쓰고 수정·삭제하며, 글과 댓글에 추천을 보낼 수 있다. 글에는 조회수와 추천수가 표시된다.

## User Stories

### 프로젝트 세팅
1. 개발자로서, `back/` 폴더에 Spring Boot 4.0.6 / JDK 25 / Gradle(Kotlin DSL) 프로젝트가 세팅되어 있길 원한다. 바로 개발을 시작할 수 있도록.
2. 개발자로서, 루트 패키지가 `com.back`, 메인 클래스가 `com.back.BackApplication`이길 원한다. 구조가 예측 가능하도록.
3. 개발자로서, 개발 모드에서 H2 파일 DB(`./db_dev.mv.db`)와 `/h2-console`을 쓰길 원한다. 데이터를 눈으로 확인하며 개발하도록.
4. 개발자로서, 테스트 모드에서 H2 인메모리 DB를 쓰길 원한다. 테스트가 서로 격리되고 빠르도록.
5. 개발자로서, 모든 엔티티가 작성일시·수정일시를 자동으로 갖길 원한다(BaseEntity 상속 + JPA Auditing). 감사 정보를 일일이 안 적도록.
6. 개발자로서, OSIV가 꺼져 있고 서비스(액션) 메서드에 `@Transactional`이 걸리길 원한다. 커넥션 점유와 트랜잭션 경계가 명확하도록.
7. 개발자로서, 앱 첫 기동 시 회원이 없으면 샘플 데이터(회원 5, 글 5, 댓글 5)가 생성되길 원한다. 빈 화면 대신 바로 둘러보도록.

### 인증 / 회원
8. 방문자로서, username·password·nickname으로 회원가입하고 싶다. 서비스를 이용하도록.
9. 방문자로서, 이미 쓰인 username/nickname으로는 가입이 거부되길 원한다. 식별자가 충돌하지 않도록.
10. Member로서, username·password로 로그인하고 싶다. 내 계정으로 활동하도록.
11. Member로서, 로그아웃하고 싶다. 공용 PC에서 계정을 보호하도록.
12. Member로서, '내 정보' 페이지에서 내 정보를 보고 싶다. 현재 상태를 확인하도록.
13. Member로서, '내 정보'에서 nickname과 password를 수정하고 싶다(username은 불변). 표시 이름과 비밀번호를 갱신하도록.

### 게시판 / 글
14. 방문자로서, 게시판(공지·자유·질문) 목록을 보고 싶다. 관심 분류로 이동하도록.
15. 방문자로서, 한 Board의 글 목록을 최신순 페이지네이션으로 보고 싶다. 글이 많아도 탐색하도록.
16. 방문자로서, 글 목록에서 제목으로 검색하고 싶다. 원하는 글을 빨리 찾도록.
17. 방문자로서, 글 상세에서 제목·본문·작성자·작성일시·조회수·추천수·댓글을 보고 싶다. 내용을 파악하도록.
18. Member로서, 자유·질문 Board에 글을 쓰고 싶다. 의견을 공유하도록.
19. Admin으로서, 공지 Board에 글을 쓰고 싶다(일반 Member는 불가). 공지를 전달하도록.
20. Member로서, 내가 쓴 글을 수정·삭제하고 싶다. 실수를 바로잡도록.
21. Admin으로서, 어떤 글이든 수정·삭제하고 싶다. 부적절한 글을 관리하도록.
22. 방문자로서, 같은 세션에서 같은 글을 다시 열어도 조회수가 한 번만 오르길 원한다. 조회수가 부풀지 않도록.

### 댓글
23. Member로서, 글에 댓글을 달고 싶다. 반응을 남기도록.
24. Member로서, 다른 댓글에 답글(대댓글)을 달고 싶다. 특정 댓글에 이어가도록.
25. Member로서, 답글에는 다시 답글을 달 수 없길 원한다(최대 깊이 2). 스레드가 무한히 깊어지지 않도록.
26. Member로서, 내 댓글을 수정·삭제하고 싶다. 실수를 바로잡도록.
27. Member로서, 답글이 달린 내 댓글을 지우면 본문은 "삭제된 댓글입니다"로 남고 답글은 유지되길 원한다. 대화 맥락이 깨지지 않도록.
28. Member로서, 답글이 없는 내 댓글은 흔적 없이 삭제되길 원한다. 화면이 깔끔하도록.
29. Admin으로서, 어떤 댓글이든 수정·삭제하고 싶다. 부적절한 댓글을 관리하도록.

### 추천
30. Member로서, 글/댓글에 추천을 보내고 싶다. 좋은 글에 반응하도록.
31. Member로서, 추천을 다시 누르면 취소되길 원한다(토글). 마음이 바뀌면 되돌리도록.
32. Member로서, 같은 글/댓글에 추천이 중복 집계되지 않길 원한다. 추천수가 정직하도록.
33. Member로서, 내가 쓴 글/댓글에는 추천할 수 없길 원한다. 자기추천을 막도록.
34. 방문자로서, 글/댓글의 추천수를 보고 싶다. 인기를 가늠하도록.

## Implementation Decisions

### 스택 / 기반
- Spring Boot **4.0.6**, **JDK 25**, **Gradle Kotlin DSL**. 위치: `back/`.
- 의존성: DevTools, Lombok, Spring Data JPA, Validation, Spring Security, H2, Thymeleaf, thymeleaf-layout-dialect.
- 루트 패키지 `com.back`, 메인 `com.back.BackApplication`(`@EnableJpaAuditing`).
- 패키지 구조: 도메인별(`com.back.domain.member`, `.post`, `.comment`, `.recommend`) + 전역(`com.back.global.*` — security, initData, jpa, exception 등).
- **OSIV 끔** (`spring.jpa.open-in-view: false`). 영속성 컨텍스트는 서비스 `@Transactional` 경계 안에서만 산다. 뷰에서 LAZY 예외가 나지 않도록, 서비스가 필요한 연관을 fetch join/`@EntityGraph`로 미리 로딩하거나 DTO로 변환해 반환한다.
- 모든 엔티티는 `BaseEntity`(id, createDate, modifyDate; `@CreatedDate`/`@LastModifiedDate`, `@MappedSuperclass`)를 상속.

### 프로필 / DB
- `application.yml`(공통) + `application-dev.yml` + `application-test.yml`.
- dev: H2 파일 DB `./db_dev.mv.db`, `ddl-auto: update`, `/h2-console` 활성.
- test: H2 인메모리, `ddl-auto: create`.

### 도메인 모델
- **Member**: username(unique, 로그인 식별자), password(BCrypt 인코딩), nickname(unique), role(`ROLE_USER`|`ROLE_ADMIN`).
- **Board**: name. 공지·자유·질문 3개로 고정(시드). CRUD UI 없음.
- **Post**: board(N:1), author=Member(N:1), title, content, viewCount(int). 추천수는 Recommendation COUNT로 산출.
- **Comment**: post(N:1), author=Member(N:1), parent=Comment(self N:1, nullable), content, deleted(boolean) — soft delete 표식. 최대 깊이 2(parent가 있는 댓글은 parent를 가질 수 없음).
- **Recommendation**: 대상 종류별로 분리(PostRecommendation: post+member unique / CommentRecommendation: comment+member unique). member(N:1) 포함. 토글 = 있으면 삭제, 없으면 생성.

### 모듈 / 인터페이스 (deep module 지향)
- **MemberService**: `join(username, password, nickname)`(중복 검증 + 비번 인코딩), `modify(member, nickname, password)`, 조회. uniqueness·인코딩 규칙을 캡슐화.
- **PostService**: 글 CRUD, 권한 검사(작성자 또는 Admin), 공지 Board 글쓰기 Admin 제약, 목록(Pageable + 제목 검색), 조회수 증가 로직. *세션 기반 중복 방지*는 컨트롤러에서 HttpSession에 본 Post id 집합을 두고, 처음 보는 경우에만 `increaseViewCount`를 호출(서비스는 "올려라"만 책임 → 단위 테스트 가능).
- **CommentService**: 댓글/답글 작성(깊이 2 강제), 수정, 삭제(답글 있으면 soft delete=tombstone, 없으면 hard delete), 권한 검사.
- **RecommendationService**: `togglePost(member, post)`, `toggleComment(member, comment)` — 자기추천 차단, unique 제약으로 중복 방지, 토글 생성/삭제. 대상의 추천수 count 제공.
- **global.security**: Spring Security 폼 로그인, `BCryptPasswordEncoder`, `UserDetailsService`(username 기반), 인가 규칙(읽기 공개, 쓰기/추천/수정/삭제 인증 필요), 로그아웃.
- **global.initData.BaseInitData**: `baseInitDataApplicationRunner`(`@Transactional`). 회원이 1명이라도 있으면 즉시 중단. 없으면 Board 3개, 회원 5명(시드1=Admin nickname "관리자", 2~5 일반), 글 5개(여러 Board 분산, 공지글은 Admin 작성), 댓글 5개(일부 대댓글 포함), 일부 추천 시드.

### 인가 규칙 요약
- 읽기(Board·Post·Comment 목록/상세): 공개.
- 쓰기/추천/수정/삭제: 인증 필요.
- Post/Comment 수정·삭제: 작성자 본인 또는 ROLE_ADMIN.
- 공지 Board 글쓰기: ROLE_ADMIN만.
- 자기추천: 금지.

### 화면 (Thymeleaf + layout dialect)
- 공통 레이아웃: 헤더(네비/로그인 상태), thymeleaf-layout-dialect로 상속.
- Tailwind 4.x Play CDN(`@tailwindcss/browser@4`) + daisyUI 5(CDN) + Pretendard 다이나믹 서브셋 웹폰트.
- 페이지: 회원가입, 로그인, 내 정보(조회+수정), Board별 글 목록(페이지네이션·제목검색), 글 상세(댓글/대댓글/추천 포함), 글 작성/수정, 댓글 작성/수정.

## Testing Decisions

- 좋은 테스트는 **외부 행동만** 검증한다(서비스의 public 메서드 입출력·예외·DB 결과). private 메서드나 구현 세부에 결합하지 않는다.
- 테스트 대상(전부): **MemberService, PostService, CommentService, RecommendationService**.
  - MemberService: username/nickname 중복 시 가입 거부, 비번이 인코딩되어 저장됨, modify가 nickname/password만 바꾸고 username 불변.
  - PostService: 비작성자 수정·삭제 거부, Admin은 허용, 공지 Board에 일반 Member 글쓰기 거부, 제목 검색·페이지네이션 결과, 조회수 증가.
  - CommentService: 깊이 2 초과(답글에 답글) 거부, 답글 있는 댓글 삭제 시 tombstone(deleted=true, 답글 보존), 답글 없는 댓글은 실제 삭제, 권한.
  - RecommendationService: 토글(생성→삭제), 중복 추천 차단, 자기추천 차단, 추천수 count 정확.
- 방식: `@SpringBootTest` + `application-test.yml`(H2 인메모리, ddl-auto=create). 각 테스트는 트랜잭션 격리. 프로젝트 신규라 prior art는 없음 — 이 PRD의 테스트가 첫 표준이 된다.

## Out of Scope

- 프론트엔드 SPA(React 등) — 이번엔 Thymeleaf SSR만.
- Board의 동적 CRUD(관리 UI) — 3개 고정 시드.
- 대대댓글(깊이 3 이상), 댓글 추천 랭킹, 글 정렬 옵션(추천순 등) — 최신순만.
- 파일/이미지 업로드, 멘션, 알림, 검색엔진(전문검색) — 제목 LIKE 검색만.
- 비밀번호 재설정/이메일 인증/소셜 로그인.
- REST JSON API, 배포/CI.

## Further Notes

- 자세한 용어 정의는 루트 `CONTEXT.md` 참조.
- Spring Boot 4.0 / JDK 25 / Security 7은 비교적 신규 — thymeleaf-layout-dialect, Security DSL 호환 버전 확인이 세팅 단계의 첫 리스크.
- 다음 단계: `/to-issues`로 이 PRD를 tracer-bullet 수직 슬라이스 이슈들로 분해.
