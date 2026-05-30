---
title: 내 정보 조회·수정
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

로그인한 Member가 자기 정보를 보고 nickname·password를 수정하는 슬라이스.

- '내 정보' 페이지: username(불변, 표시만), nickname, 가입일시 등 조회.
- `MemberService.modify(member, nickname, password)`: nickname/password만 변경. nickname 변경 시 중복 검증. password는 입력 시에만 재인코딩(빈 값이면 유지). username은 절대 바뀌지 않음.
- 인증 필요(비로그인은 로그인 페이지로).

## Acceptance criteria

- [ ] 로그인 상태에서 '내 정보' 페이지에 현재 정보가 표시된다.
- [ ] nickname/password 수정이 반영되고, username은 변경 불가하다.
- [ ] 다른 회원이 쓰는 nickname으로 변경하면 거부된다.
- [ ] 비로그인 접근은 로그인 페이지로 리다이렉트된다.
- [ ] MemberService.modify 테스트: nickname/password만 변경·username 불변·nickname 중복 거부.

## Blocked by

- `.scratch/community-board/issues/02-auth-signup-login.md`
