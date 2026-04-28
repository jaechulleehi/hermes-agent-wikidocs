## 에르메스 에이전트(Hermes Agent) provider/model/config 설정은 어떻게 확인할까

![Hermes Agent provider model config 설정 흐름](../assets/images/chapter-heroes/ch00-5-provider-model-config-codex.webp)

Hermes Agent에서 provider/model/config는 “어떤 모델을 쓸지”만의 문제가 아니다. [공식 configuration 문서](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)는 설정을 `~/.hermes/config.yaml`, secret/token을 `~/.hermes/.env`로 나누고, provider, model, terminal backend, Docker/SSH/Modal/Daytona 같은 실행 환경 설정을 함께 다룬다. 이 경계가 흐리면 CLI에서는 되던 일이 gateway나 cron에서는 실패한다. 설정을 바꾼 뒤에는 [업데이트 전후 점검](https://wikidocs.net/346253)처럼 실행 검증까지 이어가야 한다.

처음 세팅할 때는 좋은 모델을 고르는 것보다 “어디에 무엇이 저장되는가”를 먼저 알아야 한다. 모델 이름, API key, OAuth, custom endpoint, provider routing, toolset, terminal backend가 섞이면 나중에 실패 원인을 찾기 어렵다.

## 공식 docs 기준 설정 위치

| 구분 | 공식 docs 기준 위치/흐름 | 운영 기준 |
|---|---|---|
| secret/token | `~/.hermes/.env` | API key, OAuth token처럼 노출되면 안 되는 값 |
| non-secret setting | `~/.hermes/config.yaml` | model, provider, toolset, display, backend 같은 설정 |
| config 확인 | `hermes config` | 현재 설정을 읽고 이상 여부를 본다 |
| config 수정 | `hermes config edit` 또는 `hermes config set KEY VAL` | 수동 편집보다 변경 의도를 남긴다 |
| update 후 점검 | `hermes config check`, `hermes config migrate` | 새 옵션 누락 여부를 확인한다 |

설정 파일은 운영 기억과 다르다. “사용자가 짧은 보고를 좋아한다” 같은 선호는 memory/user profile의 영역이고, “어떤 provider를 쓸 것인가”는 config의 영역이다.

## provider와 model을 확인하는 순서

1. 지금 쓸 provider를 하나 고른다.
2. 공식 docs나 `hermes model` 흐름으로 사용 가능한 model을 확인한다.
3. CLI에서 단일 질문으로 응답을 확인한다.
4. 같은 model/provider로 tool 호출이 되는지 확인한다.
5. gateway나 cron으로 확장하기 전에 config 저장 위치를 확인한다.

예시는 아래처럼 단순하게 시작한다.

```bash
hermes model
hermes chat --provider openrouter -q "짧게 자기소개해줘"
hermes chat --model "provider/model-name" -q "지금 설정이 어떤 의미인지 설명해줘"
```

모델명은 provider별로 달라질 수 있다. 그래서 책에 적힌 예시를 그대로 외우기보다, 현재 공식 docs와 자신의 provider 목록을 확인하는 것이 안전하다.

## provider routing은 언제 볼까

공식 docs에는 provider routing 기능이 있다. 비용, 속도, 사용 가능한 parameter, provider 순서 같은 기준으로 요청을 라우팅할 수 있다. 하지만 첫날부터 routing을 복잡하게 만들 필요는 없다.

| 단계 | 추천 방식 |
|---|---|
| 첫 설치 | provider 하나와 model 하나로 기본 chat을 확인한다 |
| 업무 사용 | 기본 model과 fallback model을 나눈다 |
| 비용/속도 최적화 | provider routing의 sort, only, ignore, order 같은 옵션을 검토한다 |
| 팀/상시 운영 | gateway와 cron이 같은 config를 읽는지 확인한다 |

routing은 최적화 도구다. 기본 대화와 도구 호출이 안정되기 전에 routing부터 복잡하게 만들면 실패 원인이 늘어난다.

## terminal backend 설정도 같이 본다

Hermes Agent는 terminal backend를 local, Docker, SSH 등으로 구성할 수 있다. 공식 configuration docs는 Docker backend와 SSH backend 설정, 환경변수 전달, sandbox 격리 기준을 함께 설명한다.

처음에는 local backend로 CLI를 확인하고, 위험한 명령이나 격리가 필요한 작업이 늘어나면 Docker/SSH를 검토한다. 특히 Docker에 credential을 넘길 때는 `docker_forward_env`에 적은 값이 container 안의 command에 보일 수 있다는 점을 주의해야 한다.

## config가 꼬였을 때 보는 순서

| 증상 | 먼저 볼 것 |
|---|---|
| 모델 응답이 없다 | provider/API key/OAuth/custom endpoint |
| CLI는 되는데 Slack은 안 된다 | gateway가 같은 config와 env를 읽는지 |
| 도구 호출이 안 된다 | toolset 설정과 platform별 tool 허용 범위 |
| 터미널 명령이 실패한다 | terminal backend, HOME/path/auth 차이 |
| 업데이트 후 이상하다 | `hermes config check`, `hermes config migrate`, update log |

## FAQ

### `.env`와 `config.yaml`은 어떻게 나누나요?

노출되면 안 되는 secret/token은 `.env`, 일반 설정은 `config.yaml`로 나누는 것이 기본이다. 공개 문서나 WikiDocs에는 실제 token 값을 절대 남기지 않는다.

### provider routing은 꼭 써야 하나요?

아니다. 처음에는 단일 provider/model로 시작하는 편이 안전하다. routing은 비용/속도/가용성을 최적화해야 할 때 도입한다.

### CLI에서 되는데 gateway에서 안 되면 모델 문제인가요?

반드시 그렇지는 않다. gateway process가 읽는 env/config, delivery target, platform permission, restart 여부를 함께 봐야 한다.

## 다음 글

provider/model/config 경계를 잡았다면, 기존 OpenClaw나 다른 에이전트 환경에서 넘어올 때 무엇이 다른지 확인한다. 다음은 [OpenClaw와 Hermes Agent의 차이](https://wikidocs.net/345889)다.
