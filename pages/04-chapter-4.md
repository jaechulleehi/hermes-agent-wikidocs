![4장 기억 컨텍스트 프로필 경계를 나누는 구조](../assets/images/chapter-heroes/ch04-memory-context-profile-boundary-codex.png)

## 4장. 기억/컨텍스트/프로필 경계

AI 개인비서를 오래 쓰다 보면 “왜 같은 하비인데 오늘은 다르게 기억하지?”라는 순간이 온다. 답은 모델 성능보다 운영 경계에 있다. [memory/profile/session](https://wikidocs.net/345899), shared-memory, Obsidian, GitHub 원본이 서로 다른 역할을 하는데, 이 층을 하나처럼 다루면 기억이 흔들려 보인다.

우리도 이 문제를 여러 번 겪었다. WikiDocs 책을 보강할 때도 현재 대화만 보면 “4장부터 이어서 쓰자” 정도로 보이지만, 실제로는 앞선 세션 요약, Slack 스레드, shared-memory 규칙, `SOURCE_MAP.md`, GitHub 커밋 상태를 함께 봐야 했다. 이 중 하나만 보면 내용은 맞아도 흐름이 끊긴다.

이 장은 기억을 많이 넣는 법이 아니라, 어디에 무엇을 남기고 어디를 먼저 봐야 하는지 정하는 장이다.

## 이 장에서 다루는 문제

| 문제 | 겉으로 보이는 증상 | 실제로 나눠야 할 경계 |
|---|---|---|
| 같은 AI 개인비서가 다르게 느껴짐 | 어제 말한 걸 모르는 것처럼 보임 | session, memory, profile, source of truth |
| 파일은 있는데 못 읽음 | Finder나 Obsidian에는 보이지만 도구가 실패함 | 경로, 권한, 동기화, 실행 환경 |
| 과거 규칙이 현재 기준을 흔듦 | 예전 OpenClaw 방식과 Hermes 방식이 섞임 | archive, current rule, migration note |
| 긴 대화가 흐려짐 | 앞에서 합의한 작업 범위가 약해짐 | context compaction, handoff, 검증 기준 |

## 실제 운영에서 생긴 장면

OpenClaw에서 Hermes로 넘어올 때 가장 헷갈렸던 것은 “무엇이 이사된 것인가”였다. 예전 SOUL, memory, cron, gateway 흔적이 남아 있었지만 Hermes에서는 같은 구조로 자동 이식되지 않았다. 그래서 과거 유산은 archive로 두고, 현재 기준은 Hermes의 [profile](https://wikidocs.net/345899), AGENTS, shared-memory, skill, cron으로 다시 나눠야 했다.

또 하나의 장면은 WikiDocs 보강 작업이다. 로이드가 “내용이 축약되어 보인다”고 말했을 때, 단순히 현재 파일만 길게 늘리면 해결되지 않았다. 세션 기록에서 왜 그런 판단이 나왔는지 찾고, Slack 스레드에서 요청 흐름을 확인하고, shared-memory에서 민감정보 제거 기준을 확인한 뒤 본문에 공개 가능한 이야기만 남겨야 했다.

## 이 장에서 얻을 기준

- **memory**는 오래 유지될 선호/규칙만 담는다.
- **session**은 현재 대화의 작업 범위와 임시 맥락을 담는다.
- **profile**은 에이전트의 역할/권한/도구 환경을 나눈다.
- **shared-memory**는 팀이 함께 보는 운영 원본과 handoff를 담는다.
- **Obsidian/HALOX Brain**은 누적 지식과 모니터링 원장에 가깝다.
- **GitHub/WikiDocs**는 공개 문서의 source of truth와 배포 채널을 나눠 맡는다.

이 기준이 잡혀야 [외부 도구/MCP/자동화 운영](https://wikidocs.net/345907)도 안정된다. 도구가 실패했을 때 “모델이 못 했다”고 보기 전에, 어느 계층에서 정보가 끊겼는지 봐야 하기 때문이다.

## 다음 장으로 가기 전 체크 질문

1. 지금 문제는 기억 부족인가, 경로/권한/프로필 문제인가?
2. 장기 규칙으로 남길 것과 현재 세션에서만 필요한 것을 나눴는가?
3. 과거 문서와 현재 기준이 충돌할 때 source of truth가 어디인지 정했는가?
4. 긴 대화가 이어질 때 handoff와 검증 기준이 남아 있는가?

## 이어서 읽기

먼저 [같은 AI 개인비서인데 기억은 왜 다르게 느껴질까](https://wikidocs.net/345899)에서 memory/session/profile을 나눈다. 그다음 [파일은 있는데 도구는 왜 못 읽을까](https://wikidocs.net/345900)에서 실행 환경과 권한 문제를 확인한다.
