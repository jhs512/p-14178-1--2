---
title: 추천(글/댓글) 토글
state: ready-for-agent
labels: [ready-for-agent]
---

## Parent

`.scratch/community-board/PRD.md`

## What to build

Member가 Post와 Comment에 추천(좋아요)을 토글하는 슬라이스.

- `PostRecommendation`(post+member unique), `CommentRecommendation`(comment+member unique) 엔티티(BaseEntity 상속). 각각 member(N:1) 포함.
- `RecommendationService`: `togglePost(member, post)`, `toggleComment(member, comment)`. 이미 추천했으면 삭제(취소), 아니면 생성(토글). unique 제약으로 중복 방지.
- 자기추천 차단: 작성자 본인은 자기 Post/Comment에 추천 불가.
- 추천수 = 해당 대상의 Recommendation count. 글 상세/목록·댓글에 추천수와 추천 버튼(내 추천 상태 반영) 표시.
- 추천은 인증 필요.
- BaseInitData 확장: 추천 일부 시드(추천수가 0이 아니게).

## Acceptance criteria

- [ ] 로그인 Member가 글/댓글 추천 버튼을 눌러 추천하고, 다시 눌러 취소할 수 있다(토글).
- [ ] 같은 Member가 같은 대상에 추천을 중복 집계할 수 없다.
- [ ] 작성자 본인은 자기 글/댓글에 추천할 수 없다.
- [ ] 글/댓글에 정확한 추천수가 표시된다.
- [ ] RecommendationService 테스트: 토글(생성→취소), 중복 차단, 자기추천 차단, 추천수 count 정확.

## Blocked by

- `.scratch/community-board/issues/06-comments.md`
