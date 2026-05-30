# Issue tracker

이 repo의 이슈는 `.scratch/` 하위 **로컬 마크다운 파일**로 추적한다.

## 이슈 위치

기능(feature) 또는 작업 단위마다 디렉터리 하나:

```
.scratch/<feature-slug>/
```

그 안에 번호를 붙인 마크다운 파일로 이슈를 둔다:

```
.scratch/<feature-slug>/001-<issue-slug>.md
.scratch/<feature-slug>/002-<issue-slug>.md
```

## 이슈 파일 형식

각 이슈 파일은 frontmatter + 본문으로 시작한다:

```markdown
---
title: <한 줄 제목>
state: needs-triage      # docs/agents/triage-labels.md 의 라벨 중 하나
labels: []
---

## Context

<이 이슈가 존재하는 이유>

## Acceptance criteria

- [ ] ...
```

## 작업(Operations)

- **생성**: 기능 디렉터리에 번호 붙인 새 파일 작성
- **목록**: `.scratch/<feature-slug>/` 하위 파일 읽기
- **상태 변경**: frontmatter 의 `state:` 필드 편집
- **종료**: `state:` 를 종료 라벨(예: `wontfix` 또는 `done`)로 설정

"이슈 생성" 요청 시 사용자가 다르게 지정하지 않는 한 이 레이아웃을 기본으로 쓴다.
