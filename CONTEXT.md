# Context

커뮤니티 게시판 도메인 용어집. 구현 세부사항이 아니라 "무엇을 무엇이라 부르는가"만 적는다.

## Glossary

### Member (회원)
서비스에 가입한 사용자. `username`(로그인 식별자), `password`, `nickname`(표시 이름)을 가진다.
역할은 `ROLE_USER` 또는 `ROLE_ADMIN`.

### Admin (관리자)
`ROLE_ADMIN`을 가진 Member. 모든 Post·Comment를 수정·삭제할 수 있다.

### Board (게시판)
Post들을 묶는 분류. 공지·자유·질문 세 개로 고정된다. Post는 정확히 하나의 Board에 속한다.
공지 Board에는 Admin만 글을 쓸 수 있다(읽기는 공개).

### Post (글)
Member가 특정 Board에 작성한 게시물. 제목·본문·작성자를 가진다. 조회수와 추천수를 가진다.

### Comment (댓글)
Member가 특정 Post에 단 글. 작성자를 가지며 추천수를 가진다.
Comment는 다른 Comment의 답글(대댓글)일 수 있다. 답글은 댓글의 한 단계 아래까지만 — 답글에 다시 답글은 달 수 없다(최대 깊이 2).

### Recommendation (추천)
Member가 Post 또는 Comment에 보내는 "좋아요". 회원당 대상별 1회로 제한되며 다시 누르면 취소된다(토글).
어떤 대상의 추천수는 그 대상에 대한 Recommendation의 개수다.

### View count (조회수)
Post가 열람된 횟수. 같은 HTTP 세션에서 같은 Post를 다시 열어도 1회만 집계된다.

## Boundaries

- 읽기(목록·상세)는 비로그인 공개.
- 쓰기·추천·수정·삭제는 로그인 필요.
- Post·Comment의 수정·삭제는 작성자 본인 또는 Admin만 가능.
- 공지 Board의 글쓰기는 Admin만 가능.
- 작성자 본인은 자기 Post/Comment에 Recommendation을 보낼 수 없다(자기추천 차단).
- 답글이 달린 Comment를 지우면 본문만 "삭제된 댓글입니다"로 남고 답글은 유지된다(soft delete). 답글이 없으면 흔적 없이 삭제된다.
- Post를 지우면 그에 속한 Comment와 Recommendation도 함께 사라진다.
