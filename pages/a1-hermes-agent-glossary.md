## 부록 A-1. Hermes Agent 용어집

이 부록은 책을 읽다가 막힐 때 바로 확인하는 짧은 용어표다. 용어를 외우기보다 “이 기능이 어떤 운영 문제를 해결하는가”를 기준으로 보면 된다.

최신 기능명과 설정값은 [Hermes Agent 공식 문서](https://hermes-agent.nousresearch.com/docs/)를 기준으로 확인하고, 이 용어집은 책 전체를 읽는 빠른 지도처럼 사용한다.

[TOC]

## 먼저 볼 핵심 용어

| 용어 | 한 줄 의미 | 실제로 쓰는 순간 |
|---|---|---|
| Hermes Agent / 에르메스 에이전트 / 헤르메스 에이전트 | 같은 도구를 가리키는 영문/한국어/검색 별칭 | 공식 표기는 Hermes Agent, 이 책의 대표 한국어 표기는 에르메스 에이전트로 읽을 때 |
| AI 개인비서 | Hermes Agent를 내 업무 목적에 맞게 운영하는 방식 | 요청을 먼저 받고 일을 나눌 메인 창구가 필요할 때 |
| 역할형 에이전트 | profile, SOUL.md, Skill, toolset, gateway 계정을 조합해 역할을 나누는 구조 | 조사/정리/실행처럼 실패 방식이 다른 일을 분리할 때 |
| Persistent Memory | `MEMORY.md`, `USER.md`, session search, 외부 memory provider를 포함하는 기억 체계 | 매번 반복 설명하지 않아도 되는 기준을 남길 때 |
| `MEMORY.md` | 에이전트의 개인 노트 | 환경 사실, 프로젝트 관례, 도구 사용 교훈을 남길 때 |
| `USER.md` | 사용자 프로필 | 선호하는 말투, 보고 방식, 협업 기대를 남길 때 |
| session search | 과거 세션을 찾아 요약하는 기능 | memory에 넣기엔 큰 과거 작업을 다시 찾을 때 |
| profile | 독립 Hermes home directory | config, `.env`, memory, sessions, skills, cron, gateway state를 나눌 때 |
| SOUL.md | `HERMES_HOME/SOUL.md`에서 로드되는 전역 personality 파일 | 에이전트의 정체성, 톤, 태도를 정할 때 |
| Context Files | 프로젝트별 작업 규칙 파일 | repo나 작업 폴더마다 다른 기준을 전달할 때 |
| Skill | 반복 절차와 검증 순서를 담는 on-demand 문서 | 같은 일을 다음에도 안정적으로 반복할 때 |
| toolset | 사용할 도구 묶음 | 역할별로 열어둘 도구와 막아둘 도구를 정할 때 |
| MCP | 외부 도구를 Hermes Agent에 연결하는 프로토콜 | 업무 시스템을 대화 흐름 안에서 쓰게 만들 때 |
| Messaging Gateway | Slack/Telegram/Discord/Email 같은 채널 연결 | 외부 채널에서 요청을 받고 결과를 전달할 때 |
| Nous Tool Gateway | web/image/TTS/browser 도구 호출을 Nous 경로로 라우팅하는 기능 | 별도 API key 없이 일부 도구를 쓰거나 라우팅할 때 |
| cron | 정해진 시간에 fresh session을 시작하는 예약 작업 | 정기 조사, 모니터링, 보고를 자동화할 때 |
| checkpoint/rollback | 파일 변경을 되돌리기 위한 복구 기능 | 위험한 수정 전후 상태를 비교하고 되돌릴 때 |
| credential pools | provider별 인증 정보를 여러 개 관리하는 기능 | API key/OAuth token을 나누고 회전할 때 |

## 자주 헷갈리는 구분

| 헷갈리는 조합 | 이렇게 구분한다 |
|---|---|
| memory vs session search | memory는 항상 들어와야 할 작은 기준, session search는 필요할 때 찾는 과거 작업이다. |
| `MEMORY.md` vs `USER.md` | `MEMORY.md`는 에이전트가 배운 환경/업무 기준, `USER.md`는 사용자 선호와 기대다. |
| Skill vs memory | Skill은 절차와 검증 순서, memory는 오래 유지할 사실과 선호다. |
| profile vs workspace | profile은 Hermes 상태 디렉터리, workspace는 실제 도구가 실행되는 위치다. profile은 샌드박스가 아니다. |
| SOUL.md vs AGENTS.md | SOUL.md는 에이전트 정체성과 톤, AGENTS.md는 프로젝트 작업 규칙이다. |
| MCP vs Messaging Gateway | MCP는 도구 연결, Messaging Gateway는 Slack/Telegram 같은 대화 채널 연결이다. |
| Messaging Gateway vs Nous Tool Gateway | 전자는 요청을 받는 채널 입구, 후자는 일부 도구 호출의 라우팅 경로다. |
| cron vs Skill | cron은 언제 시작할지, Skill은 어떻게 처리할지를 정한다. |

## 처음 읽을 때의 다섯 질문

1. 무엇을 기억해야 하는가?
2. 어떤 절차가 반복되는가?
3. 어떤 외부 도구와 연결해야 하는가?
4. 어떤 채널에서 요청을 받을 것인가?
5. 누가 요청을 받고 누가 실제 작업을 맡는가?

이 다섯 질문에 답하면 memory, Skill, MCP, Messaging Gateway, cron, profile, 역할형 에이전트가 각각 어디에 놓이는지 보인다.
