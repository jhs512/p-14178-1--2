# Domain docs

이 repo의 도메인 문서 레이아웃.

<!-- 레이아웃: single-context (repo root 의 CONTEXT.md + docs/adr/) -->

## 레이아웃

이 repo는 **단일 컨텍스트(single-context)** 레이아웃을 쓴다:

```
CONTEXT.md          # repo 전체 도메인 언어
docs/adr/           # 아키텍처 결정 기록 (ADR)
```

이 문서들은 도메인 언어를 쓴다. 코드를 읽을 때 임의 작명보다 여기 정의된 용어를 우선한다.

## 아키텍처 결정

과거 아키텍처 결정은 다음 위치에 ADR로 기록된다:

```
docs/adr/
```

각 ADR은 결정 하나를 담는다: 맥락(context), 결정 자체, 결과(consequences). 과거 결정을 뒤집거나 반박하기 전에 먼저 참고할 것.

## 소비자 규칙(Consumer rules)

- `improve-codebase-architecture` 는 도메인 언어용 `CONTEXT.md` 와 과거 결정용 `docs/adr/` 를 읽는다.
- `diagnose` 는 버그 기술 시 올바른 도메인 용어를 쓰기 위해 `CONTEXT.md` 를 읽는다.
- `tdd` 는 테스트/단언(assertion)을 도메인 언어로 명명하기 위해 `CONTEXT.md` 를 읽는다.

결정이 도메인 언어를 바꾸면 `CONTEXT.md` 를 갱신한다. 결정이 아키텍처성이면 ADR을 추가한다.
