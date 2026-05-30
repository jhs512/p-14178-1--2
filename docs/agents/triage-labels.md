# Triage labels

이 repo의 트리아지 라벨.

<!-- 기본 매핑: 각 역할의 라벨 = 표준 역할명 그대로. 다른 문자열을 쓰면 Label 열을 수정. -->

5개 표준 트리아지 역할은 이 repo에서 다음 라벨로 매핑된다:

| Role | Label | 의미 |
| --- | --- | --- |
| Needs evaluation | `needs-triage` | 관리자가 이 이슈를 평가해야 함 |
| Waiting on reporter | `needs-info` | 신고자의 추가 정보 대기로 블록됨 |
| AFK-ready | `ready-for-agent` | 완전 명세됨. 에이전트가 사람 컨텍스트 없이 픽업 가능 |
| Needs human | `ready-for-human` | 사람 구현 필요 |
| Won't fix | `wontfix` | 처리하지 않음 |

## 사용법

`triage` 스킬이 이슈를 처리할 때 해당 역할의 **Label** 열 라벨을 붙인다. 여기서 라벨 문자열을 바꾸면 이슈 트래커가 쓰는 모든 곳도 함께 갱신할 것.
