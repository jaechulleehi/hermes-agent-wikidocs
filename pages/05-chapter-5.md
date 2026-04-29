## 5장. 외부 도구/MCP/채널 연동과 AI 워크플로우 자동화

Hermes Agent의 힘은 답변보다 연결에서 커진다. Google Workspace, Notion, GitHub, WikiDocs, MCP, CLI, API, cron, gateway, Daily Briefing Bot은 각각 다른 이름의 기능이지만, 목적은 하나다. 사람이 매번 반복하던 업무 흐름을 AI 개인비서가 안전하게 이어받게 만드는 것이다.

다만 외부 도구는 많이 붙일수록 좋은 것이 아니다. 계정, 권한 범위, 실행 위치, gateway 상태, cron prompt, 결과 전달 위치가 함께 맞아야 실제 운영이 된다. 5장은 도구 목록을 외우는 장이 아니라, **어떤 업무에 어떤 연결 방식을 붙일지** 고르는 장이다. 반복 절차를 재사용하는 Skill은 6장에서 따로 다룬다.

![5장 외부 도구 MCP 자동화 운영 구조](../assets/images/chapter-heroes/ch05-tools-mcp-automation-ops-codex.webp)

## 5장을 읽는 순서

외부 연결은 “MCP부터 붙일까?”로 시작하면 복잡해진다. 먼저 내가 하려는 일이 검색인지, 실행인지, 반복 자동화인지, 메시징 운영인지 구분해야 한다.

| 먼저 답할 질문 | 주로 보는 기능 | 연결되는 글 |
|---|---|---|
| 로컬에서 바로 확인할 일인가 | CLI/terminal | 네 가지 실행 방식 |
| 대화 흐름 안에서 외부 서비스를 부를 일인가 | MCP/tool | Google Workspace와 MCP, MCP 외부 도구 |
| 정해진 시간에 혼자 실행될 일인가 | cron | Daily Briefing Bot |
| Slack/Telegram/Discord에서 계속 받을 일인가 | gateway | always-on gateway |
| 웹에서 새 정보를 찾을 일인가 | web search/browser/API | 웹 검색/리서치 도구 |
| 반복 절차로 남길 일인가 | Skill | 6장 Skill 운영 |

이 순서를 잡으면 “도구를 많이 붙이는 문제”가 아니라 “업무 흐름에 맞는 연결 방식을 고르는 문제”로 바뀐다.

## 외부 연결을 고를 때의 기준

공식 Hermes Agent 문서는 MCP, gateway, tools, cron, terminal backend 같은 기능의 기준점이다. 이 책에서는 최신 설정값을 그대로 반복하기보다, 실제 운영에서 어떤 질문을 해야 하는지에 집중한다.

| 기능 | 하는 일 | 운영 질문 |
|---|---|---|
| MCP | 외부 서비스와 도구를 대화 흐름에 연결한다 | 어떤 계정/권한/scope로 연결했고 위험 작업은 어디서 멈출 것인가 |
| CLI | 기존 개발자 워크플로우와 로컬 명령을 호출한다 | 어느 HOME/path/auth 환경에서 실행되는가 |
| API | REST/GraphQL endpoint를 직접 호출한다 | token scope, pagination, rate limit, retry를 관리하는가 |
| cron | 정해진 시간에 fresh session으로 작업을 시작한다 | prompt가 혼자 실행될 만큼 충분하고 결과가 어디로 전달되는가 |
| gateway | 메시징 플랫폼, cron 실행, delivery를 이어준다 | process 상태와 실제 메시지 도착을 따로 확인했는가 |
| web search/browser | 새 정보와 원문을 찾는다 | 검색 결과를 원문 출처로 다시 검증했는가 |
| toolset | 역할별로 사용할 도구 범위를 제한한다 | 필요한 도구만 열었고 위험 도구는 분리했는가 |
| terminal backend | local/docker/ssh/sandbox 등 실행 위치를 정한다 | credential 노출 범위와 rollback 가능성을 확인했는가 |

여기서 중요한 기준은 “연결 성공”이 아니라 “업무 결과가 검증되었는가”다. exit code 0, gateway status OK, API 200 응답만으로는 충분하지 않다. 파일 diff, 공개 반영, 메시지 delivery, 로그, rollback 경로까지 확인해야 운영이라고 부를 수 있다.

## 실제 운영에서는 이렇게 이어진다

WikiDocs 작업을 예로 들면 GitHub가 원고의 원본 기준이고, WikiDocs는 공개 배포 채널이다. 그래서 원고를 고친 뒤에는 파일이 바뀌었는지만 보지 않고, 링크, 이미지 경로, 공개 화면 반영까지 확인해야 한다. GitHub CLI는 저장소 작업에 강하고, WikiDocs 도구는 공개 채널 반영에 필요하며, 브라우저 확인은 최종 화면 검수에 가깝다.

콘텐츠 자동화도 같은 구조다. 방울이의 크론 조사는 신호를 찾고, 좋은 신호만 shared-memory와 Obsidian으로 이어진다. 사용자가 “넘겨”라고 판단하면 뽀동이가 본문 구조를 만들고, 하비가 최종 통합하며, 필요하면 하망이가 이미지를 만든다. 여기서 cron은 시작 장치, shared-memory는 handoff 위치, WikiDocs는 공유 자산이다. 단일 기능이 아니라 역할형 에이전트와 도구가 연결된 AI 워크플로우 자동화로 봐야 한다.

## 웹 검색/리서치 도구는 어떻게 고를까

웹 검색은 실행 도구가 아니라 입력 장치다. 검색 결과는 정답이 아니라 후보 출처다. 그래서 검색 품질은 API 이름보다 검색원, 최신성, 중복 제거, 원문 접근, 인용 가능성, 후속 검증 기준으로 봐야 한다.

