## 에르메스 에이전트(Hermes Agent) CLI 첫 대화는 어떻게 시작할까

Hermes Agent CLI 첫 대화는 설치가 끝났는지 확인하는 절차가 아니다. 공식 Quickstart는 install, provider 선택, working chat 검증, session 확인, key features 체험 순서로 안내한다. 이 흐름을 따라야 나중에 Slack/gateway/cron/MCP에서 문제가 생겼을 때 “모델이 안 되는지, 설정이 안 되는지, 메시징 연결이 안 되는지”를 분리할 수 있다.

CLI는 Hermes Agent의 기본 조종석이다. [공식 CLI 문서](https://hermes-agent.nousresearch.com/docs/user-guide/cli)는 Hermes Agent CLI를 web UI가 아니라 multiline editing, slash-command autocomplete, conversation history, interrupt/redirect, streaming tool output을 갖춘 terminal UI로 설명한다. 그래서 첫 대화에서는 답변 품질보다 실행/중단/세션/명령 흐름을 확인해야 한다.

## 1단계. 대화가 되는지 확인한다

가장 단순한 확인은 interactive session을 여는 것이다.

```bash
hermes
```

비대화형으로 빠르게 확인하려면 single query mode를 쓴다.

```bash
hermes chat -q "Hello"
```

여기서 확인할 것은 멋진 답변이 아니다. 아래 세 가지다.

| 확인 항목 | 의미 |
|---|---|
| 명령 실행 | `hermes` 명령이 PATH에 잡혀 있다 |
| provider 응답 | 선택한 모델/provider가 실제로 응답한다 |
| 로그/오류 | 실패했을 때 config, API key, provider 중 어디가 문제인지 보인다 |

## 2단계. provider와 model을 지정해본다

공식 CLI 문서는 특정 model/provider를 지정하는 흐름을 제공한다.

```bash
hermes chat --model "anthropic/claude-sonnet-4"
hermes chat --provider openrouter
```

모델 이름은 환경마다 달라질 수 있으니 공식 docs와 현재 `hermes model` 흐름을 확인해야 한다. 이 책에서 중요한 것은 특정 모델명이 아니라 “모델을 바꿔도 같은 업무 흐름이 유지되는가”다.

## 3단계. 필요한 toolset만 켜본다

Hermes Agent는 tool과 toolset을 통해 파일, 터미널, 웹, 브라우저, 메시징 같은 기능을 쓴다. 첫날부터 모든 도구를 켜는 것은 좋은 시작이 아니다. 작은 범위부터 확인한다.

```bash
hermes chat --toolsets "web,terminal" -q "현재 프로젝트에서 README가 있는지 확인해줘"
```

도구를 켤 때는 세 가지를 같이 본다.

| 질문 | 이유 |
|---|---|
| 이 도구가 지금 필요한가 | 불필요한 권한을 줄이기 위해 |
| 실행 결과를 어떻게 검증할까 | 도구 호출이 곧 정답은 아니기 때문에 |
| 위험 작업은 어디서 멈출까 | 삭제/외부 공개/권한 변경을 막기 위해 |

## 4단계. slash command를 확인한다

[공식 slash command reference](https://hermes-agent.nousresearch.com/docs/reference/slash-commands)는 CLI 전용 command와 messaging 전용 command를 나눠 설명한다. CLI에서는 `/tools`, `/toolsets`, `/config`, `/cron`, `/skills`, `/platforms`, `/statusbar`, `/plugins` 같은 명령을 확인할 수 있다.

처음에는 아래 흐름만 확인해도 충분하다.

```text
/tools       현재 사용할 수 있는 도구 확인
/toolsets    도구 묶음 확인
/config      설정 상태 확인
/skills      로드 가능한 Skill 확인
```

중요한 것은 slash command를 많이 외우는 것이 아니다. “지금 문제가 provider인지, tool인지, skill인지, gateway인지”를 CLI 안에서 확인할 수 있어야 한다.

## 5단계. session이 이어지는지 본다

Hermes Agent는 한 번의 질문으로 끝나는 챗봇보다 오래 가는 작업 흐름에 맞다. 그래서 첫 대화 후에는 세션이 저장되고 다시 이어질 수 있는지 확인해야 한다. 공식 docs의 session, memory, context 관련 문서는 이후 장에서 더 자세히 다룬다.

운영 기준은 단순하다.

| 상태 | 해석 |
|---|---|
| 첫 대화가 된다 | provider/model 기본 연결은 정상이다 |
| 같은 세션에서 맥락이 이어진다 | CLI 대화 흐름은 정상이다 |
| 새 세션에서 필요한 기억만 남는다 | memory/profile/skill 경계를 설계할 준비가 됐다 |
| 과거 세션을 찾을 수 있다 | 작업 회수와 handoff가 가능해진다 |

## 첫 대화 체크리스트

- `hermes` interactive session이 열린다.
- `hermes chat -q "Hello"`가 응답한다.
- provider/model을 바꿔 실행해본다.
- 필요한 toolset 하나만 켜서 작은 작업을 시켜본다.
- `/tools`, `/toolsets`, `/config`, `/skills`를 확인한다.
- 실패했을 때 오류가 provider/config/tool/gateway 중 어디에 가까운지 분리한다.

## FAQ

### CLI를 건너뛰고 Slack부터 써도 되나요?

가능하지만 추천하지 않는다. CLI 기본 대화가 안정돼야 Slack 문제가 gateway 문제인지 provider 문제인지 구분할 수 있다.

### slash command를 모두 외워야 하나요?

아니다. 처음에는 `/tools`, `/toolsets`, `/config`, `/skills` 정도만 확인해도 충분하다. 나머지는 운영 범위가 커질 때 공식 reference를 보면 된다.

### 첫 대화에서 도구 호출까지 확인해야 하나요?

가능하면 작은 도구 호출 하나는 확인하는 편이 좋다. Hermes Agent는 답변만 하는 도구가 아니라 실제 작업을 수행하는 에이전트이기 때문이다.

## 다음 글

CLI 첫 대화가 안정됐다면 다음에는 provider/model/config 확인으로 넘어간다. 모델과 설정 저장 위치가 정리되어야 이후 gateway와 cron이 흔들리지 않는다.
