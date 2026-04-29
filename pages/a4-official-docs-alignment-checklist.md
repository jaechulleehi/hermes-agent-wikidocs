## 부록 A-4. Hermes Agent 공식 docs 정합성 점검표

이 페이지는 책의 설명을 [Hermes Agent 공식 docs](https://hermes-agent.nousresearch.com/docs/)와 맞춰 보기 위한 점검표다. 한국어 책에서는 독자가 이해하기 쉽도록 운영 관점으로 풀어 쓰지만, 기능명/명령어/설정 경로/지원 플랫폼은 공식 docs와 맞아야 한다.

정합성을 볼 때는 “비슷한 뜻인가”보다 “독자가 그대로 따라 했을 때 공식 docs와 충돌하지 않는가”를 기준으로 본다.

## 1. 공식 docs를 먼저 볼 항목

| 항목 | 공식 docs 기준 | 책에서 조심할 점 |
|---|---|---|
| 설치 | Getting Started / Installation | 설치 명령과 전제 조건을 임의로 바꾸지 않는다. |
| 업데이트 | Getting Started / Updating | 업데이트 후 `hermes doctor`, 설정/도구 확인 흐름을 함께 본다. |
| CLI | User Guide / CLI, Reference / CLI Commands | `hermes chat`, `hermes setup`, `hermes model`, `hermes doctor` 같은 명령어를 정확히 쓴다. |
| 설정 | User Guide / Configuration | `config.yaml`, `.env`, `auth.json`의 역할을 구분한다. |
| provider/model | Integrations / Providers | provider 이름, OAuth/API key 방식, env var 이름을 추측하지 않는다. 책에는 선택 기준을 두고 최신 목록은 공식 docs에서 확인하게 한다. |
| toolset/tools | Reference / Tools, Toolsets | tool 이름과 toolset 이름을 역할명처럼 바꿔 쓰지 않는다. |
| memory | User Guide / Persistent Memory | `MEMORY.md`, `USER.md`, session search, 외부 provider를 구분한다. |
| context files | User Guide / Context Files | `.hermes.md`, `HERMES.md`, `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, SOUL.md의 로딩 위치를 구분한다. |
| Skill | User Guide / Skills System | Skill은 on-demand knowledge document이며, 반복 절차와 검증 기준으로 설명한다. |
| Integrations | Integrations | provider, web backend, browser, voice, API server, plugin, memory provider는 전체 목록을 본문에 옮기지 않고 활용 기준과 확인 위치를 나눈다. |
| Guides / Tutorials / Tips | Guides | Tips & Best Practices, Daily Briefing Bot, delegation, cron, skills 같은 실전 가이드는 기능 목록보다 사용 습관과 운영 패턴으로 반영한다. |
| MCP | User Guide / MCP | stdio/HTTP 서버, tool prefix, resource/prompt utility를 임의로 단순화하지 않는다. |
| cron | User Guide / Scheduled Tasks | fresh session, delivery target, skill attachment, workdir 기준을 확인한다. |
| Messaging Gateway | User Guide / Messaging Gateway | Slack/Telegram/Discord 등 채널 연결과 session/cron delivery를 설명한다. |
| Nous Tool Gateway | User Guide / Nous Tool Gateway | web/image/TTS/browser 도구 API 라우팅 기능으로 따로 설명한다. |
| profiles | User Guide / Profiles | profile은 상태 분리이지 보안 sandbox가 아니라는 점을 명확히 한다. |
| security | User Guide / Security | allowlist, pairing, dangerous command approval, secret 관리 기준을 확인한다. |
| architecture | Developer Guide / Architecture | CLI, gateway, cron, ACP가 하나의 `AIAgent` core로 들어가는 구조를 기준으로 entry point와 내부 실행 흐름을 구분한다. |
| checkpoints/rollback | User Guide / Checkpoints and Rollback | 파일 시스템 변경 복구 기능으로 설명하고, 만능 백업처럼 쓰지 않는다. |

## 2. 이 책에서 특히 헷갈리기 쉬운 표현

| 표현 | 정합성 기준 |
|---|---|
| gateway | 문맥에 따라 `Messaging Gateway`인지 `Nous Tool Gateway`인지 밝혀야 한다. |
| AI 개인비서 | 공식 기능명이라기보다 Hermes Agent를 운영하는 활용 방식이다. |
| 역할형 에이전트 | 공식 기능 하나가 아니라 profile, SOUL.md, Skill, toolset, gateway 계정을 조합한 운영 구조다. |
| memory | 단순 파일 하나가 아니라 `MEMORY.md`, `USER.md`, session search, 외부 memory provider를 포함하는 기억 체계다. |
| profile | config/API key/memory/session/skill/cron/gateway state를 분리하지만, filesystem 권한을 막는 sandbox는 아니다. |
| Skill | “기억”이 아니라 반복 절차와 검증 순서를 담는 on-demand 문서다. |
| MCP | 모든 외부 연동을 뜻하지 않는다. MCP 서버로 노출된 도구를 Hermes가 발견하고 등록하는 방식이다. |
| cron | 자동 실행의 시작 조건이다. 작업 처리 방식은 prompt, Skill, toolset, workdir, delivery target에 따라 달라진다. |

## 3. 페이지를 고칠 때 보는 순서

1. 페이지가 설명하는 공식 기능을 하나로 정한다.
2. 공식 docs에서 기능명, 명령어, 설정 위치를 확인한다.
3. 책의 표현이 기능명과 운영 해석을 섞어 쓰고 있지 않은지 본다.
4. 독자가 그대로 따라 할 수 있는 최소 명령어와 확인 방법을 남긴다.
5. 버전이 바뀌기 쉬운 세부값은 “공식 docs에서 확인”하도록 연결한다.
6. 보안/권한/비밀값/공개 범위가 걸린 설명은 별도 체크리스트로 분리한다.

## 4. 정합성 점검 질문

| 질문 | 확인 기준 |
|---|---|
| 이 문장은 공식 기능명과 운영 해석을 구분하고 있는가 | 기능명은 영어 원문을 유지하고, 한국어 설명은 뒤에 붙인다. |
| 명령어가 최신 docs와 맞는가 | `hermes --help`, 공식 CLI reference, 해당 user guide를 함께 본다. |
| 설정 파일 경로가 맞는가 | 기본은 `~/.hermes/`, profile은 `~/.hermes/profiles/<name>/` 기준으로 쓴다. |
| 이 기능이 채널 연결인지 도구 API 라우팅인지 분명한가 | Messaging Gateway와 Nous Tool Gateway를 구분한다. |
| 자동화가 fresh session에서 실행되는가 | cron 작업은 현재 대화 맥락을 그대로 이어받지 않는다는 점을 적는다. |
| profile을 isolation처럼 과장하지 않았는가 | profile은 상태 분리이고 sandbox가 아니라는 문장을 넣는다. |
| 외부 도구 호출 결과를 검증하는가 | status, URL, 파일, 공개 페이지처럼 확인 가능한 증거를 남긴다. |
| 공개 예시에 secret이 섞이지 않았는가 | 실제 key/token/password/webhook URL은 쓰지 않는다. |

## 5. 책 전체에 적용할 표기 기준

| 상황 | 표기 |
|---|---|
| 공식 기능명 | `Persistent Memory`, `Skills System`, `MCP`, `Messaging Gateway`, `Nous Tool Gateway`, `Profiles`, `Scheduled Tasks (Cron)`처럼 원문을 병기한다. |
| 한국어 운영 설명 | 기능명 뒤에 “무엇을 해결하는 장치인가”를 붙인다. |
| 명령어 | 백틱으로 감싸고 임의 번역하지 않는다. |
| 설정 파일 | `config.yaml`, `.env`, `auth.json`, `SOUL.md`, `MEMORY.md`, `USER.md`처럼 정확한 파일명을 쓴다. |
| 위험 설정 | `--yolo`, dangerous command approval, allowlist, pairing, checkpoint/rollback은 보안 문맥에서만 다룬다. |
| 공식 docs 링크 | 기능 단위로 필요한 곳에만 넣고, 모든 문단마다 반복하지 않는다. |

## 6. Integrations를 책에 반영할 때의 기준

| 독자 상황 | 본문에서 설명할 것 | 부록/공식 docs로 보낼 것 |
|---|---|---|
| 처음 모델을 고르는 단계 | OAuth/API key/custom endpoint/local model의 선택 기준 | provider 전체 목록, env var 이름, 최신 지원 model |
| 외부 도구를 붙이는 단계 | MCP/API/CLI/gateway/cron 중 무엇을 먼저 쓸지 | MCP 서버별 설정값, platform별 세부 옵션 |
| 웹 리서치를 자동화하는 단계 | 무료 검색/공식 web backend/별도 검색 API를 구분하는 기준 | Firecrawl/Parallel/Tavily/Exa의 최신 설정, quota, 가격 |
| 팀이나 상시 운영으로 확장하는 단계 | 권한, 로그, delivery, fallback, 복구 기준 | provider routing 세부 옵션, platform별 제한, API reference |

책 본문은 “무엇을 선택해야 하는가”를 보여주고, 부록은 “공식 docs와 어디를 대조해야 하는가”를 알려준다. 빠르게 바뀌는 이름과 설정값을 본문에 길게 넣으면 입문서가 금방 낡는다.

## 7. Guides와 Tutorials를 반영할 때의 기준

공식 guides와 tutorials는 “명령어 전체를 책에 옮기는 자료”가 아니라, 입문자가 어떤 순서로 성공 경험을 만들어야 하는지 보여주는 예시다.

| 공식 가이드 유형 | 책에서 반영할 관점 | 본문에 과하게 넣지 않을 것 |
|---|---|---|
| Tips & Best Practices | 구체적인 요청, 앞부분 맥락 제공, context file, Skill, memory/Skill 구분, prompt cache, `/compress` 같은 사용 습관 | 모든 단축키와 CLI 조작법 |
| Daily Briefing Bot | cron fresh session, self-contained prompt, web search, delegation, delivery가 이어지는 자동화 패턴 | 예제 뉴스 주제와 명령어 전체 |
| Delegation & Parallel Work | 병렬 조사/검토에 delegation을 쓰고, 단순 도구 호출이나 오래 도는 작업은 다른 실행 방식을 쓰는 판단 기준 | 내부 tool schema 전체 |
| Working with Skills | 반복 절차를 Skill로 만들고, 설치/로드/검증 기준을 나누는 흐름 | Hub 목록 전체와 개별 skill 세부 설정 |
| Automation Templates | schedule/webhook/API trigger의 차이와 보고 억제(`[SILENT]`) 같은 운영 패턴 | 모든 template 예제 |

가이드 내용을 반영할 때는 “공식 예제와 같은 명령어를 따라 한다”보다 “왜 이 순서가 실패를 줄이는가”를 설명하는 편이 이 책의 역할에 맞다.

## 8. Architecture를 반영할 때의 기준

Architecture 문서는 개발자용 내부 지도다. 본문에 파일 구조와 dependency chain을 길게 옮기기보다, 입문자가 장애를 분리해서 볼 수 있는 수준으로만 반영한다.

| 공식 Architecture 항목 | 책에서 반영할 관점 | 본문에 과하게 넣지 않을 것 |
|---|---|---|
| Entry Points | CLI, Messaging Gateway, cron, ACP, API Server처럼 요청이 들어오는 입구가 다르다는 점 | 파일별 line 수, 내부 class 상세 |
| AIAgent core | prompt 구성, provider 선택, tool dispatch, compression, session 저장이 한 흐름으로 이어진다는 점 | agent loop 내부 구현 전체 |
| Tool System | tool registry와 toolset을 통해 필요한 도구만 노출한다는 점 | 모든 tool schema와 registry 코드 |
| Session Storage | session, compression, search가 운영 맥락을 이어주는 기반이라는 점 | SQLite schema 세부 구현 |
| Design Principles | prompt stability, observable execution, platform-agnostic core, profile isolation을 운영 원칙으로 해석 | contributor용 dependency chain 전체 |

책에서는 Architecture를 “개발자가 코드를 고치는 순서”보다 “운영자가 문제를 어디서부터 확인할지 나누는 지도”로 쓴다.

## 9. 공식 docs와 책의 역할 분담

공식 docs는 기능의 최신 기준이다. 설치 명령, 설정 키, 지원 플랫폼, provider, toolset, CLI reference는 공식 docs를 따른다.

이 책은 그 기능을 실제 업무 흐름으로 배치하는 운영 가이드다. “어떤 기능이 있다”에서 끝내지 않고, “내 업무에서 무엇을 기억하고, 무엇을 반복하고, 무엇을 도구와 채널로 연결할 것인가”를 정리한다.

따라서 책을 고칠 때는 공식 docs를 요약 복붙하기보다, 공식 기능명과 충돌하지 않는 선에서 독자의 판단 기준을 더 분명하게 만드는 방향이 좋다.