공식 Integrations 기준의 web backend는 Firecrawl, Parallel, Tavily, Exa처럼 공식 문서에서 지원 범위를 확인해야 하는 영역이다. DuckDuckGo/DDGS, SearXNG, Brave Search API, Perplexity, Google Suggest 같은 도구는 별도 리서치 경로나 운영 후보로 볼 수 있다. 둘을 섞어 “공식 Hermes web backend”처럼 쓰면 설정과 검증 기준이 꼬인다.

| 방식 | 비용/인증 | 강한 경우 | 조심할 점 |
|---|---|---|---|
| DuckDuckGo/DDGS | 무료, 보통 API key 없음 | 빠른 초기 조사, 키워드 후보, 가벼운 사실 확인 | 결과 재현성과 대량 호출 안정성이 약할 수 있다 |
| SearXNG | 자체 호스팅이면 무료에 가깝다 | 여러 검색원을 메타검색으로 묶고 싶을 때 | 공개 인스턴스 안정성, 차단, 운영 부담 |
| Firecrawl/Parallel/Tavily/Exa | 공식 문서에서 지원 범위 확인 | Hermes Agent web backend로 붙일 때 | 비용, quota, 설정값 변경, token 관리 |
| Brave/Perplexity 등 별도 검색 API | 유료 또는 API key 필요 | 별도 리서치 경로나 질문형 리서치가 필요할 때 | 공식 web backend와 구분하고 원문 근거를 재검증 |
| Browser 직접 확인 | 별도 API 없이 화면 확인 | 로그인 페이지, 동적 페이지, 원문 검수 | 느리고 깨지기 쉬워 자동화의 마지막 단계로 둔다 |

실제 사용 기준은 단순하다. 빠르게 신호를 찾는다면 무료 검색과 원문 확인으로 충분한 경우가 많다. 매일 같은 주제를 안정적으로 모니터링해야 한다면 유료 API나 자체 SearXNG 같은 운영형 백엔드를 검토한다. 공유 글에 근거로 넣을 정보라면 검색 결과 요약보다 원문 URL과 공식 출처 확인이 우선이다.

## 초보자용 연결 방식 선택표

| 하고 싶은 일 | 먼저 고를 방식 | 이유 |
|---|---|---|
| 설치 확인/간단한 로컬 작업 | CLI | 실패 원인이 가장 잘 보인다 |
| Slack에서 사람 요청을 받기 | gateway | 메시징 연결과 delivery가 핵심이다 |
| 매일 같은 시간에 조사/보고 | cron | fresh session prompt와 전달 대상이 핵심이다 |
| 웹에서 새 정보 찾기 | DuckDuckGo/DDGS 또는 SearXNG 계열 검색 | 무료로 시작하고 원문 검증을 붙이기 쉽다 |
| Hermes 공식 web backend 활용 | Firecrawl/Parallel/Tavily/Exa | 공식 Integrations 기준으로 설정과 지원 범위를 확인할 수 있다 |
| 별도 검색 API/AI 검색 활용 | Brave/Perplexity 등 | 특정 검색 품질이나 요약형 리서치가 필요할 때 후보가 된다 |
| 반복 절차를 다음에도 쓰기 | Skill | 도구가 아니라 검증 순서와 판단 기준을 재사용한다 |
| 위험 명령/배포/삭제 실행 | terminal backend + 승인 기준 | 실행 위치, credential, rollback 가능성을 분리해야 한다 |

이 표의 목적은 하나의 정답을 고르는 것이 아니다. 같은 작업도 CLI로 즉시 실행할지, MCP tool로 대화 흐름에 붙일지, API로 정기 조회할지, cron으로 자동화할지, 반복 절차를 Skill로 남길지에 따라 필요한 맥락과 검증이 달라진다.

## 5장을 마치면 남아야 할 것

| 남아야 할 것 | 확인 질문 | 연결되는 글 |
|---|---|---|
| 연결 방식 선택 기준 | 이 일은 CLI/API/MCP/gateway/cron/Skill 중 무엇이 맞는가 | [네 가지 실행 방식](https://wikidocs.net/346124) |
| 권한/인증 경계 | 계정 권한, token, scope, 로그 노출 범위를 분리했는가 | [Google Workspace와 MCP](https://wikidocs.net/345903) / [MCP 외부 도구](https://wikidocs.net/346231) |
| 상시 운영 점검표 | gateway, delivery, cron, channel을 따로 확인할 수 있는가 | [always-on gateway](https://wikidocs.net/345906) / [Daily Briefing Bot](https://wikidocs.net/345926) |
| 실행 검증 기준 | 도구 실행 결과를 산출물/로그/공개 반영까지 확인하는가 | [도구 실행 결과 검증](https://wikidocs.net/346290) |

5장의 핵심은 “무슨 도구를 붙였나”가 아니다. 이 일이 검색인지, 실행인지, 정기 자동화인지, 메시징 운영인지, 반복 절차인지 구분하는 것이다. 도구가 실패하면 [복구 플레이북](https://wikidocs.net/345918)으로, 반복 절차가 안정되면 6장의 [Skill 운영](https://wikidocs.net/346235)으로 이어진다.

## 다음 장으로 가기 전 체크 질문

1. 이 일은 on-demand 도구 호출인가, cron 자동화인가?
2. 반복 절차가 Skill로 남길 만큼 안정됐는가?
3. gateway가 살아 있는 것과 실제 메시지 delivery가 되는 것을 구분했는가?
4. 권한 변경/삭제/외부 공개처럼 승인 필요한 작업을 분리했는가?
