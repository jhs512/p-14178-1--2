---
title: 글 작성/수정/삭제 + 권한 + 공지 Admin 제약
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

로그인한 Member가 글을 작성·수정·삭제하는 슬라이스. 권한 규칙 포함.

- `PostService` CRUD: 작성(board 선택), 수정, 삭제. 삭제 시 해당 Post의 Comment·Recommendation도 함께 제거(연관 정리).
- 권한: 수정·삭제는 작성자 본인 또는 ROLE_ADMIN만. 위반 시 거부.
- 공지 Board 글쓰기는 ROLE_ADMIN만(일반 Member 거부). 자유·질문은 로그인 Member면 가능.
- 페이지: 글 작성 폼(board 선택, 제목/본문), 글 수정 폼. 상세 화면에 본인/Admin일 때만 수정·삭제 버튼 노출.
- 쓰기/수정/삭제는 인증 필요.

## Acceptance criteria

- [ ] 로그인 Member가 자유·질문 Board에 글을 쓰고, 자기 글을 수정·삭제할 수 있다.
- [ ] 작성자가 아니고 Admin도 아니면 수정·삭제가 거부된다.
- [ ] Admin은 임의의 글을 수정·삭제할 수 있다.
- [ ] 공지 Board 글쓰기는 Admin만 가능하고 일반 Member는 거부된다.
- [ ] 글 삭제 시 딸린 댓글·추천이 함께 사라진다.
- [ ] PostService 테스트: 비작성자 수정·삭제 거부, Admin 허용, 공지 Board 일반 Member 글쓰기 거부.

## Blocked by

- `.scratch/community-board/issues/04-board-post-read.md`
