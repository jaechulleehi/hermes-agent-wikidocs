## 과거 유산과 현재 기준은 어떻게 나눌까

과거 유산은 무조건 지울 필요도, 현재 운영 문서에 계속 섞어둘 필요도 없다. 핵심은 archive/history에는 남기고, 현재 기준 문서에서는 [Hermes Agent](00-hermes-agent-core-concepts.md) 기준을 먼저 보이게 하는 것이다.

[OpenClaw에서 Hermes로](01-why-we-moved-from-openclaw-to-hermes.md) 넘어올 때도 이 문제가 있었다. 이름은 Hermes로 바뀌었지만 과거 파일, autostart 흔적, profile 이름, skill 설명, 문서 링크가 남아 있으면 사용자는 지금 기준이 무엇인지 헷갈린다.

## 왜 과거 흔적이 현재 규칙을 흔들까

운영 문서는 사람이 읽는 기준이면서 에이전트가 따르는 기준이기도 하다. 현재 문서에 과거 명칭과 예전 절차가 섞이면 하비, [방울이](23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md), 뽀동이도 어느 기준을 따라야 할지 흐려진다.

문제는 과거 유산 자체가 아니다. 과거 유산이 현재 기준처럼 보이는 순간이 문제다.

## 남길 것과 뺄 것

| 구분 | 남길 위치 | 이유 |
|---|---|---|
| 전환 과정 기록 | archive/history | 나중에 왜 바꿨는지 설명할 수 있음 |
| 실패 사례 | 회고/FAQ/복구 플레이북 | 같은 문제를 반복하지 않기 위해 필요 |
| 현재 실행 기준 | AGENTS.md, skill, repo 문서 | 에이전트가 따라야 하는 기준 |
| 오래된 명령 | archive 또는 migration 문서 | 현재 명령처럼 보이면 위험 |
| 토큰/웹훅/시크릿 | 어디에도 원문 보존하지 않음 | 보안 리스크 |

현재 기준 문서에는 지금 써야 할 이름, 경로, 명령, source of truth만 남겨야 한다.

## 실제 운영 예시

OpenClaw 흔적을 모두 지우면 전환사가 사라진다. 반대로 모든 흔적을 현재 문서에 남기면 Hermes 기준이 흐려진다. 그래서 우리는 이렇게 나눴다.

```text
과거 전환 이유와 시행착오 → 책의 1장/8장 사례
현재 운영 기준 → AGENTS.md, skill, GitHub repo
민감정보 → 원문 보존 금지, [REDACTED] 처리
```

이렇게 나누면 독자는 전환의 맥락을 이해하고, 에이전트는 현재 기준을 따른다.

## 운영 기준

과거 유산을 다룰 때는 아래 기준을 둔다.

- 현재 기준 문서에는 현재 이름과 경로를 먼저 쓴다.
- 과거 이름은 전환 설명이나 비교 문맥에서만 쓴다.
- archive/history와 active docs를 섞지 않는다.
- 민감정보는 원문으로 남기지 않는다.
- 전환에서 배운 기준은 skill이나 [체크리스트](21-how-to-make-hermes-operation-checklists-actually-useful.md)로 옮긴다.
- 현재 source of truth가 무엇인지 문서 상단에서 분명히 한다.

## FAQ

### 과거 문서를 다 지우면 안 되나요?

지우면 전환 과정에서 배운 이유와 실패 패턴이 사라진다. 다만 현재 운영 기준처럼 보이지 않게 위치를 나눠야 한다.

### 과거 이름을 본문에서 써도 되나요?

가능하다. OpenClaw에서 Hermes로 넘어온 이유처럼 맥락을 설명할 때는 필요하다. 다만 현재 실행 기준은 Hermes 기준으로 고정해야 한다.

## 다음 글

다음 글에서는 긴 대화가 쌓일 때 컨텍스트가 왜 흐려지고, Hermes가 이 문제를 어떻게 줄이는지 정리한다.

[다음 글: 긴 대화에서 컨텍스트는 왜 흐려질까](26-why-agents-get-fuzzy-in-long-conversations-and-how-hermes-holds-up.md)
