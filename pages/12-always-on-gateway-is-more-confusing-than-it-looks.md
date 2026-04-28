![always-on gateway에서 프로세스 로그 delivery를 함께 확인하는 구조](../assets/how-image-agent-creates-wikidocs-visuals/ch5-3-always-on-gateway-codex.png)

## always-on gateway는 왜 자주 헷갈릴까

에르메스 에이전트(Hermes Agent)의 always-on gateway는 “켜져 있다”는 말 때문에 단순해 보인다. 하지만 운영에서는 gateway process, messaging platform 연결, cron scheduler, delivery target, 로그가 함께 맞아야 한다. 하나라도 어긋나면 status는 괜찮아 보여도 사용자는 결과를 받지 못할 수 있다.

Hermes Agent에서 gateway는 대화형 업무와 예약 실행을 연결하는 중요한 축이다. 그래서 gateway 문제는 5장 도구 자동화뿐 아니라 [체크리스트/복구 플레이북](https://wikidocs.net/345921)으로도 이어진다.

## 왜 켜져 있는데도 헷갈릴까

| 확인 대상 | 헷갈리는 이유 | 봐야 할 것 |
|---|---|---|
| process | 실행 중처럼 보일 수 있음 | 실제 PID, 최근 로그, restart 시각 |
| messaging | 플랫폼 연결은 별도 | Slack/Telegram delivery 여부 |
| cron | 스케줄러는 fresh session으로 실행 | prompt, schedule, target, recent output |
| profile | profile마다 gateway 설정이 다름 | 어떤 profile의 gateway인지 |
| target | home channel과 thread가 다름 | origin/local/명시 target 구분 |

## 실제 운영 장면: 보고 규칙 수정 후 gateway 재시작

뽀동이의 보고 방식이 장황하다는 피드백이 있었고, 프로필 운영 문서와 memory에 짧은 보고 규칙을 반영했다. 여기서 작업은 파일 수정으로 끝나지 않았다. 새 규칙이 실제 Slack 응답에 안정적으로 반영되려면 gateway 재시작과 상태 확인이 필요했다.

이 장면이 보여주는 것은 gateway가 단순 백그라운드 프로세스가 아니라는 점이다. 에이전트 정체성/프로필 변경, 메시징 응답, cron 실행이 모두 gateway와 만난다. 그래서 “수정했다”와 “실제로 반영됐다” 사이에 확인 단계가 필요하다.

## gateway status를 그대로 믿으면 안 되는 이유

status는 출발점이다. 실제 운영에서는 다음 질문까지 봐야 한다.

1. 내가 보고 있는 gateway가 맞는 profile인가?
2. 최근 로그에 restart나 오류가 남았는가?
3. 메시지가 실제 대상 채널/스레드로 갔는가?
4. cron job은 최근 실행 결과를 남겼는가?
5. delivery target이 `origin`, home channel, explicit target 중 무엇인가?

이 기준이 없으면 [Daily Briefing Bot](https://wikidocs.net/345926) 같은 자동화도 “돌았는지”와 “도착했는지”를 헷갈리게 된다.

## 운영 기준

- gateway는 process, log, delivery를 함께 확인한다.
- profile별 gateway를 섞어 보지 않는다.
- cron 결과는 실행 여부와 전달 여부를 분리해서 본다.
- 설정 변경 후에는 restart 여부와 실제 응답 변화를 확인한다.
- 공개 문서에는 내부 서비스 라벨, 계정, 토큰, 채널 ID를 그대로 쓰지 않는다.

## FAQ

### gateway status가 OK면 끝 아닌가요?

아니다. OK는 살아 있다는 신호일 뿐이다. 메시지가 원하는 채널/스레드에 전달됐는지까지 봐야 한다.

### 항상 켜두는 게 좋은가요?

메시징/cron 운영을 한다면 켜져 있어야 한다. 다만 always-on이라고 해서 검증이 필요 없는 것은 아니다.

### cron과 gateway는 어떤 관계인가요?

cron은 예약 실행이고 gateway는 메시징/스케줄러 운영 축이다. cron 결과를 Slack으로 보내려면 delivery 경로까지 맞아야 한다.

## 다음 글

다음은 [Daily Briefing Bot은 어떤 업무 자동화 패턴일까](https://wikidocs.net/345926)에서 gateway/cron/fresh session이 하나의 자동화 패턴으로 묶이는 방식을 본다.
