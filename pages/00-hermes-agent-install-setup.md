## 에르메스 에이전트(Hermes Agent) 설치와 세팅은 어떻게 시작할까

에르메스 에이전트(Hermes Agent) 설치는 공식 문서 기준으로 매우 짧다. `Hermes Agent 설치`, `에르메스 에이전트 설치`, `Hermes Agent 설치 방법`을 찾는 독자라면 Linux, macOS, WSL2, Android Termux에서 one-line installer로 시작할 수 있다. 다만 실제 업무 자동화에 쓰려면 설치 명령보다 설치 후 검증 순서가 더 중요하다.

처음 목표는 “모든 기능을 켜기”가 아니다. 먼저 기본 chat이 정상 작동하는지 확인하고, provider 설정을 안정화하고, 그다음 [Docker/Gateway](https://wikidocs.net/346139), [cron](https://wikidocs.net/345926), [skill](https://wikidocs.net/345904), [MCP](https://wikidocs.net/345903) 같은 기능을 하나씩 붙여야 한다.

## 공식 설치 명령

[공식 설치 문서](https://hermes-agent.nousresearch.com/docs/getting-started/installation)는 아래 명령을 안내한다.

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

공식 문서 기준 지원 흐름은 Linux, macOS, WSL2, Android Termux다. Windows native는 공식 문서에서 직접 지원 대상으로 보지 않고, WSL2 사용을 권장한다.

## 설치보다 중요한 첫 검증 순서

| 순서 | 할 일 | 확인할 것 |
|---|---|---|
| 1 | 설치 스크립트 실행 | `hermes` 명령이 잡히는가 |
| 2 | shell reload | `source ~/.bashrc` 또는 `source ~/.zshrc` 후 실행되는가 |
| 3 | provider 선택 | `hermes model`로 사용할 모델/provider가 연결되는가 |
| 4 | 기본 chat 실행 | 평범한 질문에 응답하는가 |
| 5 | tool 범위 확인 | 파일/터미널/브라우저 등 필요한 도구만 켰는가 |
| 6 | gateway 필요 여부 판단 | Slack/Telegram/Discord 같은 메시징 연결이 필요한가 |
| 7 | 자동화 후보 분리 | cron으로 돌릴 일과 사람이 직접 시킬 일을 구분했는가 |

## 좋은 시작 방식

처음부터 dashboard, gateway, cron, MCP를 모두 붙이면 문제가 생겼을 때 원인을 찾기 어렵다. 좋은 시작 순서는 단순하다.

1. Hermes Agent CLI가 작동한다.
2. provider/model 설정이 안정적이다.
3. 기본 대화가 된다.
4. 작은 파일 읽기나 검색 같은 도구 호출을 확인한다.
5. Slack/Telegram 같은 메시징 gateway를 붙인다.
6. 반복 업무 하나를 cron이나 skill 후보로 분리한다.

이 순서가 중요한 이유는 Hermes Agent가 기능이 많은 도구라서다. 기능을 많이 켜는 것보다 “어느 단계까지 정상인지”를 나눠 확인해야 복구가 쉽다.

## 설치할 때 자주 생기는 오해

### 설치가 끝나면 바로 업무 자동화가 되나요?

아니다. 설치는 실행 환경을 만든 것뿐이다. AI 업무 자동화는 요청 창구, provider, 도구 권한, 기록 위치, 검증 기준이 정해져야 시작된다.

### API key만 넣으면 되나요?

provider에 따라 API key, OAuth, custom endpoint, model name, context length 설정이 달라질 수 있다. 공식 Quickstart도 설치 다음 단계로 provider 선택과 기본 chat 검증을 강조한다.

### gateway부터 켜도 되나요?

가능하지만 추천 순서는 아니다. CLI 기본 chat이 안정된 뒤 gateway를 붙여야 메시징 문제인지 모델/provider 문제인지 구분할 수 있다.

### Docker로 바로 시작해도 되나요?

가능하다. 다만 Docker는 데이터 디렉터리, gateway health, port, restart policy까지 같이 봐야 한다. 단순 실습은 CLI 설치가 쉽고, 상시 운영은 Docker/Gateway 장에서 따로 판단하는 편이 좋다.

## 다음 글

설치와 기본 세팅을 이해했다면, 다음에는 [OpenClaw와 Hermes Agent가 무엇이 다른지](https://wikidocs.net/345889) 본다. 이미 OpenClaw나 유사한 에이전트 환경을 써봤다면 전환 기준을 먼저 잡아야 한다.
