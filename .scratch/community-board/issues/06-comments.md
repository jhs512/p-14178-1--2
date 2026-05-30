---
title: 댓글·대댓글 CRUD + soft delete
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

글에 댓글과 한 단계 답글(대댓글)을 달고, 수정·삭제하는 슬라이스.

- `Comment` 엔티티(BaseEntity 상속): post(N:1), author=Member(N:1), parent=Comment(self N:1, nullable), content, deleted(boolean, soft delete 표식).
- 깊이 제약: parent가 있는 Comment(답글)에는 다시 답글을 달 수 없다(최대 깊이 2). 위반 시 거부.
- `CommentService`: 댓글/답글 작성, 수정, 삭제, 권한 검사(작성자 본인 또는 ROLE_ADMIN).
- 삭제 규칙: 답글이 달린 댓글을 지우면 deleted=true로 표시(본문은 "삭제된 댓글입니다"로 렌더, 답글 유지). 답글이 없으면 실제 삭제.
- 글 상세 페이지: 댓글/대댓글 트리 렌더, 댓글·답글 작성 폼, 본인/Admin일 때 수정·삭제.
- BaseInitData 확장: 댓글 5개 시드(일부는 대댓글).

## Acceptance criteria

- [ ] 로그인 Member가 글에 댓글을, 댓글에 답글을 달 수 있다.
- [ ] 답글에 다시 답글을 다는 시도는 거부된다(최대 깊이 2).
- [ ] 답글이 달린 댓글 삭제 시 본문이 "삭제된 댓글입니다"로 남고 답글은 유지된다.
- [ ] 답글이 없는 댓글 삭제 시 흔적 없이 사라진다.
- [ ] 작성자 본인 또는 Admin만 댓글을 수정·삭제할 수 있다.
- [ ] 첫 기동 시 댓글 5개 시드(대댓글 포함)가 생성된다.
- [ ] CommentService 테스트: 깊이 2 초과 거부, 답글 있는 댓글 soft delete + 답글 보존, 답글 없는 댓글 hard delete, 권한.

## Blocked by

- `.scratch/community-board/issues/04-board-post-read.md`
