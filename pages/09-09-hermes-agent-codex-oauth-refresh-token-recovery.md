## Hermes Agent Codex OAuth 토큰 충돌은 어떻게 복구할까

Hermes Agent를 Slack이나 gateway로 운영하다가 갑자기 답변 품질이 달라지거나 다른 provider로 우회되는 일이 생기면, 먼저 “Slack 연결이 끊겼다”고 생각하기 쉽다. 하지만 실제 원인은 messaging gateway가 아니라 메인 모델 provider 인증일 수 있다. 특히 `openai-codex`를 Hermes Agent와 Codex CLI에서 함께 쓰는 운영 환경이라면 Codex OAuth refresh token 충돌을 먼저 확인해야 한다.

이 문제는 OpenAI 계정 하나를 여러 곳에서 쓰는 것 자체가 문제라는 뜻이 아니다. 문제는 여러 실행 주체가 같은 refresh token을 복사해서 나눠 가졌을 때 생긴다. 각 Hermes profile과 Codex CLI가 서로 다른 device auth credential을 가지고 있으면, 같은 계정이어도 독립 세션으로 운영할 수 있다.

[TOC]

## 먼저 증상을 나눠 본다

운영 중 이런 로그가 보이면 Slack/gateway 장애와 provider 인증 장애를 분리해서 봐야 한다.

```text
Primary provider auth failed:
Codex refresh token was already consumed by another client
Fallback provider resolved: fallback-provider-name
```

이때 의미는 대략 이렇다.

| 항목 | 상태 해석 |
|---|---|
| Slack gateway | 메시지는 받고 있을 수 있다 |
| Hermes gateway process | 살아 있을 수 있다 |
| 메인 provider | `openai-codex` 인증이 실패했다 |
| fallback provider | 보조 provider로 우회 응답 중일 수 있다 |
| 실제 원인 후보 | Codex OAuth refresh token이 다른 client에서 먼저 소비됐다 |

즉 “에이전트가 죽었다”가 아니라 “메인 모델로 들어가는 출입증 갱신이 실패했다”에 가깝다. 그래서 gateway 재설치부터 하면 원인을 놓치기 쉽다. 먼저 provider 인증 상태를 확인해야 한다.

## refresh token이 이미 소비됐다는 말

OAuth credential에는 보통 두 가지 성격의 토큰이 있다.

| 구분 | 비유 | 역할 |
|---|---|---|
| access token | 잠깐 쓰는 출입증 | 실제 API 호출에 사용된다 |
| refresh token | 새 출입증을 받는 교환권 | access token이 만료됐을 때 새 access token을 받는다 |

Codex OAuth의 refresh token은 회전될 수 있다. 한 client가 refresh token으로 새 토큰을 발급받으면, 기존 refresh token은 더 이상 쓸 수 없게 되는 구조다. 그런데 Hermes profile 여러 개나 Codex CLI가 같은 refresh token을 복사해서 들고 있으면 문제가 생긴다.

예를 들어 `harvey`, `bangwooli`, `ppodongi`, Codex CLI가 같은 refresh token을 공유하고 있었다고 하자. 그중 하나가 먼저 갱신에 성공하면 나머지가 들고 있던 같은 refresh token은 “이미 사용된 교환권”이 된다. 그다음 다른 profile이 메인 provider를 호출하는 순간 `Codex refresh token was already consumed by another client` 오류가 난다.

중요한 점은 “같은 OpenAI 계정”이 문제가 아니라 “같은 refresh token 공유”가 문제라는 것이다.

## 안전한 운영 구조

정석 구조는 OpenAI 계정 하나를 쓰더라도 실행 주체마다 별도 device auth session을 갖는 것이다.

```text
OpenAI 계정 1개
├─ Codex CLI 세션
├─ harvey Hermes profile 세션
├─ bangwooli Hermes profile 세션
├─ ppodongi Hermes profile 세션
└─ 그 외 역할형 에이전트 profile 세션
```

각 줄이 새 device auth로 발급받은 서로 다른 refresh token이면 괜찮다. 한 profile이 refresh해도 다른 profile의 refresh token을 직접 소비하지 않는다.

반대로 피해야 할 구조는 이렇다.

```text
하나의 Codex credential 파일
├─ Codex CLI에서 사용
├─ harvey profile에 복사
├─ bangwooli profile에 복사
└─ ppodongi profile에 복사
```

이 구조에서는 지금 당장 로그인 상태처럼 보이더라도, 어느 한쪽이 갱신하는 순간 다른 profile이 줄줄이 깨질 수 있다.

## 복구 순서

복구는 “다시 로그인”보다 “어느 credential이 누구 것인지 분리”가 먼저다.

