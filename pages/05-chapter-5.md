## 5장. 외부 도구/MCP/채널 연동과 AI 워크플로우 자동화

Hermes Agent가 강해지는 순간은 답변을 잘할 때가 아니라 외부 도구와 연결되어 실제 업무를 움직일 때다. Google Workspace, Notion, GitHub, WikiDocs, MCP, CLI, API, cron, gateway, Daily Briefing Bot은 모두 기능 목록이 아니라 AI 워크플로우 자동화와 업무 흐름을 이어주는 연결 장치다. MCP는 Model Context Protocol 기반의 도구 연결면이고, 반복 절차를 재사용하는 Skill은 6장에서 따로 다룬다.

![5장 외부 도구 MCP 자동화 운영 구조](../assets/images/chapter-heroes/ch05-tools-mcp-automation-ops-codex.webp)

하지만 외부 도구는 붙였다고 끝나지 않는다. 계정, 권한 범위, 실행 위치, gateway 상태, cron prompt, 결과 전달 위치를 함께 봐야 한다. 특히 GitHub CLI, MCP server, API 직접 호출은 모두 외부 연결이지만 인증, 로그, 검증 방식이 다르다. 5장은 도구를 많이 붙이는 법보다 **어떤 업무 흐름에 어떤 연결 방식을 붙일지**를 정하는 장이다.

## 이 장에서 다루는 문제

| 기능 | 흔한 착각 | 실제 사용 기준 |
|---|---|---|
| MCP | 연결만 되면 자동화가 끝난다 | 어떤 업무 흐름에 붙을지 먼저 정한다 |
| CLI/API | 명령이나 endpoint만 알면 된다 | 실행 환경, 인증 scope, 로그, retry 기준을 확인한다 |
| Google Workspace/Notion/WikiDocs | API 인증만 되면 된다 | source of truth, 계정/권한/삭제 위험/공유 범위를 분리한다 |
| gateway | status가 OK면 끝이다 | 메시징 delivery, cron 실행, 로그를 함께 본다 |
| Daily Briefing Bot | 뉴스 요약 예제다 | fresh session 기반 자동화 패턴이다 |
| 웹 검색/리서치 | 검색 API만 붙이면 조사가 된다 | 무료/유료 검색원, 출처 품질, 재현성, 후속 검증을 나눈다 |
| 내장 도구/toolset | 도구가 많을수록 좋다 | 역할별로 필요한 도구 범위와 위험도를 제한한다 |
| terminal backend | 명령만 맞으면 된다 | local/docker/ssh/modal 같은 실행 위치와 격리 수준을 고른다 |
| 실행 결과 검증 | exit code 0이면 끝이다 | diff, 공개 반영, delivery, rollback 경로까지 확인한다 |
| 실행 방식 | 에이전트만 나누면 운영도 자동으로 정리된다 | 직접 처리/profile/delegation/subagent/cron을 구분한다 |

## 공식 문서 기능을 운영 기준으로 읽기

공식 Hermes Agent 문서를 5장에서 그대로 옮기지는 않는다. 이 책은 공식 docs를 대신하는 설정 사전이 아니라, 입문자가 외부 연결을 업무 흐름에 어떻게 배치할지 판단하게 돕는 안내서다. 최신 지원 provider, web backend, platform, 설정값은 공식 docs와 부록 A-4에서 확인하고, 본문에서는 “무엇을 하는 기능인가”와 “실제 운영에서 무엇을 확인해야 하는가”를 나눠서 본다.

| 기능 | 공식 기준으로 보는 뜻 | 실제 운영에서의 질문 |
|---|---|---|
| MCP | Model Context Protocol의 약자로, 외부 서비스와 도구를 Hermes Agent 대화 흐름에 연결하는 방식 | 어떤 계정/권한/scope로 연결했고, 위험 작업은 어디서 멈출 것인가 |
| CLI | 로컬 명령과 기존 개발자 워크플로우를 Hermes Agent 실행 흐름에서 호출하는 방식 | 어느 HOME/path/auth 환경에서 실행되는지 확인했는가 |
| API | REST/GraphQL 같은 endpoint를 직접 호출해 세밀하게 제어하는 방식 | token scope, pagination, rate limit, retry를 관리하는가 |
| cron | 정해진 시간이나 주기에 fresh session으로 작업을 실행하는 예약 자동화 | prompt가 혼자 실행될 만큼 충분하고, 결과가 어디로 전달되는가 |
| gateway | 메시징 플랫폼, cron 실행, delivery를 이어주는 always-on 운영 축 | process가 살아 있는 것과 실제 메시지가 도착한 것을 구분했는가 |
| Daily Briefing Bot | cron, search, summarization, messaging delivery가 묶인 예제 | 뉴스 요약을 넘어 정기 모니터링/신호 분류/후속 실행으로 확장할 수 있는가 |
| 웹 검색/리서치 | web search, browser, 검색 백엔드로 새 정보를 찾는 기능 | 무료 검색으로 충분한지, 유료 API가 필요한지, 어떤 출처를 믿을지 정했는가 |
| 내장 도구/toolset | web, file, terminal, browser, memory, messaging, MCP 같은 도구 묶음 | 역할별로 필요한 toolset만 열었고 위험 도구는 제한했는가 |
| terminal backend | local, docker, ssh, sandbox 같은 실행 위치와 격리 방식 | 명령이 어느 환경에서 실행되고 credential이 어디까지 노출되는가 |
| 도구 실행 검증 | 도구 호출 결과를 다음 판단에 쓰기 전 확인하는 절차 | exit code, diff, 공개 반영, 메시지 delivery, rollback 가능성을 확인했는가 |
| delegation/subagent | 작업을 하위 에이전트나 별도 실행 단위로 나누는 방식 | 역할 분리와 실행 방식을 섞지 않고 최종 통합자를 정했는가 |

