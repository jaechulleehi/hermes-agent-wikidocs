## 0장. 에르메스 에이전트(Hermes Agent) 기초 가이드

에르메스 에이전트(Hermes Agent)를 처음 찾는 독자는 보통 세 가지를 먼저 묻는다. [공식 GitHub](https://github.com/NousResearch/hermes-agent)와 [공식 문서](https://hermes-agent.nousresearch.com/docs/)는 어디에 있는지, 설치와 세팅은 어떻게 시작하는지, OpenClaw나 Claude Code/Codex 같은 다른 AI 에이전트와 무엇이 다른지다. 0장은 그 기초 질문을 먼저 정리하는 입문 장이다.

이 장은 공식 문서를 그대로 번역하지 않는다. 공식 문서의 기준은 확인하되, 독자가 실제로 에르메스 에이전트를 AI 개인비서, AI 자동화, 업무 자동화, 나만의 AI 팀 운영에 붙일 때 먼저 알아야 할 판단 기준으로 바꿔 설명한다.

## 이 장에서 다루는 질문

| 글 | 검색자가 묻는 질문 | 이 장의 답 |
|---|---|---|
| [00-1](https://wikidocs.net/346055) | 에르메스 에이전트(Hermes Agent)란 무엇인가 | self-improving AI agent를 업무 자동화 운영 구조로 읽는다 |
| [00-2](https://wikidocs.net/346136) | Hermes Agent 공식 GitHub와 공식 문서는 어디인가 | 공식 저장소와 공식 docs를 확인하고, 이 책의 해설 범위를 구분한다 |
| [00-3](https://wikidocs.net/346137) | Hermes Agent 설치와 세팅은 어떻게 시작하나 | one-line installer, provider 설정, 기본 chat 검증 순서로 본다 |
| [00-4](https://wikidocs.net/345889) | Hermes Agent와 OpenClaw는 무엇이 다른가 | 이름 변경이 아니라 memory/profile/skill/cron/gateway 중심의 운영 전환이다 |
| [00-5](https://wikidocs.net/346138) | Hermes Agent와 Claude Code/Codex는 어떻게 다른가 | 코딩 에이전트와 업무 자동화 운영 환경의 차이를 구분한다 |
| [00-6](https://wikidocs.net/346139) | Hermes Agent Docker/Gateway는 언제 필요한가 | 컨테이너 실행과 메시징 gateway를 항상 켜진 운영 구조로 이해한다 |

## 공식 문서를 먼저 봐야 하는 이유

Hermes Agent는 Nous Research가 공개한 오픈소스 AI 에이전트다. 공식 문서는 Hermes Agent를 “경험에서 skill을 만들고, 사용 중 개선하며, 세션 사이에서 기억을 유지하는 self-improving AI agent”로 설명한다. 이 표현은 단순 홍보 문구가 아니라 책 전체의 기준점이다.

다만 공식 문서만 보면 기능 목록이 먼저 보인다. installation, quickstart, memory, skills, MCP, cron, gateway, Docker, delegation 같은 항목이 따로 보이기 때문이다. 실제 운영에서는 이 기능들이 따로 움직이지 않는다. 요청을 받고, 기억을 참고하고, 도구를 실행하고, 결과를 남기는 하나의 AI 워크플로우로 이어진다.

## 0장을 읽고 나서 잡아야 할 기준

1. Hermes Agent는 챗봇 wrapper가 아니라 장기 운영을 전제로 한 AI 에이전트다.
2. 설치는 시작일 뿐이고, provider 설정과 기본 chat 검증이 먼저다.
3. gateway, cron, skill, MCP는 기능명이 아니라 업무 흐름의 연결 지점이다.
4. Claude Code/Codex는 강한 코딩 에이전트이고, Hermes Agent는 그것들을 포함해 여러 실행 방식을 조율할 수 있는 운영 환경에 가깝다.
5. OpenClaw에서 Hermes로 넘어오는 일은 도구 교체보다 기억/스킬/프로필/자동화 기준을 다시 세우는 일이다.

## 다음 장으로 이어지는 방식

0장에서 기본 용어와 비교 기준을 잡았다면, [1장](https://wikidocs.net/345888)에서는 왜 AI를 챗봇 하나가 아니라 AI 개인비서와 역할형 에이전트로 나눠 봐야 하는지 다룬다. 기초 장은 “무엇인가/어디서 시작하나”에 답하고, 1장부터는 “어떻게 업무 구조로 굴릴 것인가”에 답한다.
