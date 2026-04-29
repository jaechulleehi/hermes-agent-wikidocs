## 0장. 에르메스 에이전트(Hermes Agent) 기초 가이드

에르메스 에이전트(Hermes Agent)를 처음 시작할 때 가장 위험한 흐름은 설치 명령부터 복사하고, 곧바로 Slack/gateway/cron/MCP를 붙이는 것이다. 그렇게 시작하면 “일하는 AI”를 만들기도 전에 설치 문제, provider 설정, gateway 연결, 권한, 비용 경계가 한꺼번에 섞인다.

그래서 0장은 기능을 모두 외우는 장이 아니라, Hermes Agent를 실제 업무에 붙이기 전 **무엇을 먼저 확인하고 어떤 순서로 확장할지**를 잡는 입문 흐름이다. 공식 docs와 GitHub는 Hermes Agent를 self-improving AI agent, CLI/TUI, messaging gateway, memory, skill, tool, cron, MCP를 갖춘 실행 환경으로 설명한다. 이 책은 그 기능을 “어떤 업무 목적을 안정적으로 반복하게 만들 것인가”라는 질문으로 다시 읽는다. 처음에는 [에르메스 에이전트(Hermes Agent)란 무엇인가](https://wikidocs.net/346055)를 먼저 읽고, 설치보다 확인 순서를 앞에 둔다.

![Hermes Agent 기초 가이드 흐름](../assets/images/chapter-heroes/ch00-hermes-foundation-guide-tool-badges-codex.webp)

이 책은 공식 문서를 대체하지 않는다. 공식 GitHub와 docs에서 설치/명령/설정 기준을 확인하고, 이 책에서는 AI 개인비서와 역할형 에이전트 운영 관점으로 다시 해석한다. 특히 0장에서는 Hermes Agent의 특징을 기능 목록이 아니라 목적별 운영 장치로 본다. 기억할 정보는 memory와 shared-memory에 남기고, 반복되는 절차는 Skill로 만들고, 과거 작업은 session search로 다시 찾고, 정기 작업은 cron으로 시작하고, 외부 업무 시스템은 MCP/gateway로 연결한다. 이 흐름을 잡아야 이후 장의 메인 창구, 역할 분리, 기억 시스템, Skill 운영이 하나의 AI 업무 자동화 구조로 이어진다.

## 0장을 읽는 순서

| 순서 | 페이지 | 먼저 답하는 질문 |
|---|---|---|
| 00-01 | 에르메스 에이전트(Hermes Agent)란 무엇인가 | Hermes Agent가 챗봇/IDE copilot과 무엇이 다른가 |
| 00-02 | 공식 GitHub/docs는 어디서 볼까 | 공식 source of truth와 이 책의 역할은 어떻게 다른가 |
| 00-03 | 설치와 세팅은 어떻게 시작할까 | 설치 명령보다 먼저/나중에 무엇을 검증해야 하는가 |
| 00-04 | CLI 첫 대화는 어떻게 시작할까 | 설치 후 첫 대화, 세션, slash command를 어떻게 확인하는가 |
| 00-05 | provider/model/config는 어떻게 확인할까 | 모델 라우팅과 설정 파일은 어디서 흔들리는가 |
| 00-06 | OpenClaw와 무엇이 다를까 | 기존 환경에서 Hermes로 넘어올 때 무엇을 버리고 가져올까 |
| 00-07 | Claude Code/Codex와 어떻게 다를까 | 코딩 에이전트와 운영형 개인비서는 어떻게 다르게 쓸까 |
| 00-08 | Docker/Gateway는 언제 필요할까 | 로컬 실습과 상시 운영의 경계는 어디인가 |
| 00-09 | 어떤 채널에서 쓸 수 있을까 | CLI/Slack/Telegram/Discord/Webhook 중 어디서 부를까 |
| 00-10 | 업데이트 전후 무엇을 점검할까 | 운영 중인 gateway/cron/skill을 깨지 않으려면 무엇을 볼까 |
| 00-11 | 맥미니 상시 운영은 무엇을 세팅할까 | 전용 Mac mini에서 tmux/launchd/절전/gateway를 어떻게 나눠 볼까 |

## Hermes Agent를 목적별로 보면 무엇이 다른가

Hermes Agent의 특징은 기능이 많다는 데서 끝나지 않는다. 중요한 차이는 사용자가 매번 처음부터 설명하지 않아도 되도록 기억, 절차, 도구, 채널을 나눠 운영할 수 있다는 점이다.

| 목적 | Hermes Agent에서 보는 장치 | 실제 활용 예 |
|---|---|---|
| 오래 남겨야 할 기준을 기억하기 | memory/profile/shared-memory | 사용자 선호, 팀 규칙, 프로젝트 공통 기준을 다음 대화에서도 유지한다 |
| 과거 작업을 다시 찾기 | session search/context compaction | 지난 작업의 결정과 결과를 새 요청에서 이어받는다 |
| 반복 절차를 재사용하기 | Skill | 검증 순서, 발행 절차, 리뷰 기준을 스스로 만들고 보강한다 |
| 정기적으로 일을 시작하기 | cron | 매일 조사, 주간 요약, 사용량 점검처럼 반복 시작점을 만든다 |
| 외부 업무 시스템과 연결하기 | MCP/tool/gateway | GitHub, Slack, WikiDocs, Google Workspace 같은 실제 업무 도구를 붙인다 |
| 여러 역할로 나눠 처리하기 | delegation/profile/역할형 에이전트 | 조사/정리/실행/이미지 제작을 분리하고 최종 결과만 통합한다 |

따라서 0장에서 먼저 잡아야 할 질문은 “어떤 기능을 켤까”가 아니라 “내 업무에서 무엇을 기억하고, 무엇을 반복하고, 무엇을 도구와 연결할까”다. 이 관점이 있어야 이후 장의 AI 개인비서, 역할형 에이전트, memory, Skill, MCP, cron이 하나의 업무 흐름으로 이어진다.

## 공식 docs 기준으로 먼저 알아야 할 것

공식 Hermes Agent docs는 Quick Links에서 Installation, Quickstart, Learning Path, Configuration, Messaging Gateway, Tools & Toolsets, Memory System, Skills System, MCP Integration, Voice Mode, Personality/SOUL.md, Context Files, Security, Architecture, FAQ를 안내한다. 이 목록은 “기능이 많다”는 뜻이 아니라, Hermes Agent가 대화창 하나가 아니라 운영 환경이라는 뜻이다.

GitHub README도 같은 방향을 강조한다. Hermes Agent는 Nous Research가 만든 self-improving AI agent이며, built-in learning loop, skill creation, memory, session search, messaging gateway, scheduled automation, delegation/parallel workstreams를 주요 특징으로 둔다. 그래서 처음 설치한 뒤 바로 업무 자동화를 기대하기보다, 아래 순서로 좁혀 가는 편이 안전하다.

```text
공식 GitHub/docs 확인
        ↓
설치와 기본 실행 확인
        ↓
첫 CLI 대화와 세션 확인
        ↓
provider/model/config 확인
        ↓
필요한 tool/toolset만 켜기
        ↓
gateway/채널 연결
        ↓
cron/skill/MCP 같은 운영 기능 확장
        ↓
업데이트/복구/보안 기준 마련
```

## 처음 세팅할 때 먼저 정할 것

| 구분 | 성급한 접근 | 안정적인 접근 |
|---|---|---|
| 설치 | 설치 명령만 복사한다 | 공식 docs와 GitHub를 확인하고 지원 환경을 먼저 본다 |
| 첫 실행 | gateway부터 켠다 | CLI에서 기본 chat과 세션을 먼저 확인한다 |
| 모델 | 아무 모델이나 붙인다 | provider/model/config 저장 위치와 라우팅 기준을 본다 |
| 도구 | 모든 tool을 켠다 | 필요한 toolset만 켜고 위험 작업 승인 기준을 둔다 |
| 메시징 | Slack/Telegram/Discord를 한 번에 붙인다 | 주 채널 하나를 먼저 안정화한다 |
| 자동화 | cron을 바로 만든다 | fresh session에서 혼자 실행될 prompt인지 확인한다 |
| 업데이트 | 최신 버전이면 바로 올린다 | gateway, cron, config, skill, 로그를 전후로 확인한다 |

공식 Tips & Best Practices에서 반복해서 강조하는 것도 같은 방향이다. 처음부터 긴 자동화를 만들기보다, 요청을 구체적으로 쓰고, 필요한 맥락을 앞에 두고, 반복 지시는 AGENTS.md나 Skill로 옮기고, 에이전트가 도구로 직접 확인하게 해야 한다.

| 사용 습관 | 좋은 시작 방식 |
|---|---|
| 요청 쓰기 | “고쳐줘”보다 파일/증상/기대 결과를 함께 준다 |
| 맥락 제공 | 오류 메시지, 경로, 완료 기준을 첫 요청에 넣는다 |
| 반복 지시 | 매번 말하지 말고 AGENTS.md, SOUL.md, Skill로 분리한다 |
| 도구 사용 | 사람이 순서를 모두 지시하기보다 확인할 목표와 제한을 준다 |
| 비용/성능 | 긴 세션은 `/usage`, `/compress`, 모델 전환 기준을 함께 본다 |

0장에서 중요한 것은 “다 알기”가 아니다. 지금 내가 어느 단계에서 막혔는지 구분하는 것이다. 설치 문제인지, provider 문제인지, config 문제인지, gateway 문제인지, 권한 문제인지가 분리되어야 이후 장의 AI 개인비서/역할형 에이전트 운영이 흔들리지 않는다.

## 0장을 마치면 남아야 할 산출물

0장은 읽고 끝나는 소개 장이 아니라, 내 Hermes Agent 환경을 처음 설명할 수 있게 만드는 준비 장이다. 0장을 지나 1장으로 넘어가기 전에는 아래 정도가 정리되어 있어야 한다.

| 산출물 | 왜 필요한가 | 연결되는 다음 장 |
|---|---|---|
| 내가 쓰는 실행 환경 | macOS/Linux/WSL2/Termux/Docker 중 어디서 돌리는지 알아야 설치 문제를 좁힐 수 있다 | 00-03 설치/세팅 |
| 기본 CLI 검증 결과 | Slack/gateway 문제와 provider/model 문제를 분리하는 기준선이다 | 00-04 CLI 첫 대화 |
| provider/model/config 위치 | 비용, 모델 라우팅, API key/OAuth 경계를 헷갈리지 않기 위해 필요하다 | 00-05 provider/model/config |
| 주 사용 채널 | CLI로만 쓸지 Slack 같은 메시징 채널로 부를지 정해야 gateway가 필요해진다 | 00-09 사용 채널 |
| 상시 운영 여부 | 가끔 쓰는 도구인지 Mac mini/서버에서 계속 켜둘 AI 개인비서인지에 따라 운영 방식이 달라진다 | 00-08 Docker/Gateway, 00-11 맥미니 상시 운영 |
| 업데이트/복구 기준 | 최신 기능보다 중요한 것은 업데이트 후 같은 업무 흐름이 살아 있는지다 | 00-10 업데이트 검증, 09장 복구 플레이북 |

간단히 적으면 충분하다. 예를 들어 “macOS Mac mini에서 Slack gateway를 상시 운영하고, provider는 OpenRouter를 쓰며, cron 결과는 Slack thread로 받는다”처럼 한 문장으로 현재 운영 그림을 말할 수 있으면 된다.

## 처음 세팅 메모 템플릿

처음 세팅할 때는 비밀번호나 토큰을 남기지 말고, 아래 항목만 별도로 적어두면 이후 디버깅이 쉬워진다.

```text
실행 환경:
설치 방식:
주 사용 provider/model:
config 확인 위치:
첫 CLI 대화 확인 여부:
사용할 toolset 범위:
주 사용 채널:
gateway 필요 여부:
cron/Skill/MCP 확장 예정 여부:
업데이트 전후 검증 질문:
```

이 템플릿의 목적은 내부 값을 공개하는 것이 아니라, 문제를 재현할 최소 맥락을 남기는 것이다. 실제 토큰, webhook URL, channel ID, 개인 경로는 팀 내부 보안 문서나 secret store에 둔다.

## 막혔을 때 바로 가는 길

처음 독자는 전체를 순서대로 읽어도 되지만, 이미 막힌 상태라면 아래처럼 들어가면 빠르다.

| 지금 증상 | 먼저 읽을 곳 | 다음에 확인할 곳 |
|---|---|---|
| Hermes Agent가 무엇인지 아직 헷갈린다 | [00-01 핵심 개념](https://wikidocs.net/346055) | 01장 AI 개인비서/AI 팀 |
| 설치는 했는데 첫 대화가 안 된다 | [00-04 CLI 첫 대화](https://wikidocs.net/346251) | [00-05 provider/model/config](https://wikidocs.net/346252) |
| CLI는 되는데 Slack에서 답이 없다 | [00-08 Docker/Gateway](https://wikidocs.net/346139) | [05-02 always-on gateway](https://wikidocs.net/345906) |
| cron 결과가 어디로 갔는지 모르겠다 | [05-03 Daily Briefing Bot](https://wikidocs.net/345926) | [08-1 운영 질문 분류](https://wikidocs.net/345912) |
| 비용이 어디서 나가는지 모르겠다 | [00-05 provider/model/config](https://wikidocs.net/346252) | [05장 외부 도구/검색 비용](https://wikidocs.net/345907) |
| 여러 봇이 동시에 답해 시끄럽다 | [08-2 멀티봇 스레드](https://wikidocs.net/345913) | 09장 체크리스트/복구 |

이 표의 목적은 “정답 페이지”를 찍어 주는 것이 아니라, 재설치/재시작/토큰 교체를 하기 전에 문제 범위를 좁히는 것이다.

## 이 책과 공식 문서를 함께 보는 법

공식 문서는 기능의 source of truth다. 설치 명령, CLI command, config option, messaging gateway 설정, security boundary, architecture처럼 바뀔 수 있는 내용은 공식 docs를 먼저 확인해야 한다.

이 책은 그 기능을 실제 업무에 붙일 때의 판단 기준을 다룬다. 예를 들어 공식 docs가 `hermes gateway setup`을 설명한다면, 이 책은 “왜 gateway status와 실제 Slack delivery를 따로 확인해야 하는가”를 설명한다. 공식 docs가 skills system을 설명한다면, 이 책은 “어떤 반복 업무를 skill로 남기고 어떤 정보는 memory에 남기면 안 되는가”를 다룬다.
