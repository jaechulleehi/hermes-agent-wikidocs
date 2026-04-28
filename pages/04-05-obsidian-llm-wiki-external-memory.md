## Obsidian LLM Wiki는 외부 장기 기억으로 어떻게 쓰일까

Obsidian LLM Wiki의 가치는 메모를 많이 쌓는 데 있지 않다. [shared-memory](https://wikidocs.net/346128)가 실행과 handoff의 공용 작업층이라면, Obsidian은 시간이 지나며 누적되는 외부 장기 기억층이다. 나중에 AI 개인비서와 역할형 에이전트가 다시 꺼내 쓸 수 있는 구조로 운영 지식을 남기는 데 있다. 특히 HaloX처럼 모니터링, 콘텐츠, 제품 신호, 운영 판단이 함께 움직이는 환경에서는 Obsidian이 기억층의 핵심이 된다.

따라서 Obsidian은 강의 시스템이나 콘텐츠 제작 도구에만 묶으면 안 된다. HALOX Brain은 누적 지식 베이스, 운영 원장, 연결형 LLM Wiki 역할을 맡는 외부 장기 기억층이다. 이 지식층이 커질수록 [OpenViking/RAG](https://wikidocs.net/346131)처럼 필요한 내용을 회수하는 구조가 중요해진다.

## Obsidian은 memory보다 크고 shared-memory보다 누적형이다

Hermes Agent의 memory는 항상 주입되는 짧은 장기 기준이다. shared-memory는 공용 작업 원본과 handoff에 가깝다. Obsidian은 시간이 갈수록 쌓이고 연결되는 지식층이다.

| 층 | 핵심 역할 | 좋은 사용 예 |
|---|---|---|
| memory | 짧은 장기 규칙 | 로이드의 반복 선호 |
| shared-memory | 공용 작업 원본 | 콘텐츠 패키지, handoff, 팀 규칙 |
| Obsidian LLM Wiki | 누적 지식/운영 원장 | 모니터링 기록, MOC, product signal, 발행 연결 |
| WikiDocs | 공개 독자용 지식 | 내부 경험을 재작성한 책 페이지 |

Obsidian은 사람이 탐색하기에도 좋고, 나중에 RAG나 OpenViking 같은 회수층과 연결하기에도 좋다. 그래서 “기록 저장소”가 아니라 “AI 팀이 다시 쓸 수 있는 지식 구조”로 설계해야 한다.

## 실제 케이스: HALOX Brain과 shared-memory를 나눈 이유

HaloX 운영에서는 매일 조사/모니터링 결과가 쌓인다. 이 결과를 전부 memory에 넣으면 다음 세션이 무거워지고, 핵심 규칙이 묻힌다. 반대로 Slack에만 두면 나중에 회수하기 어렵다.

그래서 누적 원장은 HALOX Brain에, 작업 패키지와 handoff는 shared-memory에, 반복될 선호와 팀 규칙은 memory/team-rules에 둔다. 예를 들어 방울이의 조사 결과가 콘텐츠 후보가 되면, 원장은 Obsidian에 남기고 후속 작업 패키지는 shared-memory로 이어진다.

## LLM Wiki로 쓰려면 무엇이 달라져야 할까

사람이 읽기 좋은 메모와 LLM이 회수하기 좋은 지식은 겹치지만 완전히 같지는 않다. LLM Wiki로 쓰려면 최소한 다음 구조가 필요하다.

| 요소 | 필요한 이유 |
|---|---|
| 명확한 제목 | 검색과 회수의 첫 단서가 됨 |
| frontmatter/status | 현재성, 소유자, 상태를 구분함 |
| 원자료 링크 | 근거를 다시 확인할 수 있음 |
| 판단 요약 | 왜 이 기록이 중요한지 남김 |
| 연결 노트 | 관련 개념, 콘텐츠, 제품 신호와 이어짐 |
| 다음 액션 | 후속 작업자가 이어받을 수 있음 |

이 구조가 있어야 나중에 하비가 “이건 어디서 봐야 하지?”라고 판단할 때 Obsidian을 기억층으로 쓸 수 있다.

## Obsidian을 기억층으로 쓸 때의 기준

1. 일일 로그와 긴 브리프를 memory에 넣지 않는다.
2. 누적 지식과 운영 원장은 Obsidian에 둔다.
3. shared-memory는 작업 원본과 handoff로 쓴다.
4. Obsidian 노트는 원자료/판단/상태/연결/다음 액션을 갖추게 한다.
5. RAG/OpenViking 연결을 염두에 두고 제목과 구조를 정리한다.
6. 공개 콘텐츠로 바꿀 때는 내부값을 제거하고 WikiDocs용으로 다시 쓴다.

## 자주 헷갈리는 질문

### Obsidian과 WikiDocs는 어떻게 나누나요?

Obsidian은 내부 운영 지식과 원장이고, WikiDocs는 공개 독자에게 설명하는 책이다. 같은 이야기도 공개용으로는 다시 써야 한다.

### 모든 대화를 Obsidian에 옮겨야 하나요?

아니다. 판단 기준, 근거, 후속 작업이 있는 것만 남긴다. 일상 대화 전체를 옮기면 오히려 회수가 어려워진다.

### Obsidian이 있으면 RAG가 필요 없나요?

아니다. Obsidian은 지식을 사람이 관리하는 공간이고, RAG는 필요한 지식을 검색해 모델에 주입하는 실행 구조다. Obsidian이 잘 정리되어 있을수록 RAG 품질도 좋아진다.

## 다음에 읽을 글

다음은 [OpenClaw에서 Hermes로 기억을 옮길 때 무엇을 버릴까](https://wikidocs.net/346130)에서 과거 유산과 현재 기준을 나눈다.
