## 에르메스 에이전트(Hermes Agent) 설치와 세팅은 어떻게 시작할까

에르메스 에이전트(Hermes Agent) 설치는 공식 docs 기준으로 짧다. Linux, macOS, WSL2, Android Termux에서는 one-line installer로 시작할 수 있고, 윈도우 사용자는 보통 WSL2 경로를 먼저 본다. 하지만 실제 업무 자동화에 쓰려면 설치 명령보다 설치 후 검증 순서가 더 중요하다. 설치가 끝난 뒤에는 [CLI 첫 대화](https://wikidocs.net/346251)와 provider/model/config 설정을 이어서 확인해야 한다.

![에르메스 에이전트 설치와 기본 검증 흐름](../assets/how-image-agent-creates-wikidocs-visuals/ch00-3-install-setup-flow-codex.webp)

처음 목표는 모든 기능을 켜는 것이 아니다. 먼저 `hermes` 명령이 잡히는지 확인하고, 기본 chat이 되는지 보고, provider/model 설정을 안정화한 뒤, CLI 세션, tool/toolset, Docker/Gateway, cron, Skill, MCP를 순서대로 붙여야 한다. 설치 단계에서 원인을 분리해두면 나중에 Slack, Telegram, Discord, cron, gateway에서 문제가 생겨도 복구가 쉬워진다.

## 공식 설치 명령

[공식 설치 문서](https://hermes-agent.nousresearch.com/docs/getting-started/installation)는 아래 명령을 안내한다.

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

공식 문서 기준 지원 흐름은 Linux, macOS, WSL2, Android Termux다. Windows native는 공식 문서에서 직접 지원 대상으로 보지 않고, WSL2 사용을 권장한다. Nix/NixOS와 수동 개발 설치 흐름은 공식 docs의 별도 페이지를 확인하는 편이 안전하다.

## 설치 전에 확인할 것

| 확인 항목 | 왜 필요한가 |
|---|---|
| 실행 환경 | Linux/macOS/WSL2/Termux/Nix 중 어떤 경로인지에 따라 설치 방법과 의존성이 달라진다 |
| Python/Node/git/ripgrep/ffmpeg 등 의존성 | installer가 일부를 처리하더라도 실패 시 원인을 좁히는 기준이 된다 |
| 사용할 provider | 설치 후 바로 대화하려면 모델/provider 연결이 필요하다 |
| 사용할 채널 | CLI만 쓸지, Slack/Telegram/Discord gateway까지 붙일지에 따라 다음 단계가 달라진다 |
| 권한/보안 기준 | 파일/터미널/브라우저 도구를 어디까지 허용할지 먼저 정해야 한다 |

설치 전 단계에서 모든 것을 완벽히 결정할 필요는 없다. 다만 “CLI 실습”인지 “상시 gateway 운영”인지가 다르면 검증 순서가 달라진다는 점은 알아야 한다.

## 설치 환경을 고르는 기준

OpenClaw 커뮤니티 대화에서 가장 많이 반복된 질문은 “어디에 설치해야 안전하고 오래 쓸 수 있는가”였다. Hermes Agent도 마찬가지다. 설치 명령보다 먼저 운영 환경을 정해야 한다.

| 환경 | 좋은 경우 | 조심할 점 |
|---|---|---|
| 개인 노트북/macOS | CLI 실습, 짧은 개인 업무, 파일 기반 실험 | 개인 로그인 정보와 업무 파일이 많다면 위험 명령 범위를 좁힌다 |
| WSL2 | Windows 사용자가 공식 지원 경로로 시작할 때 | Windows native와 WSL 경로/HOME이 섞이지 않게 한다 |
| Mac mini/맥미니/전용 머신 | Slack AI 비서, gateway, cron을 오래 켜둘 때 | 별도 계정, 별도 작업 폴더, launchd/절전/재시작 정책을 둔다 |
| VPS/클라우드 | 팀 채널, 상시 gateway, 외부 webhook 운영 | secret/env 전달, 방화벽, 비용 알림을 먼저 잡는다 |
| Docker | 위험한 terminal 작업이나 재현 가능한 실행 환경이 필요할 때 | container에 넘기는 credential을 최소화한다 |

초보자는 “내 컴퓨터에 설치할 수 있나”보다 “이 에이전트가 내 파일과 계정에 어디까지 접근해도 되는가”를 먼저 묻는 편이 안전하다.

## 설치보다 중요한 첫 검증 순서

| 순서 | 할 일 | 확인할 것 |
|---|---|---|
| 1 | 설치 스크립트 실행 | `hermes` 명령이 잡히는가 |
| 2 | shell reload | `source ~/.bashrc` 또는 `source ~/.zshrc` 후 실행되는가 |
| 3 | provider 선택 | `hermes model` 또는 config 흐름으로 사용할 모델/provider가 연결되는가 |
| 4 | 기본 chat 실행 | `hermes` 또는 `hermes chat -q "Hello"`가 응답하는가 |
| 5 | 세션 확인 | 대화가 저장되고 이어서 볼 수 있는가 |
| 6 | tool 범위 확인 | 파일/터미널/브라우저 등 필요한 도구만 켰는가 |
| 7 | gateway 필요 여부 판단 | Slack/Telegram/Discord 같은 메시징 연결이 필요한가 |
| 8 | 자동화 후보 분리 | cron으로 돌릴 일과 사람이 직접 시킬 일을 구분했는가 |

## 좋은 시작 방식

처음부터 dashboard, gateway, cron, MCP를 모두 붙이면 문제가 생겼을 때 원인을 찾기 어렵다. 좋은 시작 순서는 단순하다.

1. Hermes Agent CLI가 작동한다.
2. provider/model 설정이 안정적이다.
3. 기본 대화가 된다.
4. 작은 파일 읽기나 검색 같은 도구 호출을 확인한다.
5. Slack/Telegram 같은 messaging gateway를 붙인다.
6. 반복 업무 하나를 cron이나 Skill 후보로 분리한다.

이 순서가 중요한 이유는 Hermes Agent가 기능이 많은 도구라서다. 기능을 많이 켜는 것보다 “어느 단계까지 정상인지”를 나눠 확인해야 복구가 쉽다.

## 설치할 때 자주 생기는 오해

### 설치가 끝나면 바로 업무 자동화가 되나요?

아니다. 설치는 실행 환경을 만든 것뿐이다. AI 업무 자동화는 요청 창구, provider, 도구 권한, 기록 위치, 검증 기준이 정해져야 시작된다.

### API key만 넣으면 되나요?

provider에 따라 API key, OAuth, custom endpoint, model name, context length 설정이 달라질 수 있다. 공식 Quickstart도 설치 다음 단계로 provider 선택과 기본 chat 검증을 강조한다.

### gateway부터 켜도 되나요?

가능하지만 추천 순서는 아니다. CLI 기본 chat이 안정된 뒤 gateway를 붙여야 메시징 문제인지 모델/provider 문제인지 구분할 수 있다. 특히 [맥미니 같은 전용 머신에 상시 운영](https://wikidocs.net/346395)할 때는 gateway만이 아니라 launchd/LaunchAgent, 절전, 로그 확인 순서까지 함께 잡아야 한다.

### Docker로 바로 시작해도 되나요?

가능하다. 다만 Docker는 데이터 디렉터리, gateway health, port, restart policy까지 같이 봐야 한다. 단순 실습은 CLI 설치가 쉽고, 상시 운영은 Docker/Gateway 장에서 따로 판단하는 편이 좋다.

## 다음 글

설치와 기본 세팅을 끝냈다면 바로 자동화로 넘어가지 말고 다음 단계인 [에르메스 에이전트(Hermes Agent) CLI 첫 대화](https://wikidocs.net/346251)를 확인한다. CLI에서 첫 대화와 세션이 안정돼야 provider, config, gateway 문제를 분리할 수 있다.
