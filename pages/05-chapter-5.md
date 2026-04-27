## 5장. 외부 도구/MCP/자동화 운영

[Hermes Agent](00-hermes-agent-core-concepts.md)가 AI 개인비서에서 업무 자동화 시스템으로 넘어가는 순간은 외부 도구를 붙일 때다. Google Workspace, Slack, WikiDocs, MCP, cron, gateway, skill이 연결되면 하비는 단순히 답변하는 창이 아니라 실제 업무 흐름을 움직이는 운영 창구가 된다.

하지만 도구가 많아질수록 문제도 늘어난다. 인증은 어디서 관리하는지, 상시 실행이 필요한지, skill로 남길지, cron으로 돌릴지, [gateway](12-always-on-gateway-is-more-confusing-than-it-looks.md)가 실제로 살아 있는지부터 나눠야 한다. 5장은 “붙일 수 있다”가 아니라 “운영 가능한 방식으로 붙인다”에 초점을 둔다.

## 이 장에서 다루는 문제

| 순서 | 글 | 핵심 질문 |
|---|---|---|
| 05-1 | Google Workspace와 MCP 연동은 왜 오래 걸릴까 | 외부 도구 연동에서 기능보다 먼저 봐야 할 운영 조건은 무엇인가 |
| 05-2 | Hermes Agent 스킬은 언제 만들고 어떻게 관리할까 | 반복 절차는 언제 skill로 남기고 언제 그냥 실행할까 |
| 05-3 | always-on gateway는 왜 자주 헷갈릴까 | 상시 실행 구조에서 실제로 살아 있는 프로세스를 어떻게 확인할까 |
| 05-4 | Daily Briefing Bot은 어떤 업무 자동화 패턴일까 | cron, fresh session, 요약, 전달을 어떻게 하나의 패턴으로 볼까 |

## 처음에 생기는 착각

외부 도구 자동화는 “API가 있으면 된다”로 끝나지 않는다. 실제로는 인증, 권한, 실행 위치, 실패 시 알림, 민감정보 처리, 반복 검증이 더 큰 문제다.

MCP도 마찬가지다. MCP 서버가 붙었다고 해서 바로 업무 자동화가 안정되는 것은 아니다. 어떤 도구가 어떤 [profile](03-why-same-harvey-feels-like-different-memory.md)에서 호출되는지, 어떤 정보가 memory에 남아야 하는지, 어떤 작업이 cron으로 돌아도 되는지 구분해야 한다.

## 이 장에서 얻을 기준

- 외부 도구는 연결 가능성보다 운영 리스크를 먼저 본다.
- 반복 절차는 skill로 남기고, 시간 기반 반복 작업은 cron으로 돌린다.
- 상시 응답이 필요하면 gateway 상태와 실제 프로세스를 함께 본다.
- fresh session에서 도는 자동화는 prompt가 스스로 완결되어야 한다.
- 민감정보, 토큰, 웹훅 URL은 문서와 요약에 원문으로 남기지 않는다.
- Slack, [WikiDocs](07-why-we-write-the-wiki-first.md), Google Workspace 같은 도구는 기능이 아니라 workflow connector로 본다.

## 다음 장으로 가기 전 체크 질문

지금 붙이려는 것은 한 번 실행할 도구인가, 반복할 절차인가, 정해진 시간에 돌아야 할 자동화인가, 상시 대기해야 할 gateway인가?

이 구분이 잡히면 다음 장의 WikiDocs, 블로그, 강의 콘텐츠 시스템도 훨씬 안정적으로 이어진다.

## 이어서 읽기

- 외부 도구 연동의 첫 기준은 [Google Workspace와 MCP 연동은 왜 오래 걸릴까](05-why-google-workspace-integration-takes-longer-than-expected.md)에서 확인한다.
- 반복 절차와 자동화의 경계는 [Hermes Agent 스킬은 언제 만들고 어떻게 관리할까](10-when-and-how-to-manage-skills-in-hermes.md), [Daily Briefing Bot은 어떤 업무 자동화 패턴일까](05-daily-briefing-bot-workflow.md)를 이어서 보면 좋다.
- 실제 자동화가 콘텐츠 발행으로 이어지는 흐름은 [크론 조사에서 WikiDocs 발행까지 이어지는 AI 업무 자동화 케이스](30-cron-research-to-agent-content-workflow.md)에서 확인할 수 있다.