## 도구 선택 기준이 드러나는 순간

WikiDocs 작업에서도 외부 도구 판단 기준이 계속 등장했다. GitHub가 source of truth였고, WikiDocs는 공개 배포 채널이었다. 그래서 원고를 고친 뒤에는 “파일이 바뀌었는가”만 보지 않고, 공개 화면에서 본문 링크와 이미지가 제대로 작동하는지까지 확인해야 했다.

콘텐츠 자동화도 마찬가지다. 방울이의 크론 조사는 신호를 찾고, 좋은 신호만 shared-memory와 Obsidian으로 이어진다. 사용자가 “넘겨”라고 판단하면 뽀동이가 본문 구조를 만들고, 하비가 최종 통합하며, 필요하면 하망이가 이미지를 만든다. 여기서 cron은 신호를 찾는 장치이고, shared-memory는 handoff 위치이며, WikiDocs는 공유 자산이다. 단일 기능이 아니라 [역할형 에이전트](https://wikidocs.net/345925)와 도구가 연결된 운영 흐름으로 봐야 한다.

## 웹 검색/리서치 도구는 어떻게 고를까

웹 검색은 MCP/API/CLI와 비슷해 보이지만 실제로는 성격이 조금 다르다. 검색 도구는 “정답을 실행하는 도구”가 아니라 후보 출처를 가져오는 입력 장치다. 그래서 검색 품질은 API 이름보다 검색원, 최신성, 중복 제거, 원문 접근, 인용 가능성, 후속 검증 기준으로 봐야 한다.

공식 Integrations 기준으로 Hermes Agent의 web backend는 Firecrawl, Parallel, Tavily, Exa처럼 공식 docs에서 지원 범위를 확인해야 하는 영역이다. 반면 DuckDuckGo/DDGS, SearXNG, Brave Search API, Perplexity, Google Suggest 같은 도구는 별도 운영 후보나 리서치 경로로 볼 수 있다. 이 둘을 섞어서 “공식 Hermes web backend”처럼 쓰면 나중에 설정과 검증 기준이 꼬인다.

현재 우리 운영에서는 웹 리서치를 시작할 때 무료 검색 계열을 먼저 쓴다. DuckDuckGo 기반 DDGS처럼 API key 없이 쓸 수 있는 검색 경로는 빠른 신호 탐색과 fallback에 적합하다. 필요하면 브라우저로 원문을 직접 열어 확인하고, SEO/GEO 키워드 확장은 Google Suggest 같은 공개 suggest endpoint를 별도로 본다. 반대로 Firecrawl, Parallel, Tavily, Exa 같은 공식 web backend나 Brave Search API, Perplexity 같은 별도 상용 도구는 대량 검색, extract/crawl, 안정적인 API 응답, 검색 결과 품질 관리가 필요할 때 후보가 된다. SearXNG는 직접 호스팅하거나 공개 인스턴스를 쓰는 무료 메타검색 옵션으로 볼 수 있지만, 공개 인스턴스는 안정성과 차단 위험을 따로 봐야 한다.

| 방식 | 비용/인증 | 강한 경우 | 조심할 점 |
|---|---|---|---|
| DuckDuckGo/DDGS | 무료, 보통 API key 없음 | 빠른 초기 조사, 키워드 후보, 가벼운 사실 확인 | 결과 재현성과 대량 호출 안정성이 약할 수 있다 |
| SearXNG | 자체 호스팅이면 무료에 가깝다 | 여러 검색원을 메타검색으로 묶고 싶을 때 | 공개 인스턴스 안정성, 차단, 운영 부담 |
| Firecrawl/Parallel/Tavily/Exa | 공식 docs에서 지원 범위 확인 | Hermes Agent web backend로 붙일 때 | 비용, quota, 설정값 변경, token 관리 |
| Brave/Perplexity 등 별도 검색 API | 유료 또는 API key 필요 | 별도 리서치 경로나 질문형 리서치가 필요할 때 | 공식 web backend와 구분하고 원문 근거를 재검증 |
| Browser 직접 확인 | 별도 API 없이 화면 확인 | 로그인 페이지, 동적 페이지, 원문 검수 | 느리고 깨지기 쉬워 자동화의 마지막 단계로 둔다 |

실제 사용 기준은 단순하다. “오늘 빠르게 신호를 찾는가”라면 무료 검색과 브라우저 확인으로 충분한 경우가 많다. “매일 같은 주제를 안정적으로 모니터링하고, 결과를 Slack이나 WikiDocs로 넘기는가”라면 유료 API나 자체 SearXNG 같은 운영형 백엔드를 검토한다. “공유 글에 근거로 넣을 것인가”라면 검색 결과 요약이 아니라 원문 URL과 공식 출처 확인이 우선이다.

## 초보자용 연결 방식 선택 순서

외부 도구를 붙일 때 처음부터 MCP/API/CLI/브라우저를 모두 비교하면 어렵다. 먼저 “내가 하려는 일이 무엇인가”를 아래 순서로 좁힌다.

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

이 순서로 보면 “무슨 MCP를 붙일까”보다 “이 업무가 검색인지, 실행인지, 반복 절차인지, 상시 메시징인지”가 먼저 보인다.

## 5장을 마치면 남아야 할 것

| 남아야 할 것 | 확인 질문 | 연결되는 글 |
|---|---|---|
| 연결 방식 선택표 | 이 일은 CLI/API/MCP/gateway/cron/skill 중 무엇이 맞는가 | [네 가지 실행 방식](https://wikidocs.net/346124) |
| 권한/인증 경계 | 계정 권한, token, scope, 로그 노출 범위를 분리했는가 | [Google Workspace와 MCP](https://wikidocs.net/345903) / [MCP 외부 도구](https://wikidocs.net/346231) |
| 상시 운영 점검표 | gateway, delivery, cron, channel을 따로 확인할 수 있는가 | [always-on gateway](https://wikidocs.net/345906) / [Daily Briefing Bot](https://wikidocs.net/345926) |
| 실행 검증 기준 | 도구 실행 결과를 exit code가 아니라 산출물/로그/공개 반영까지 확인하는가 | [도구 실행 결과 검증](https://wikidocs.net/346290) |

## 이 장에서 얻을 기준

- MCP는 Model Context Protocol 기반의 “도구 연결”이지 “운영 완성”이 아니다.
- CLI는 빠르고 익숙하지만 실행 환경/HOME/path/auth 차이를 확인해야 한다.
- API는 세밀하지만 token scope, pagination, rate limit, retry를 직접 봐야 한다.
- cron은 혼자 실행되는 fresh session이므로 prompt 안에 필요한 맥락이 있어야 한다.
- gateway는 항상 켜져 있다는 느낌보다 실제 전달/로그/상태를 함께 봐야 한다.
- 내장 도구와 toolset은 역할별로 열어야 하며, 모든 도구를 항상 열어두는 것이 좋은 운영은 아니다.
- terminal 도구는 강력하지만 실행 위치, credential, 삭제/배포 위험, rollback 기준을 함께 봐야 한다.
- 도구 실행 결과는 exit code만 보지 말고 diff, 공개 반영, delivery, source of truth 기준으로 다시 확인해야 한다.
- 외부 도구 작업에는 삭제/공개/권한 변경 같은 위험 작업을 분리하는 승인 기준이 필요하다.

5장을 읽을 때 핵심은 “무슨 도구를 붙였나”가 아니다. 같은 작업도 CLI로 즉시 실행할지, MCP tool로 대화 흐름에 붙일지, API로 정기 조회할지, cron으로 자동화할지, 반복 절차를 [Skill](https://wikidocs.net/346235)로 남길지에 따라 필요한 맥락과 검증이 달라진다. 도구가 실패하면 [복구 플레이북](https://wikidocs.net/345918)으로, 반복 절차가 안정되면 6장의 Skill 운영으로 이어진다.

이 기준은 4장의 [AI 에이전트 기억 시스템](https://wikidocs.net/345902)과 이어진다. 도구 자동화가 실패할 때도 많은 원인은 모델이 아니라 profile, 권한, 경로, source of truth 경계에 있다.

## 다음 장으로 가기 전 체크 질문

1. 이 일은 on-demand 도구 호출인가, cron 자동화인가?
2. 반복 절차가 skill로 남길 만큼 안정됐는가?
3. gateway가 살아 있는 것과 실제 메시지 delivery가 되는 것을 구분했는가?
4. 권한 변경/삭제/외부 공개처럼 승인 필요한 작업을 분리했는가?
