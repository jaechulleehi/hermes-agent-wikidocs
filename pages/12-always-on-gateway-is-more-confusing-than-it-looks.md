## always-on gateway는 왜 자주 헷갈릴까

always-on gateway는 AI 개인비서를 상시 대기 상태로 만드는 장치처럼 보인다. 하지만 실제 운영에서는 “켜져 있다”와 “제대로 응답할 준비가 됐다”가 다르다. 그래서 gateway는 생각보다 자주 헷갈린다.

Hermes Agent에서 gateway를 다룰 때는 status 한 줄만 믿으면 안 된다. 어떤 profile이 떠 있는지, 실제 프로세스가 살아 있는지, autostart가 예전 설정을 물고 있지 않은지, 메시징 플랫폼과 연결이 살아 있는지까지 함께 봐야 한다.

## 왜 켜져 있는데도 헷갈릴까

상시 실행 구조는 눈에 보이지 않는 층이 많다.

| 층 | 확인할 것 |
|---|---|
| 프로세스 | 실제 Hermes gateway가 실행 중인가 |
| profile | 어느 에이전트 profile로 떠 있는가 |
| config | 현재 config와 autostart config가 같은가 |
| 메시징 연결 | Slack, Telegram 같은 플랫폼 연결이 살아 있는가 |
| heartbeat | heartbeat가 최신 상태를 말하는가 |
| 로그 | 오류가 조용히 반복되고 있지 않은가 |

하나라도 어긋나면 “분명 켜졌는데 응답이 이상한” 상태가 된다.

## heartbeat와 status를 그대로 믿으면 안 되는 이유

heartbeat는 좋은 신호지만, 항상 충분한 증거는 아니다. heartbeat가 찍혀도 실제 메시지 전달이 막혀 있을 수 있고, status가 좋아 보여도 예전 profile이 떠 있을 수 있다.

그래서 운영에서는 “상태 출력”과 “실제 동작”을 함께 확인해야 한다. 예를 들어 Slack에서 응답이 필요한 구조라면 gateway status뿐 아니라 실제 Slack thread 응답, 로그, 프로세스, config를 같이 본다.

## 실제 운영 예시

나쁜 확인은 이렇게 끝난다.

```text
gateway status가 OK니까 됐겠지.
```

좋은 확인은 이렇게 나눈다.

```text
봉구야, 실제 프로세스와 gateway status를 같이 봐줘.
어떤 profile이 떠 있는지 확인하고,
Slack 메시지 전달이 되는지 작은 테스트까지 해줘.
예전 OpenClaw autostart 흔적이 남았는지도 확인해줘.
```

상시 실행 문제는 감으로 보면 오래 걸린다. 계층별로 나눠야 빨리 잡힌다.

## 운영 기준

always-on gateway를 점검할 때는 아래 순서로 본다.

1. 실제 프로세스가 떠 있는지 확인한다.
2. 어떤 profile로 실행 중인지 확인한다.
3. 현재 config와 autostart config가 같은지 본다.
4. gateway status와 로그를 함께 본다.
5. 메시징 플랫폼으로 실제 응답 테스트를 한다.
6. 예전 autostart, LaunchAgent, OpenClaw 흔적이 남았는지 본다.
7. 재시작 후에도 같은 profile로 올라오는지 확인한다.

## FAQ

### gateway status가 OK면 끝 아닌가요?

아니다. status는 중요한 단서지만 실제 메시지 수신/응답, profile, 로그, autostart까지 확인해야 한다.

### 항상 켜두는 게 좋은가요?

상시 응답이 필요한 에이전트라면 좋다. 하지만 실험 중인 profile이나 위험한 쓰기 권한이 있는 도구는 on-demand로 시작하는 편이 안전하다.

## 다음 글

다음 글에서는 Hermes Agent Daily Briefing Bot을 cron 기반 업무 자동화 패턴으로 정리한다.

[다음 글: Daily Briefing Bot은 어떤 업무 자동화 패턴일까](05-daily-briefing-bot-workflow.md)
