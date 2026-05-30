---
title: 부트스트랩 + 공통 레이아웃 + 홈 페이지
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

`back/` 폴더에 Spring Boot 4 프로젝트 골격을 세우고, 앱이 떠서 공통 레이아웃이 적용된 홈 페이지 한 장을 렌더한다. 이후 모든 슬라이스가 올라탈 기반.

- Spring Boot **4.0.6**, **JDK 25**, **Gradle Kotlin DSL**. 위치 `back/`.
- 의존성: DevTools, Lombok, Spring Data JPA, Validation, Spring Security, H2, Thymeleaf, thymeleaf-layout-dialect.
- 루트 패키지 `com.back`, 메인 `com.back.BackApplication`(`@EnableJpaAuditing`).
- 패키지 구조: `com.back.domain.*`, `com.back.global.*`.
- `BaseEntity`(`@MappedSuperclass`): id, createDate(`@CreatedDate`), modifyDate(`@LastModifiedDate`). 이후 모든 엔티티가 상속.
- `application.yml`(공통) + `application-dev.yml`(H2 파일 `./db_dev.mv.db`, `ddl-auto: update`, `/h2-console` on) + `application-test.yml`(H2 인메모리, `ddl-auto: create`).
- `spring.jpa.open-in-view: false`. 뷰 LAZY 예외 방지 규약(서비스에서 fetch join/`@EntityGraph`/DTO로 미리 로딩)을 팀 규칙으로 채택.
- 공통 레이아웃: thymeleaf-layout-dialect 기반 부모 레이아웃. `<head>`에 Tailwind 4.x Play CDN(`@tailwindcss/browser@4`), daisyUI 5 CDN, Pretendard 다이나믹 서브셋 웹폰트. 헤더에 네비 + (로그인 상태 placeholder).
- 홈 페이지: 레이아웃 상속한 간단한 환영 화면.
- 이 단계 Security: 모든 페이지 공개(이후 슬라이스에서 인가 강화). h2-console 접근 허용.

## Acceptance criteria

- [ ] `back/`에서 `./gradlew bootRun`으로 dev 프로필 앱이 기동된다.
- [ ] 홈 페이지(`/`)가 공통 레이아웃(Tailwind/daisyUI/Pretendard 적용)으로 렌더된다.
- [ ] dev에서 `/h2-console` 접속 가능, 파일 DB `db_dev.mv.db` 생성.
- [ ] test 프로필은 H2 인메모리 + `ddl-auto: create`로 동작.
- [ ] `BaseEntity` 상속 시 createDate/modifyDate가 JPA Auditing으로 자동 채워진다(간단한 검증 엔티티 또는 이후 슬라이스에서 확인).
- [ ] 홈 페이지 렌더 smoke 테스트(MockMvc, 200 응답)가 통과한다.

## Blocked by

None - can start immediately
