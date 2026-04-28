## 맥미니로 에르메스 에이전트(Hermes Agent)를 상시 운영하려면 무엇을 세팅할까

맥미니로 에르메스 에이전트(Hermes Agent)를 운영한다는 말은 “내 컴퓨터에 설치했다”와 다르다. Slack AI 비서, gateway, cron, 역할형 에이전트를 오래 켜두려면 실행 환경, 자동 시작 방식, 세션 유지 방식, 절전 정책, 로그 확인 순서를 함께 정해야 한다. 맥미니는 개인 노트북보다 상시 운영에 적합하지만, 아무 설정 없이 켜두면 절전/재시작/프로필 혼선 때문에 AI 개인비서가 어느 순간 답하지 않을 수 있다.

이 페이지는 로컬 실습을 넘어 Mac mini/맥미니를 전용 머신처럼 쓰려는 독자를 위한 운영 기준이다. 먼저 [설치와 세팅 기본 순서](https://wikidocs.net/346137)를 확인한 뒤, gateway가 필요해지는 순간에는 [Docker/Gateway 판단 기준](https://wikidocs.net/346139)과 함께 읽는 편이 좋다.

## 왜 맥미니가 좋은 선택지가 될까

개인 노트북은 화면을 닫거나 이동하거나 네트워크가 바뀌는 일이 많다. 반면 맥미니는 책상 한쪽에서 계속 켜두기 쉽고, Slack/Telegram/Discord 같은 메시징 gateway나 cron 기반 자동화가 끊기지 않게 운영하기 좋다. 비용과 접근성 면에서도 VPS보다 부담이 낮고, 로컬 파일/Obsidian/GitHub/WikiDocs 작업 환경을 함께 쓰기 쉽다.

하지만 장점이 곧 운영 안정성을 뜻하지는 않는다. 맥미니에 설치한 Hermes Agent가 실제 업무용 AI 개인비서가 되려면 아래 네 가지를 분리해서 봐야 한다.

| 구분 | 역할 | 흔한 착각 |
|---|---|---|
| 실행 환경 | Hermes Agent가 설치되고 profile/config가 있는 위치 | 설치됐으니 운영도 끝났다고 본다 |
| 세션 유지 | tmux처럼 사람이 붙어 보는 장기 대화/복구 창 | tmux를 상시 서비스처럼 쓴다 |
| 자동 시작 | launchd/LaunchAgent로 gateway를 다시 띄우는 방식 | 터미널 하나 켜두면 충분하다고 본다 |
| 절전 정책 | macOS가 잠자기/네트워크 끊김으로 gateway를 멈추지 않게 하는 기준 | 화면만 꺼지는 것과 시스템 잠자기를 구분하지 않는다 |

## tmux와 launchd는 같은 선택지가 아니다

맥미니에서 가장 먼저 나오는 선택지는 보통 tmux다. tmux는 SSH로 접속해도 세션을 유지할 수 있고, 장기 실행 중인 작업을 다시 확인하기 좋다. Hermes Agent나 Claude Code/Codex 같은 interactive CLI를 오래 보려면 tmux가 편하다.

하지만 tmux는 “사람이 붙어서 보고 이어갈 작업”에 가깝다. Slack gateway처럼 항상 떠 있어야 하는 입구는 tmux 창 하나에 의존하지 않는 편이 낫다. 터미널이 닫히거나 세션이 꼬였을 때 자동 복구 기준이 약하기 때문이다.

| 방식 | 좋은 경우 | 피해야 할 사용 |
|---|---|---|
| tmux | SSH 접속, 장기 대화 관찰, 수동 복구, 여러 에이전트 실험 | Slack gateway의 유일한 상시 실행 방식 |
| launchd/LaunchAgent | 로그인 후 gateway 자동 시작, 재시작, 로그 경로 고정 | 원인 진단 없이 무조건 재시작만 반복 |
| Docker restart policy | 격리된 서버형 운영, dependency 고정 | macOS 로컬 파일/키체인 접근이 많이 필요한 작업 |
| cron | 정해진 시간의 fresh session 자동화 | 사람이 붙어 판단해야 하는 장기 대화 |

따라서 맥미니 운영의 기본 판단은 단순하다. 사람이 붙어 보는 세션은 tmux, 메시징 입구와 예약 실행 축은 gateway/launchd, 위험하거나 재현성이 필요한 실행은 Docker 쪽으로 분리한다.

## launchd/LaunchAgent로 gateway를 관리할 때 볼 것

macOS에서 Hermes gateway를 계속 켜두려면 launchd 기반 LaunchAgent가 자연스러운 선택지가 된다. Hermes CLI에도 gateway service를 설치/시작/중지/재시작/상태 확인하는 명령이 있다.

```bash
hermes gateway install
hermes gateway start
hermes gateway status
hermes gateway restart
hermes gateway stop
```

여기서 중요한 것은 명령을 외우는 것이 아니다. 운영자는 “어느 profile의 gateway가 어떤 LaunchAgent로 떠 있는가”를 알아야 한다. 하비/방울이/뽀동이처럼 profile이 나뉘면 gateway도 profile별로 다르게 뜰 수 있다. 한 profile의 설정을 고쳐놓고 다른 profile의 gateway를 재시작하면, Slack에서는 아무 변화가 없는 것처럼 보인다.

확인할 항목은 아래 순서가 좋다.

```text
1. active profile이 맞는가?
2. config.yaml과 .env가 그 profile에 있는가?
3. LaunchAgent label이 어느 profile을 실행하는가?
4. ProgramArguments가 현재 Hermes 설치 경로를 가리키는가?
5. gateway log/error log 위치가 어디인가?
6. restart 후 최근 로그에 Starting 흔적이 찍혔는가?
7. Slack/Telegram/Discord에서 실제 응답이 오는가?
```

status가 정상이어도 실제 메시지가 오지 않을 수 있다. 그래서 gateway는 [process/log/delivery를 함께 확인하는 운영 기준](https://wikidocs.net/345906)으로 봐야 한다.

## 절전 설정은 “화면 꺼짐”과 “시스템 잠자기”를 나눠 본다

맥미니 상시 운영에서 자주 놓치는 부분은 절전이다. 화면이 꺼지는 것은 보통 문제가 아니다. 문제는 시스템이 잠자기에 들어가거나, 네트워크 연결이 끊기거나, 재부팅 후 gateway가 자동으로 돌아오지 않는 경우다.

팀 문서나 책으로 정리할 때는 특정 개인의 환경값을 그대로 적기보다 아래 기준을 남기는 편이 안전하다.

| 확인 항목 | 운영 기준 |
|---|---|
| 디스플레이 끄기 | 가능하다. gateway와 cron에는 보통 직접 문제가 아니다 |
| 시스템 잠자기 | 상시 gateway/cron 운영 중에는 피한다 |
| 네트워크 유지 | Slack/gateway delivery가 끊기지 않아야 한다 |
| 재부팅 후 복구 | LaunchAgent 또는 service로 자동 시작되어야 한다 |
| 임시 장기 작업 | 필요하면 `caffeinate` 같은 임시 방지 수단을 쓸 수 있다 |
| 영구 정책 | `pmset`류 설정은 현재 값 확인/복구 방법까지 함께 기록한다 |

핵심은 “잠자지 않게 만들자”가 아니라, 왜 잠자기를 막는지와 어떤 범위에서 막는지를 정하는 것이다. 임시 작업 때문에 잠깐 막는 것과, Slack AI 비서를 상시 운영하기 위해 머신 정책을 바꾸는 것은 다른 결정이다.

## 맥미니 gateway가 답하지 않을 때 확인 순서

Slack에서 AI 개인비서가 갑자기 답하지 않으면 바로 모델 문제로 보면 늦다. 맥미니 상시 운영에서는 아래 순서로 분리하는 편이 빠르다.

```text
1. 맥미니가 깨어 있고 네트워크가 살아 있는가?
2. Hermes CLI 기본 대화는 되는가?
3. 해당 profile의 gateway process가 살아 있는가?
4. launchd/LaunchAgent 상태가 loaded인지 확인했는가?
5. gateway log/error log에 최근 오류가 있는가?
6. Slack bot token/app token/channel 권한이 맞는가?
7. thread/mention/trigger 규칙 때문에 침묵한 것은 아닌가?
8. cron 결과라면 실행 실패인지 delivery 실패인지 분리했는가?
```

이 순서는 [운영 FAQ](https://wikidocs.net/345912), [gateway 권한/실행 격리](https://wikidocs.net/346261), [복구 플레이북](https://wikidocs.net/345918)으로 이어진다. 특히 멀티봇 Slack 스레드에서는 gateway가 살아 있어도 호출 규칙 때문에 일부 에이전트가 침묵하는 것이 정상일 수 있다.

## 상시 운영 점검표

맥미니를 전용 Hermes Agent host로 쓰려면 한 번 성공한 설정을 기준선으로 남겨야 한다. 아래 표는 값 자체를 기록하라는 뜻이 아니라, 어떤 종류의 정보를 확인해야 하는지 정리한 것이다.

| 점검 항목 | 남길 내용 | 주의할 점 |
|---|---|---|
| 실행 주체 | 어느 macOS 사용자/profile에서 실행되는가 | 개인 홈 경로 전체를 공개하지 않는다 |
| 시작 방식 | tmux, launchd/LaunchAgent, Docker 중 무엇인가 | tmux를 gateway 자동 복구 방식처럼 쓰지 않는다 |
| 로그 위치 | gateway log/error log를 어디서 보는가 | 토큰이 찍힌 로그를 그대로 공유하지 않는다 |
| 절전 정책 | 시스템 잠자기/네트워크 유지/재부팅 후 복구 기준 | 화면 꺼짐과 시스템 잠자기를 혼동하지 않는다 |
| 채널 검증 | Slack/Telegram/Discord에서 실제 답이 오는가 | process status만 보고 정상으로 판단하지 않는다 |
| 복구 순서 | restart 전에 무엇을 확인할 것인가 | 원인 없이 반복 restart만 하지 않는다 |

운영 문장으로 바꾸면 이렇다. “맥미니가 켜져 있다”가 기준이 아니라, “맞는 profile의 gateway가 자동 시작되고, 로그를 확인할 수 있으며, 실제 Slack thread로 delivery가 돌아온다”가 기준이다.

## 보호해야 할 운영값

맥미니 구축기는 실제성이 강할수록 유용하지만, 그대로 공개하면 안 되는 값도 많다. 아래 항목은 예시 구조만 남기고 실제 값은 공개하지 않는다.

- 개인 홈 디렉터리의 정확한 경로
- Slack channel ID, bot token, app token, webhook URL
- LaunchAgent label 중 개인/조직을 특정할 수 있는 값
- API key, OAuth token, provider credential
- 외부 접속 주소, Tailscale 이름, SSH host alias
- 내부 운영 채널명, 고객/파트너 이름

책이나 팀 문서에는 “어떤 값을 봐야 하는가”와 “왜 그 값이 중요한가”를 남기고, 실제 값은 팀 내부 문서나 secure secret store에 둔다.

## 운영 기준

- 맥미니는 상시 Hermes Agent 운영에 적합하지만, 설치만으로 운영이 끝나지 않는다.
- tmux는 수동 관찰/복구용, launchd/LaunchAgent는 gateway 자동 시작/재시작용으로 나눈다.
- macOS 절전은 화면 꺼짐/시스템 잠자기/네트워크 유지/재부팅 후 복구를 따로 본다.
- gateway 문제는 process, profile, log, delivery, trigger 규칙을 분리해서 본다.
- 공개 글에는 내부 경로/토큰/채널 ID를 남기지 않는다.

## FAQ

### 맥미니에 설치하면 Docker는 필요 없나요?

꼭 그렇지는 않다. 맥미니는 host이고 Docker는 실행 격리 방식이다. 로컬 파일과 계정을 많이 써야 하면 host 실행이 편하고, 위험 명령이나 재현 가능한 테스트가 중요하면 Docker backend가 더 안전할 수 있다.

### tmux로 gateway를 켜두면 안 되나요?

테스트나 임시 운영은 가능하다. 다만 장기 운영에서는 launchd/LaunchAgent처럼 재시작, 로그, profile, 부팅 후 복구 기준이 있는 방식이 낫다.

### 절전 방지는 무조건 해야 하나요?

상시 Slack gateway나 cron을 돌린다면 시스템 잠자기는 피하는 편이 좋다. 다만 화면 꺼짐까지 막을 필요는 없다. 어떤 절전을 막는지 구분해야 한다.

### gateway restart 후에도 답이 없으면 무엇을 먼저 보나요?

먼저 profile과 log를 본다. 재시작한 gateway가 실제 Slack에서 쓰는 profile인지, 최근 로그에 Starting이나 오류가 있는지 확인한 뒤 channel 권한과 trigger 규칙을 본다.

## 다음 글

맥미니 상시 운영을 잡았다면 다음은 [업데이트 전후 검증](https://wikidocs.net/346253)이다. 상시 gateway와 cron이 있는 환경에서는 업데이트가 단순 설치 문제가 아니라 config, profile, service, log, delivery를 함께 확인하는 운영 문제가 된다.
