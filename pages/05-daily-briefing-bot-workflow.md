## Hermes Agent Daily Briefing Bot은 어떤 업무 자동화 패턴일까

Hermes Agent의 Daily Briefing Bot 공식 가이드는 단순한 아침 뉴스봇 예제가 아니다. 이 예제에는 AI 업무 자동화의 기본 패턴이 거의 모두 들어 있다.

정해진 시간에 cron이 작업을 실행하고, Hermes가 새로운 세션에서 프롬프트를 수행하고, web search로 최신 정보를 찾고, 요약한 뒤 Telegram이나 Discord 같은 메시징 채널로 전달한다. 이 흐름은 조사형 에이전트, 정리형 에이전트, 예약 실행, 메시지 전달이 하나로 묶인 자동화 구조다.

공식 가이드: https://hermes-agent.nousresearch.com/docs/guides/daily-briefing-bot

## 이 예제가 중요한 이유

많은 자동화는 “한 번 실행되는 명령”에서 끝난다. 하지만 실무에서 필요한 자동화는 반복된다. 매일 아침, 매주 월요일, 이슈가 생길 때마다 같은 기준으로 정보를 모으고 정리해 전달해야 한다.

Daily Briefing Bot은 이 반복 구조를 보여준다. 중요한 건 뉴스를 요약하는 기능 자체가 아니라, 반복 업무를 다음 흐름으로 바꾸는 방식이다.

```text
예약 실행
→ fresh session
→ 정보 수집
→ 요약/정리
→ 메시지 전달
→ 필요하면 위키나 콘텐츠로 축적
```

## 공식 가이드의 핵심 운영 기준

Daily Briefing Bot에서 특히 중요한 기준은 self-contained prompt다. cron job은 이전 대화의 맥락을 기억한 채 실행되지 않는다. 매번 fresh session에서 실행되므로, 프롬프트 안에 작업에 필요한 정보가 충분히 들어 있어야 한다.

나쁜 예시는 이런 식이다.

```text
내 평소 아침 브리핑 해줘.
```

좋은 예시는 이런 식이다.

```text
최근 24시간 동안 AI 에이전트와 오픈소스 LLM 관련 뉴스를 찾아줘.
최소 5개 자료를 확인하고, 중요한 3개를 골라줘.
각 항목에는 제목, 2문장 요약, 출처 URL을 포함해줘.
독자는 AI 업무 자동화에 관심 있는 실무자라고 가정해줘.
```

## 우리 운영에 적용하면

우리의 Hermes 운영에서는 이 패턴을 여러 곳에 쓸 수 있다.

- 매일 AI 에이전트 업데이트 브리핑
- OpenAI Codex, Claude Code, Hermes Agent 변경 사항 모니터링
- GitHub repo 변경 요약
- WikiDocs 글감 후보 정리
- 경쟁 서비스 또는 오픈소스 프로젝트 동향 요약
- Slack thread나 Obsidian 메모 기반 주간 정리

핵심은 브리핑을 읽고 끝내지 않는 것이다. 중요한 내용은 위키 원천 자료로 남기고, 일부는 WikiDocs 글감이나 강의 소재로 이어져야 한다.

## 실패할 때 먼저 볼 것

Daily Briefing Bot이 제대로 돌지 않으면 모델보다 운영 조건을 먼저 확인한다.

- gateway가 실행 중인가?
- cron schedule이 의도한 시간대에 맞는가?
- web search에 필요한 API key가 설정되어 있는가?
- delivery target이 올바른가?
- messaging이 없다면 `deliver: local`로 저장되게 했는가?
- prompt가 self-contained 한가?
- 결과 형식과 독자 정보가 prompt에 들어 있는가?

## 다음 글

이 자동화 패턴은 이후 체크리스트와 복구 플레이북에서도 다시 사용한다. 다음 장에서는 위키와 콘텐츠 시스템으로 이어지는 흐름을 다룬다.
