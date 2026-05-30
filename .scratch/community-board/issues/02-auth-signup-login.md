---
title: 회원가입 + 로그인/로그아웃
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

방문자가 회원가입하고, Member로 로그인/로그아웃하는 인증 수직 슬라이스.

- `Member` 엔티티(BaseEntity 상속): username(unique, 로그인 식별자), password(BCrypt 인코딩 저장), nickname(unique), role(`ROLE_USER`|`ROLE_ADMIN`).
- `MemberService.join(username, password, nickname)`: username/nickname 중복 검증, 비밀번호 인코딩, 기본 role `ROLE_USER`.
- Spring Security 폼 로그인: `UserDetailsService`(username 기반), `BCryptPasswordEncoder`, 로그인/로그아웃 처리.
- 페이지: 회원가입 폼(username/password/nickname, 검증 메시지), 로그인 폼. 공통 레이아웃 헤더에 로그인 상태(닉네임/로그아웃 vs 로그인/회원가입) 반영.
- BaseInitData 골격 시작: 회원이 1명이라도 있으면 즉시 중단. 회원 5명 시드(시드1 = Admin, nickname "관리자", `ROLE_ADMIN`; 2~5 `ROLE_USER`). `@Transactional` ApplicationRunner 빈 `baseInitDataApplicationRunner`.

## Acceptance criteria

- [ ] 회원가입 폼에서 가입하면 Member가 생성되고 password가 BCrypt로 인코딩되어 저장된다.
- [ ] 이미 존재하는 username 또는 nickname으로 가입하면 거부되고 폼에 오류가 표시된다.
- [ ] 가입한 계정으로 로그인/로그아웃이 동작하고, 헤더가 로그인 상태에 따라 바뀐다.
- [ ] 첫 기동 시 회원이 없으면 시드 회원 5명(Admin 1 + USER 4)이 생성되고, 이미 회원이 있으면 시드가 생성되지 않는다.
- [ ] MemberService 테스트: 중복 username/nickname 거부, 비밀번호 인코딩 저장 검증(H2 인메모리, test 프로필).

## Blocked by

- `.scratch/community-board/issues/01-bootstrap-layout-home.md`
