## 에르메스 에이전트(Hermes Agent) provider/model/config 설정은 어떻게 확인할까

Hermes Agent에서 provider/model/config는 “어떤 모델을 쓸지”만의 문제가 아니다. [공식 configuration 문서](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)는 설정을 `~/.hermes/config.yaml`, secret/token을 `~/.hermes/.env`로 나누고, provider, model, 실행 환경 설정을 함께 다룬다. 이 경계가 흐리면 CLI에서는 되던 일이 gateway나 cron에서는 실패할 수 있다.

![Hermes Agent provider model config 설정 흐름](../assets/images/chapter-heroes/ch00-5-provider-model-config-codex.webp)

이 페이지는 provider 전체 목록을 외우게 하는 문서가 아니다. 입문자가 먼저 볼 것은 “내가 어떤 방식으로 Hermes Agent를 연결하고, 그 선택이 비용/권한/복구에 어떤 영향을 주는가”다. 최신 provider 목록, env var 이름, 세부 설정값은 [Hermes Agent 공식 문서](https://hermes-agent.nousresearch.com/docs/integrations/providers)와 [공식 문서 정합성 점검표](https://wikidocs.net/346587)에서 다시 확인한다.

## 공식 문서 기준 설정 위치

| 구분 | 위치/흐름 | 실제 확인 포인트 |
|---|---|---|
| secret/token | `~/.hermes/.env` | API key, OAuth token처럼 노출되면 안 되는 값 |
| 일반 설정 | `~/.hermes/config.yaml` | model, provider, toolset, display, backend 같은 설정 |
| config 확인 | `hermes config` | 현재 설정을 읽고 이상 여부를 본다 |
| config 수정 | `hermes config edit` 또는 `hermes config set KEY VAL` | 수동 편집보다 변경 의도를 남긴다 |
| update 후 점검 | `hermes config check`, `hermes config migrate` | 새 옵션 누락 여부를 확인한다 |

설정 파일은 운영 기억과 다르다. “사용자가 짧은 보고를 좋아한다” 같은 선호는 memory/user profile의 영역이고, “어떤 provider를 쓸 것인가”는 config의 영역이다.

## provider와 model을 확인하는 순서

1. 지금 쓸 provider를 하나 고른다.
2. 공식 문서나 `hermes model` 흐름으로 사용 가능한 model을 확인한다.
3. CLI에서 단일 질문으로 응답을 확인한다.
4. 같은 model/provider로 tool 호출이 되는지 확인한다.
5. gateway나 cron으로 확장하기 전에 config 저장 위치를 확인한다.

예시는 아래처럼 단순하게 시작한다.

```bash
hermes model
hermes chat --provider openrouter -q "짧게 자기소개해줘"
hermes chat --model "provider/model-name" -q "지금 설정이 어떤 의미인지 설명해줘"
```

모델명은 provider별로 달라질 수 있다. 책에 적힌 예시를 그대로 외우기보다 현재 공식 문서와 자신의 provider 목록을 확인하는 것이 안전하다.

## provider/model 선택 기준

| 상황 | 먼저 검토할 방식 | 확인할 것 |
|---|---|---|
| 개인이 빠르게 시작 | OAuth/구독 기반 provider | 세션 만료, third-party 사용 정책, 개인 계정 한도 |
| 서버/gateway/cron 운영 | API key 기반 provider | `.env` 보관, 과금 owner, 월 한도, billing alert |
| 여러 모델을 한 인터페이스로 묶기 | OpenRouter, custom endpoint, proxy | 요청 추적, 로그 보관, fallback 기준 |
| 회사 데이터 반출 제한 | local/self-hosted endpoint | GPU/서버 비용, 속도, 모델 품질, 접근 권한 |
| 비용/장애 대응이 중요 | routing/fallback | 어느 요청이 어느 provider로 갔는지 확인할 로그 |

처음에는 하나의 provider/model로 CLI 응답과 도구 호출을 확인한다. 그다음 gateway, cron, routing, local/self-hosted로 넓히는 편이 실패 원인을 줄인다.

## routing은 언제 볼까

공식 문서에는 provider routing 기능이 있다. 비용, 속도, 사용 가능한 parameter, provider 순서 같은 기준으로 요청을 라우팅할 수 있다. 하지만 첫날부터 routing을 복잡하게 만들 필요는 없다.

| 단계 | 추천 방식 |
|---|---|
| 첫 설치 | provider 하나와 model 하나로 기본 chat을 확인한다 |
| 업무 사용 | 기본 model과 fallback model을 나눈다 |
| 비용/속도 조정 | routing의 sort, only, ignore, order 같은 옵션을 검토한다 |
| 팀/상시 운영 | gateway와 cron이 같은 config를 읽는지 확인한다 |

routing은 조정 도구다. 기본 대화와 도구 호출이 안정되기 전에 routing부터 복잡하게 만들면 실패 원인이 늘어난다.

## 비용은 모델값만 보면 안 된다

입문자가 자주 헷갈리는 지점은 provider 설정을 “로그인 방식”으로만 보는 것이다. 실제 운영에서는 인증 방식이 곧 구독/API 비용, 토큰, 한도, 과금, 복구 기준이 된다.

| 비용 항목 | 어디서 생기나 | 확인 포인트 |
|---|---|---|
| 구독/OAuth | 개인 계정 기반 provider 연결 | 세션 만료, 정책 변경, third-party 허용 여부 확인 |
| API 사용량 | provider API key, aggregator, 검색 API | 월 한도, 알림, key scope, 과금 owner를 정한다 |
| 검색/크롤링 비용 | web backend와 별도 검색 API | 공식 지원 범위, 무료 검색으로 충분한지, 대량 호출이 필요한지 나눈다 |
| 인프라 비용 | Mac mini, VPS, Docker, GPU, storage | 상시 gateway/cron 운영 시간과 로그 보관 기준을 둔다 |
| 복구 비용 | 장애 대응, token rotation, 설정 재검증 | 비용보다 먼저 원인 분리와 rollback 경로를 남긴다 |

개인 실험은 OAuth/구독으로 빠르게 시작할 수 있지만, 팀 운영이나 상시 자동화는 API key, quota, fallback, billing alert를 분리해서 설계하는 편이 안전하다.

## 실행 환경 설정도 같이 본다

Hermes Agent는 terminal backend를 local, Docker, SSH 등으로 구성할 수 있다. 처음에는 local backend로 CLI를 확인하고, 위험한 명령이나 격리가 필요한 작업이 늘어나면 Docker/SSH를 검토한다. 특히 Docker에 credential을 넘길 때는 `docker_forward_env`에 적은 값이 container 안의 command에 보일 수 있다는 점을 주의해야 한다.

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

노출되면 안 되는 secret/token은 `.env`, 일반 설정은 `config.yaml`로 나누는 것이 기본이다. WikiDocs나 팀 문서에는 실제 token 값을 절대 남기지 않는다.

### provider routing은 꼭 써야 하나요?

아니다. 처음에는 단일 provider/model로 시작하는 편이 안전하다. routing은 비용/속도/가용성을 세밀하게 조정해야 할 때 도입한다.
