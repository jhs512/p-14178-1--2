---
title: Board 시드 + 글 목록/상세 (읽기 공개)
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

비로그인 방문자도 게시판과 글을 읽을 수 있는 슬라이스.

- `Board` 엔티티: name. BaseInitData에서 공지·자유·질문 3개 고정 시드(CRUD UI 없음).
- `Post` 엔티티(BaseEntity 상속): board(N:1), author=Member(N:1), title, content, viewCount(int). 추천수는 이후 슬라이스에서 Recommendation count로 산출(지금은 0/미표시 가능).
- `PostService`: Board별 목록 조회(Spring Data `Pageable`, 최신순, 페이지당 N개), 제목 LIKE 검색, 상세 조회, 조회수 증가(`increaseViewCount`).
- 조회수 세션 dedup: 컨트롤러에서 HttpSession에 본 Post id 집합을 저장, 처음 보는 경우에만 increaseViewCount 호출. (서비스는 "올려라"만 책임 → 단위 테스트 용이.)
- 페이지: 게시판 목록(3개 Board), Board별 글 목록(페이지네이션 + 제목 검색창), 글 상세(제목·본문·작성자·작성일시·조회수).
- OSIV off이므로 목록/상세에서 author 등 연관은 서비스에서 미리 로딩(fetch join/`@EntityGraph` 또는 DTO).
- BaseInitData 확장: 글 5개 시드(여러 Board에 분산, 공지 글은 Admin 작성).
- 읽기는 모두 공개(비로그인 허용).

## Acceptance criteria

- [ ] 첫 기동 시 Board 공지·자유·질문 3개와 글 5개 시드가 생성된다(회원 시드 이후, 회원 있으면 중단 규칙 유지).
- [ ] 비로그인 상태로 게시판 목록·Board별 글 목록·글 상세를 볼 수 있다.
- [ ] 글 목록이 최신순 페이지네이션되고, 제목 검색이 동작한다.
- [ ] 같은 세션에서 같은 글을 다시 열면 조회수가 추가로 오르지 않는다.
- [ ] 뷰 렌더 중 LazyInitializationException이 발생하지 않는다.
- [ ] PostService 테스트: 제목 검색, 페이지네이션, increaseViewCount 동작.

## Blocked by

- `.scratch/community-board/issues/02-auth-signup-login.md`