```text
1. 현재 증상을 보존한다.
2. gateway가 살아 있는지와 provider 인증이 실패했는지를 나눠 본다.
3. 각 Hermes profile의 openai-codex 인증 상태를 확인한다.
4. Codex CLI credential과 Hermes profile credential이 같은 refresh token을 공유하는지 지문으로 비교한다.
5. 깨진 profile은 새 device auth로 다시 로그인한다.
6. 기존 Codex CLI credential을 import하거나 auth 파일을 복사하지 않는다.
7. 낡은 공유 credential은 제거한다.
8. 각 profile에서 직접 Codex 호출 테스트를 한다.
9. 필요한 gateway process를 재시작한다.
10. refresh/access token 지문 중복이 없는지 다시 확인한다.
```

공개 문서나 팀 공유 기록에는 실제 token 값을 절대 남기지 않는다. 비교가 필요하면 token 원문이 아니라 해시 지문 일부만 사용한다. 지문도 운영 판단용으로만 쓰고, 외부 문서에는 “중복 없음/중복 있음” 정도로 정리하는 편이 안전하다.

## 명령 예시

환경마다 Hermes 설치 경로와 profile 이름은 다를 수 있다. 아래는 흐름을 보여주는 예시다.

```bash
codex login status

hermes --profile harvey auth add openai-codex
hermes --profile bangwooli auth add openai-codex
hermes --profile ppodongi auth add openai-codex

hermes --profile harvey gateway run --replace
```

프로젝트 소스에서 직접 실행해야 하는 설치라면 `python -m hermes_cli.main --profile PROFILE_NAME auth add openai-codex`처럼 해당 환경의 Hermes CLI 진입점을 사용하면 된다. 중요한 것은 명령 모양보다 profile마다 새 device auth를 따로 진행한다는 점이다.

로그인 과정에서 Codex CLI credential을 가져오거나 기존 credential 파일을 복사하는 선택지가 보이면 주의해야 한다. 멀티 profile 운영에서는 편해 보여도 나중에 refresh token 충돌로 돌아올 수 있다.

## 점검표

| 질문 | 안전한 답 |
|---|---|
| Codex CLI와 Hermes profile이 같은 credential 파일을 쓰는가 | 아니오 |
| Hermes profile끼리 auth 파일을 복사했는가 | 아니오 |
| 각 profile에서 `auth add openai-codex`를 따로 진행했는가 | 예 |
| 깨진 credential을 제거했는가 | 예 |
| 각 profile에서 직접 provider 호출 테스트를 했는가 | 예 |
| gateway 재시작 후 Slack/Telegram/Discord 같은 채널 응답을 확인했는가 | 예 |
| token 원문이 로그/문서/Slack에 남지 않았는가 | 예 |

이 점검표에서 하나라도 흔들리면 “로그인은 되어 보이는데 다음 refresh 때 또 깨지는” 상태일 수 있다.

## 실제 운영에서 얻은 기준

우리 운영에서는 Hermes Agent를 Slack gateway로 띄워 두고, 동시에 Codex CLI는 코드 작업용으로 사용했다. 이 구조 자체는 자연스럽다. 하비는 메인 창구로 요청을 받고, 방울이/뽀동이/봉구/비벙이/하망이 같은 역할형 에이전트는 profile 단위로 분리되어 움직인다. Codex CLI는 별도로 코드 작업을 맡는다.

문제는 일부 profile이 같은 Codex refresh token을 공유하고 있었던 점이었다. 그래서 한 profile이나 CLI 쪽에서 token refresh가 일어나자 다른 profile의 credential이 `already consumed` 상태가 됐다. 복구 후에는 Hermes profile들과 Codex CLI가 모두 서로 다른 refresh token을 갖도록 재인증했고, 중복 지문이 없음을 확인했다.

이 사례의 교훈은 단순하다.

- profile은 성격만 나누는 것이 아니라 credential 경계도 나눠야 한다.
- Codex CLI와 Hermes Agent는 같은 계정을 써도 되지만 같은 refresh token을 공유하면 안 된다.
- gateway 장애처럼 보여도 먼저 provider 인증과 fallback 여부를 확인해야 한다.
- 복구 후에는 “로그인됨”만 보지 말고 profile별 직접 호출과 token 중복 여부를 검증해야 한다.

## FAQ

### OpenAI 계정 하나로 여러 Hermes profile을 써도 되나요?

가능하다. 다만 각 profile에서 새 device auth를 따로 진행해야 한다. 같은 계정의 사용량/한도는 공유될 수 있지만, refresh token은 profile별로 독립된 세션이어야 한다.

### Codex CLI를 같이 쓰면 위험한가요?

같이 쓰는 것 자체가 위험한 것은 아니다. Codex CLI는 CLI 전용 credential을 쓰고, Hermes profile은 profile별 credential을 쓰면 된다. 위험한 것은 CLI credential을 Hermes profile에 import하거나, profile 간 auth 파일을 복사해서 같은 refresh token을 공유하는 것이다.
