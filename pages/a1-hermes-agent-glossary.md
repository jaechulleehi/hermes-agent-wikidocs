## 부록 A-1. Hermes Agent 용어집

이 부록은 책을 읽다가 반복해서 만나는 용어를 빠르게 확인하기 위한 참조 페이지다. Hermes Agent는 챗봇 하나를 켜는 도구라기보다, 기억/절차/도구/채널을 나눠 실제 업무 흐름을 운영하게 만드는 AI 에이전트 프레임워크다.

용어를 외우기보다 “공식 docs에서는 무엇이라고 부르고, 실제 운영에서는 어떤 문제를 해결하는가”로 읽으면 전체 흐름이 훨씬 덜 복잡해진다.

## 먼저 잡을 핵심 용어

| 용어 | 공식 docs 기준 | 이 책에서의 운영 의미 |
|---|---|---|
| AI 개인비서 | Hermes Agent를 특정 사용자/업무 목적에 맞게 운영하는 활용 방식 | 사용자의 요청을 먼저 받고 흐름을 정리하는 메인 창구 |
| 역할형 에이전트 | profiles, SOUL.md, skills, toolsets, gateway 계정을 조합해 분리 운영할 수 있는 구조 | 조사/정리/실행처럼 실패 방식이 다른 일을 나눠 맡는 에이전트 |
| Persistent Memory | `MEMORY.md`, `USER.md`, session search, 외부 memory provider로 구성되는 기억 체계 | 오래 유지해야 할 사용자 선호, 환경 사실, 운영 기준을 남기는 기억 |
| `MEMORY.md` | 에이전트의 개인 노트 저장소 | 환경 사실, 프로젝트 관례, 도구 사용 교훈을 남긴다 |
| `USER.md` | 사용자 프로필 저장소 | 사용자 선호, 말투, 기대, 협업 방식을 남긴다 |
| session search | SQLite 기반 과거 세션 검색과 요약 | memory에 넣기엔 크지만 다시 찾아야 하는 과거 작업을 회상한다 |
| profile | 독립 Hermes home directory | config, `.env`, SOUL.md, memories, sessions, skills, cron, gateway state를 분리한다 |
| SOUL.md | `HERMES_HOME/SOUL.md`에서 로드되는 전역 personality 파일 | 에이전트의 정체성/톤/태도를 정한다 |
| Context Files | `.hermes.md`, `HERMES.md`, `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.cursor/rules/*.mdc` | 프로젝트별 작업 규칙과 맥락을 전달한다 |
| Skill | `~/.hermes/skills/`에 저장되는 on-demand knowledge document | 반복 절차와 검증 순서를 재사용 가능한 작업 기준으로 남긴다 |
| toolset | 사용할 도구 묶음 | 조사만 필요한 일에는 수정/실행 도구를 제한하는 식으로 범위를 줄인다 |
| MCP | Model Context Protocol 서버 연결 기능 | 외부 도구/업무 시스템을 Hermes Agent가 쓸 수 있게 붙인다 |
| Messaging Gateway | Telegram/Discord/Slack/WhatsApp/Signal/Email 등 메시징 채널을 연결하는 백그라운드 프로세스 | 외부 채널에서 요청을 받고 세션/cron/전달을 운영하는 입구 |
| Nous Tool Gateway | Nous 구독으로 web/image/TTS/browser 도구 API를 라우팅하는 기능 | 별도 API 키 없이 일부 도구를 쓰게 해주는 도구 실행 경로 |
| cron | scheduled tasks 기능 | 정해진 시간이나 주기로 fresh agent session을 시작한다 |
| checkpoint/rollback | 파일 시스템 변경을 되돌리기 위한 복구 기능 | 위험한 수정 전후 상태를 비교하고 필요하면 되돌린다 |
| credential pools | provider별 인증 정보를 여러 개 관리하는 기능 | API 키/OAuth 토큰을 안전하게 나누고 회전한다 |

## 헷갈리기 쉬운 구분

| 헷갈리는 조합 | 구분 기준 |
|---|---|
| memory vs session search | memory는 항상 들어와야 할 작은 기준이고, session search는 과거 대화를 필요할 때 찾아오는 기능이다. |
| `MEMORY.md` vs `USER.md` | `MEMORY.md`는 에이전트가 배운 환경/업무 기준이고, `USER.md`는 사용자 선호와 기대다. |
| Skill vs memory | Skill은 반복 절차와 검증 순서이고, memory는 선호나 기준 같은 안정적인 사실이다. |
| profile vs workspace | profile은 Hermes 상태 디렉터리이고, workspace/working directory는 도구가 실행되는 위치다. profile은 샌드박스가 아니다. |
| SOUL.md vs AGENTS.md | SOUL.md는 에이전트 정체성과 톤이고, AGENTS.md는 프로젝트 작업 규칙과 검증 방식이다. |
| MCP vs Messaging Gateway | MCP는 도구 서버를 연결하는 방식이고, Messaging Gateway는 Slack/Telegram 같은 채널에서 요청을 받는 입구다. |
| Messaging Gateway vs Nous Tool Gateway | Messaging Gateway는 대화 채널 연결이고, Nous Tool Gateway는 web/image/TTS/browser 도구 호출을 Nous 구독으로 라우팅하는 기능이다. |
| cron vs Skill | cron은 언제 시작할지를 정하고, Skill은 어떻게 처리할지를 남긴다. |
| 역할형 에이전트 vs toolset | 역할형 에이전트는 맡는 일의 성격이고, toolset은 실행할 수 있는 도구 범위다. |
| GitHub 원본 vs WikiDocs 공개본 | GitHub는 수정과 검증의 source of truth이고, WikiDocs는 독자가 읽는 배포 채널이다. |

## 공식 docs와 함께 확인할 것

| 확인할 내용 | 공식 docs에서 볼 곳 |
|---|---|
| 설치/업데이트 | Getting Started / Installation / Updating |
| CLI 명령어 | Reference / CLI Commands |
| slash command | Reference / Slash Commands |
| provider/model 설정 | Integrations / Providers, User Guide / Configuration |
| memory/session search | User Guide / Features / Persistent Memory |
| context files | User Guide / Features / Context Files |
| skills | User Guide / Features / Skills System |
| MCP | User Guide / Features / MCP |
| cron | User Guide / Features / Scheduled Tasks |
| messaging gateway | User Guide / Messaging Gateway |
| tool gateway | User Guide / Features / Nous Tool Gateway |
| profiles | User Guide / Profiles |
| security/checkpoint | User Guide / Security, Checkpoints and Rollback |

## 처음 읽을 때의 기준

처음부터 모든 용어를 외울 필요는 없다. 아래 다섯 가지 질문만 잡으면 된다.

1. 무엇을 기억해야 하는가?
2. 어떤 절차가 반복되는가?
3. 어떤 외부 도구와 연결해야 하는가?
4. 어떤 채널에서 요청을 받을 것인가?
5. 누가 요청을 받고 누가 실제 작업을 맡는가?

이 질문에 답하면 memory, Skill, MCP, Messaging Gateway, cron, profile, 역할형 에이전트가 각각 어디에 놓이는지 자연스럽게 보인다.
