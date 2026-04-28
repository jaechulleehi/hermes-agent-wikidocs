## Hermes Agent와 Claude Code/Codex는 어떻게 다를까

Hermes Agent와 Claude Code/Codex는 모두 AI 에이전트처럼 보이지만 쓰임이 다르다. Claude Code와 Codex는 주로 코드 작성, 리팩터링, PR 리뷰 같은 코딩 작업에 강한 CLI 에이전트이고, 에르메스 에이전트(Hermes Agent)는 memory, skills, cron, MCP, gateway, messaging, delegation을 묶어 장기 업무 자동화 흐름을 운영하는 환경에 가깝다.

따라서 “무엇이 더 좋나”보다 “어떤 일을 맡길 것인가”로 비교해야 한다. 이 비교는 [에르메스 에이전트(Hermes Agent)의 핵심 개념](https://wikidocs.net/346055)을 먼저 잡고 읽으면 더 선명하다. 코드를 깊게 고치거나 개발 작업을 위임할 때는 Claude Code/Codex가 강하고, 여러 도구와 역할형 에이전트를 묶어 Slack 요청, 정기 조사, WikiDocs 발행, 기억 관리까지 이어야 할 때는 Hermes Agent의 운영 구조가 중요해진다.

## 한눈에 보는 비교

| 도구 | 주 용도 | 강점 | Hermes Agent와의 관계 |
|---|---|---|---|
| Claude Code | 코드 작성/리팩터링/PR 작업 | Anthropic Claude 기반의 코딩 CLI, 긴 코드 작업과 리뷰 | Hermes가 필요할 때 외부 코딩 에이전트로 위임할 수 있다 |
| Codex | 코드 수정/자동 구현/리뷰 | OpenAI 계열 코딩 에이전트, git repo 기반 작업 | Hermes의 autonomous-ai-agents skill 흐름에서 호출 후보가 된다 |
| Hermes Agent | AI 개인비서/업무 자동화/메모리/스킬/메시징/cron | 여러 도구와 역할을 묶는 운영 시스템 | Claude Code/Codex 같은 코딩 에이전트까지 포함해 작업을 조율한다 |

## Claude Code/Codex를 먼저 쓰면 좋은 경우

- 코드베이스 안에서 버그를 고쳐야 한다.
- 파일을 읽고 수정하고 테스트를 돌리는 개발 작업이 중심이다.
- PR 리뷰나 리팩터링처럼 코드 품질 판단이 중요하다.
- 작업 범위가 하나의 git repository 안에 비교적 분명하다.

이 경우에는 Claude Code나 Codex 같은 코딩 에이전트가 바로 맞을 수 있다. 특히 코드 수정 자체가 목표라면 Hermes Agent를 앞에 둘 필요 없이 코딩 에이전트에 직접 맡기는 편이 빠르다.

## Hermes Agent를 먼저 쓰면 좋은 경우

- Slack이나 Telegram 같은 메시징 창구에서 요청을 받고 싶다.
- 조사/정리/실행/이미지 제작처럼 역할을 나눠야 한다.
- 결과를 memory, skill, Obsidian, WikiDocs, GitHub 같은 여러 저장소에 나눠 남겨야 한다.
- 매일 정해진 시간에 cron으로 조사나 보고를 돌리고 싶다.
- Google Workspace, MCP, gateway, 외부 API를 업무 흐름에 붙이고 싶다.
- 코딩 작업뿐 아니라 콘텐츠, 문서화, 리서치, 운영 체크리스트까지 이어야 한다.

이 경우 Hermes Agent는 단일 코딩 도구보다 “메인 창구와 오케스트레이터”에 가깝다. 사용자는 Hermes에게 요청하고, Hermes는 필요하면 코딩 에이전트, 조사형 에이전트, 정리형 에이전트, 실행형 에이전트를 나눠 쓰는 구조를 만들 수 있다.

## 실제 운영에서는 함께 쓴다

실제 업무에서는 둘 중 하나만 고르는 문제가 아니다. 예를 들어 “GitHub 저장소의 문서 구조를 고치고 WikiDocs에 반영해줘”라는 요청은 여러 층으로 나뉜다.

1. Hermes Agent가 요청 의도와 공개 범위를 정리한다.
2. 조사형 에이전트가 현재 문서와 공식 docs를 확인한다.
3. 정리형 에이전트가 독자용 구조를 다시 쓴다.
4. 실행형 에이전트가 파일 수정, 검증, commit/push를 맡는다.
5. 코드 변경이나 복잡한 리팩터링이 필요하면 Claude Code/Codex에 위임한다.
6. 결과는 WikiDocs나 GitHub 기록으로 남는다.

이렇게 보면 Hermes Agent는 Claude Code/Codex의 경쟁자라기보다, 어떤 일을 어떤 실행 방식에 맡길지 결정하는 상위 운영 흐름에 가깝다.

## 비교할 때의 기준

1. 작업의 중심이 코드인가, 업무 흐름인가?
2. 결과가 한 번의 수정으로 끝나는가, 반복 운영으로 남아야 하는가?
3. 메시징/gateway/cron 같은 상시 운영이 필요한가?
4. memory와 skill로 다음 작업에 재사용해야 하는가?
5. 여러 역할형 에이전트를 조율할 필요가 있는가?

이 질문에서 “코드”가 중심이면 Claude Code/Codex를 먼저 보고, “업무 흐름”이 중심이면 Hermes Agent를 먼저 보면 된다.

## FAQ

### Hermes Agent가 Claude Code를 대체하나요?

완전히 대체한다고 보기 어렵다. Claude Code는 코딩 작업에 특화된 CLI 에이전트이고, Hermes Agent는 코딩을 포함한 여러 업무 흐름을 조율하는 운영 환경에 가깝다.

### Codex와 Hermes Agent를 같이 쓸 수 있나요?

가능하다. Hermes Agent의 skill과 delegation 흐름에서 Codex 같은 코딩 에이전트를 특정 작업 실행자로 볼 수 있다. 다만 인증, git repository, 권한, sandbox 기준은 별도로 확인해야 한다.

### 비개발자도 Hermes Agent를 쓸 이유가 있나요?

있다. Hermes Agent의 핵심은 코딩만이 아니라 AI 개인비서, 업무 자동화, 정기 보고, 문서화, 기억 관리, 메시징 연동이다. 개발자가 아니어도 반복 업무와 기록 흐름이 있다면 쓸 이유가 생긴다.

## 다음 글

다음에는 [Docker와 Gateway를 언제 써야 하는지](https://wikidocs.net/346139) 본다. Hermes Agent를 개인 CLI로만 쓸지, 항상 켜진 AI 비서로 운영할지에 따라 구조가 달라진다.
