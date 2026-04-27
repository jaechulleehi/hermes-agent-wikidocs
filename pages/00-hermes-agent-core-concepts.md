## Hermes Agent 주요 개념과 기능은 어떻게 연결될까

에르메스 에이전트(Hermes Agent)를 처음 볼 때 가장 헷갈리는 지점은 기능 이름이 많다는 것이다. AI 개인비서, 역할형 에이전트, memory, session, profile, skill, MCP, cron, gateway, WikiDocs, GitHub 같은 말이 한꺼번에 나온다.

이 단어들은 따로 외우는 용어가 아니다. 하나의 운영 흐름 안에서 서로 역할이 다르다. Hermes Agent를 업무 자동화 시스템으로 쓰려면 “무슨 기능이 있는가”보다 “어떤 개념이 어떤 문제를 해결하는가”를 먼저 잡아야 한다.

이 페이지는 책 전체에서 반복해서 나오는 주요 개념과 기능을 한 번에 연결하는 안내판이다.

## 한눈에 보는 개념 지도

| 개념/기능 | 책에서의 의미 | 함께 읽을 글 |
|---|---|---|
| AI 개인비서 | 사용자의 요청을 받아 해석하고 흐름을 잡는 메인 창구 | [AI 챗봇과 AI 개인비서는 어떻게 다를까](https://wikidocs.net/345923), [AI 개인비서 메인 창구는 왜 필요할까](https://wikidocs.net/345892) |
| 역할형 에이전트 | 조사/정리/실행/이미지 제작처럼 실패 패턴이 다른 일을 나누는 방식 | [역할형 에이전트는 어떤 기준으로 나눌까](https://wikidocs.net/345925) |
| 조사형 에이전트 | 근거 수집, 비교, 확장조사에 강한 역할 | [조사형 에이전트는 어디서 강하고 어디서 흔들릴까](https://wikidocs.net/345895) |
| 정리형 에이전트 | 문서 구조, 흐름, 가독성, 발행 품질을 책임지는 역할 | [정리형 에이전트는 어디서 강하고 어디서 흔들릴까](https://wikidocs.net/345896) |
| 실행형 에이전트 | 파일 수정, 명령 실행, 검증처럼 실제 상태를 바꾸는 역할 | [역할형 에이전트는 어떤 기준으로 나눌까](https://wikidocs.net/345925), [Hermes Agent 운영 체크리스트는 어떻게 써야 할까](https://wikidocs.net/345919) |
| 이미지형 에이전트 | 썸네일, 본문 이미지, 설명용 시각 자료를 만드는 역할 | [하망이와 WikiDocs 본문 이미지를 만드는 법](https://wikidocs.net/345989) |
| session | 현재 대화 안에서 유지되는 흐름 | [같은 AI 개인비서인데 기억은 왜 다르게 느껴질까](https://wikidocs.net/345899) |
| memory | 세션을 넘어 오래 유지할 사용자/운영 정보 | [긴 대화에서 컨텍스트는 왜 흐려질까](https://wikidocs.net/345901) |
| profile | 에이전트의 identity, tool, memory, state 경계를 나누는 실행 단위 | [같은 AI 개인비서인데 기억은 왜 다르게 느껴질까](https://wikidocs.net/345899) |
| source of truth | 최종 기준이 되는 문서/저장소/운영 위치 | [과거 유산과 현재 기준은 어떻게 나눌까](https://wikidocs.net/345915) |
| skill | 반복 절차를 재사용 가능한 작업 매뉴얼로 남기는 기능 | [Hermes Agent 스킬은 언제 만들고 어떻게 관리할까](https://wikidocs.net/345904) |
| MCP | 외부 도구와 Hermes Agent를 연결하는 업무 연결 계층 | [Google Workspace와 MCP 연동은 왜 오래 걸릴까](https://wikidocs.net/345903) |
| cron | 정해진 시간이나 주기로 fresh session 작업을 실행하는 자동화 방식 | [Daily Briefing Bot은 어떤 업무 자동화 패턴일까](https://wikidocs.net/345926) |
| gateway | 메시징/자동화 요청을 상시 받아들이기 위한 실행 구조 | [always-on gateway는 왜 자주 헷갈릴까](https://wikidocs.net/345906) |
| WikiDocs | 공개 책/전자책으로 읽히는 배포 채널 | [왜 WikiDocs를 먼저 쓰고 블로그/강의를 나중에 뽑을까](https://wikidocs.net/345908) |
| GitHub | WikiDocs 책의 source of truth와 변경 이력 관리 위치 | [GitHub와 WikiDocs로 콘텐츠를 발행하고 고치는 흐름](https://wikidocs.net/345994) |
| 체크리스트 | 시행착오를 다음 실행 순서로 바꾸는 문서 | [시행착오를 운영 체크리스트로 바꾸는 법](https://wikidocs.net/345917), [Hermes Agent 운영 체크리스트는 어떻게 써야 할까](https://wikidocs.net/345919) |
| 복구 플레이북 | 문제가 생겼을 때 확인 순서와 복구 순서를 정리한 문서 | [복구 플레이북은 왜 문서보다 순서가 중요할까](https://wikidocs.net/345918) |
| OpenClaw migration | OpenClaw에서 Hermes로 넘어오며 운영 기준을 다시 세운 전환 경험 | [OpenClaw에서 Hermes로 왜 넘어왔나](https://wikidocs.net/345889), [OpenClaw에서 Hermes로 넘어올 때 무엇을 점검해야 할까](https://wikidocs.net/345920) |

## 기능보다 흐름이 먼저다

Hermes Agent의 기능을 하나씩 보면 각각은 단순해 보인다. skill은 반복 절차를 남기고, cron은 예약 작업을 실행하고, MCP는 외부 도구를 연결하고, gateway는 상시 요청을 받는다.

하지만 실제 업무에서는 이 기능들이 따로 움직이지 않는다. 예를 들어 Daily Briefing Bot 하나만 봐도 cron이 fresh session을 열고, 필요한 정보를 조사하고, 요약한 뒤, 메시징 채널로 전달한다. 이 흐름이 반복되면 skill이나 체크리스트로 남길 기준이 생기고, 결과물이 콘텐츠가 되면 WikiDocs와 GitHub로 이어진다.

그래서 이 책에서는 기능을 “설치 방법” 중심으로 설명하지 않는다. 기능이 어떤 운영 문제를 해결하고, 어떤 실패를 만들 수 있으며, 어떤 기준으로 써야 하는지를 중심으로 설명한다.

## 내부 이름은 운영 예시다

하비/방울이/뽀동이/봉구/비벙이/하망이는 책 전체에서 자주 나오는 내부 이름이다. 하지만 독자는 먼저 공개 개념을 이해하면 된다.

- 하비는 AI 개인비서 메인 창구의 예시다.
- 방울이는 조사형 에이전트의 예시다.
- 뽀동이는 정리형 에이전트의 예시다.
- 봉구는 실행형 에이전트의 예시다.
- 하망이는 이미지형 에이전트의 예시다.

이 이름들은 세계관을 설명하기 위한 장식이 아니라, 실제 운영에서 요청을 빠르게 나누기 위한 호출명이다. “방울이 시켜서 확장조사”, “뽀동이가 WikiDocs 구조 정리”, “하망이가 이미지 제작 방향 정리”처럼 역할과 책임을 짧게 부르기 위한 방식이다.

## 이 페이지를 어떻게 쓰면 좋을까

처음 읽는 독자는 이 페이지를 책의 개념 색인처럼 쓰면 된다. 본문에서 낯선 개념이 나오면 이 페이지로 돌아와 해당 개념이 어느 장과 연결되는지 확인한다.

이미 Hermes Agent를 쓰고 있는 독자는 문제를 분류하는 기준으로 쓰면 된다.

```text
기억이 이상하다 → session/memory/profile/source of truth 문제인가?
외부 도구가 안 된다 → MCP/gateway/권한/실행 환경 문제인가?
반복 업무가 많다 → skill/cron/checklist 중 무엇으로 남길까?
콘텐츠가 흩어진다 → WikiDocs/GitHub/source of truth를 어디에 둘까?
조직 도입이 흔들린다 → 역할/권한/검증/복구 기준이 있는가?
```

## 다음 글

개념을 한 번 잡았다면 [Hermes Agent는 왜 운영 시스템인가](https://wikidocs.net/345890)에서 이 기능들이 왜 하나의 운영 시스템으로 묶이는지 확인한다. 그다음 [OpenClaw에서 Hermes로 왜 넘어왔나](https://wikidocs.net/345889)를 읽으면 이 구조가 실제 전환 과정에서 어떻게 생겼는지 볼 수 있다.
