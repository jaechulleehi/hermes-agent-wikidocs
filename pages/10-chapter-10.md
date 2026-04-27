## 10장. Hermes Agent 실제 운영 케이스

[Hermes Agent](00-hermes-agent-core-concepts.md)는 기능 목록으로만 보면 잘 와닿지 않는다. 실제로는 Slack 요청이 들어오고, 하비가 판단하고, 방울이/뽀동이/하망이 같은 역할형 에이전트가 이어받고, GitHub와 WikiDocs에 결과가 남는 흐름에서 가치가 드러난다.

10장은 앞 장에서 설명한 AI 개인비서, [역할형 에이전트](03-role-based-agent-splitting.md), cron, MCP, WikiDocs, 체크리스트가 실제 업무에서 어떻게 연결되는지 보여준다. 성공담을 모으는 장이 아니라, 요청이 운영 자산으로 바뀌는 과정을 남기는 장이다.

## 이 장에서 다루는 문제

| 순서 | 케이스 | 핵심 흐름 |
|---|---|---|
| 10-1 | 하망이와 WikiDocs 이미지 제작 | 이미지 요청 → 메시지 압축 → 제작/검수 → GitHub 발행 |
| 10-2 | 크론 조사에서 WikiDocs 발행 | cron 조사 → 하비 판단 → 방울이 확장조사 → 뽀동이 원고화 |
| 10-3 | 방울이/뽀동이 handoff | 근거 수집과 문서 구조화의 경계 |
| 10-4 | GitHub/WikiDocs 발행 | source of truth와 공개 배포 채널 분리 |
| 10-5 | Slack 스레드 분배 | 하비가 메인 창구로 일을 나누고 묶는 방식 |
| 10-6 | SEO/GEO 치트시트 자산화 | 이미지 제작 → QA → skill화 → 배포 문안 → WikiDocs 기록 |

## 실제 케이스를 읽는 기준

각 케이스는 결과물보다 흐름을 본다. 어떤 요청이 들어왔는지, 처음에 무엇을 헷갈렸는지, 어떤 역할로 나눴는지, 어디에서 검증했는지, 무엇이 다음 작업의 기준으로 남았는지가 핵심이다.

이 기준이 있어야 한 번 만든 결과물이 이미지 하나, 글 하나, Slack 답변 하나로 끝나지 않는다. 운영 케이스는 다음 요청을 더 빠르고 안전하게 처리하게 만드는 재사용 자산이어야 한다.

## 이 장에서 얻을 기준

- Slack 요청은 그대로 저장하지 않고 운영 흐름으로 재구성한다.
- 역할형 에이전트 이름보다 책임 경계를 먼저 본다.
- 공개 가능한 정보와 내부 판단을 분리한다.
- 반복될 작업은 skill, 체크리스트, [WikiDocs](07-why-we-write-the-wiki-first.md) 페이지 중 어디에 남길지 정한다.
- GitHub commit과 WikiDocs 발행까지 끝나야 운영 자산으로 본다.

## 책을 마무리하며

이 책의 핵심은 “AI를 잘 쓰는 법”보다 “AI가 일하는 구조를 운영하는 법”에 가깝다. 좋은 [AI 개인비서](00-ai-chatbot-vs-ai-personal-assistant.md) 하나로 시작해도, 실제 업무에서는 기억, 역할, 도구, 검증, 발행, 복구가 함께 움직인다.

다음 요청이 들어오면 이 장의 케이스처럼 보면 된다. 어떤 흐름으로 나눌 것인가. 어떤 기준으로 검증할 것인가. 어디에 남겨 다음 사람이 다시 쓸 수 있게 할 것인가.

## 이어서 읽기

- 첫 케이스는 [하망이와 WikiDocs 이미지를 만든 실제 운영 케이스](29-hamangi-wikidocs-image-production-case.md)다. 이미지 요청이 제작/검수/발행 흐름으로 바뀌는 과정을 본다.
- 자동화와 콘텐츠 발행이 이어지는 흐름은 [크론 조사에서 WikiDocs 발행까지 이어지는 AI 업무 자동화 케이스](30-cron-research-to-agent-content-workflow.md)에서 확인한다.
- 책 전체의 핵심 사례 흐름은 [방울이 확장조사와 뽀동이 글쓰기는 어떻게 이어질까](31-bangwooli-ppodongi-content-handoff.md), [GitHub와 WikiDocs로 콘텐츠를 발행하고 고치는 흐름](32-github-wikidocs-content-publishing-workflow.md), [Slack 스레드에서 하비가 일을 분배하는 방식](33-slack-thread-harvey-delegation-case.md)으로 이어진다.
