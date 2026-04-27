## Obsidian LLM Wiki는 운영 기록을 어떻게 살릴까

Obsidian LLM Wiki는 단순한 노트 저장소가 아니다. [Hermes Agent](https://wikidocs.net/346055)로 일하면서 생긴 조사, 판단, 실패, 발행 기준을 다시 꺼내 쓸 수 있게 만드는 운영 기록층이다.

AI 개인비서와 [역할형 에이전트](https://wikidocs.net/345925)를 오래 쓰면 대화는 빠르게 쌓인다. 하지만 Slack 스레드나 세션 안에만 남은 지식은 다음 작업에서 다시 찾기 어렵다. 그래서 중요한 운영 경험은 WikiDocs, Obsidian, repo 문서처럼 다시 읽을 수 있는 위치로 옮겨야 한다.

우리 운영에서 이 문제가 가장 선명하게 드러난 곳은 HaloX 기록 체계였다. 처음에는 memory, shared-memory, Obsidian, Slack, GitHub가 모두 “기록”처럼 보였다. 하지만 실제로 써보니 역할이 달랐다. memory는 오래 유지할 규칙, shared-memory는 공용 작업 원본, Obsidian `HALOX Brain`은 시간이 지나며 다시 회수할 지식 베이스에 가까웠다.

## 문서를 많이 쌓아도 다시 못 쓰는 이유

문서가 많아도 다시 못 쓰는 경우가 있다. 이유는 보통 세 가지다.

| 문제 | 결과 |
|---|---|
| 제목이 상황만 말하고 질문을 말하지 않음 | 나중에 검색해도 찾기 어렵다 |
| 결정과 근거가 분리되어 있지 않음 | 왜 그렇게 정했는지 알 수 없다 |
| 다음에 쓸 기준이 남지 않음 | 같은 시행착오를 반복한다 |
| 기록 위치가 역할별로 나뉘지 않음 | memory, shared-memory, Obsidian이 서로 중복된다 |

LLM Wiki는 기록을 많이 쌓는 도구라기보다, 다시 회수할 수 있게 만드는 구조다. 중요한 것은 “어디에든 남긴다”가 아니라 “나중에 어떤 질문으로 다시 찾을지”를 기준으로 남기는 것이다.

## 실제 케이스: HALOX Brain과 shared-memory를 나눈 이유

HaloX 운영에서는 다음 3계층 기준을 세웠다.

| 계층 | 넣는 것 | 넣지 않는 것 |
|---|---|---|
| memory | 장기 규칙, 사용자 선호, 에이전트 역할 기준 | 일일 로그, 긴 초안, 임시 상태 |
| shared-memory | 팀 공통 규칙, handoff, content package, research queue | 개인 기억, 장기 선호 원문 |
| HALOX Brain | daily/weekly monitoring, glossary, framework, MOC, 누적 지식 | 최신 발행 상태의 단일 원본 |

이 기준은 추상 원칙이 아니라 실제 운영에서 생겼다. 방울이가 매일 제품/시장 신호를 모으면 그 결과를 Obsidian `HALOX Brain/01_Research/monitoring/daily`에 남기고, 강한 신호만 research queue나 feature idea로 승격한다. 뽀동이가 콘텐츠 패키지를 만들 때는 원고와 메타, 내부 링크, 시각 자료 방향을 shared-memory의 `content-system/packages/`에 둔다. 하비는 최종 답변 전에 HALOX Brain과 shared-memory를 같이 확인한다.

이렇게 나누면 “기록이 많아졌는데 어디가 원본인지 모르겠다”는 문제가 줄어든다. Obsidian은 사람이 탐색하기 좋은 연결형 위키가 되고, shared-memory는 에이전트들이 함께 물고 일하는 작업 원본이 된다.

## 실제 케이스: AI 버튼 글 패키지를 다시 찾을 수 있게 만든 방식

HaloX 콘텐츠 작업에서도 LLM Wiki 구조가 필요했다. 예를 들어 AI 버튼과 agentic discovery UX 글을 만들 때, 원천은 하나가 아니었다.

- 방울이/하비가 모은 research note
- 기존 HaloX 블로그 인덱스
- 이전 발행 글의 메타/내부 링크 규칙
- 새 글의 draft, schema, visual guide
- 나중에 다시 찾을 수 있는 content package 경로

이 작업은 단순히 “블로그 글 하나 작성”이 아니었다. `00-brief.md`, `10-draft.md`, `20-meta.json`, `30-schema.jsonld`, `60-internal-links.md`, `70-article-visuals.md`처럼 파일을 나눠 두었기 때문에 이후 뽀동이가 제목, 메타, 내부 링크, 이미지 방향을 다시 회수할 수 있었다.

여기서 Obsidian LLM Wiki의 역할은 완성 원고를 저장하는 데서 끝나지 않는다. 어떤 주제가 왜 나왔고, 어떤 기존 글과 구분되며, 어떤 내부 링크를 붙여야 하는지까지 연결해 둔다. 그래야 다음에 비슷한 GEO/AI search 글을 만들 때 “지난번에 어떻게 했더라?”를 처음부터 다시 묻지 않는다.

## memory와 wiki는 다르다

[memory](https://wikidocs.net/345899)에는 오래 유지할 사용자 선호와 운영 규칙을 넣는다. 반면 wiki에는 맥락, 사례, 근거, 흐름, 시행착오를 담는다.

예를 들어 “로이드는 구분할 때 슬래시를 선호한다”는 memory에 가깝다. 하지만 왜 WikiDocs 글에서 구분자 규칙이 필요했는지, 어떤 파일을 고쳤는지, 이후 검증 기준이 어떻게 바뀌었는지는 wiki나 repo 문서에 남기는 편이 좋다.

또 “HALOX 질문은 HALOX Brain → shared-memory → memory 순서로 본다”는 장기 운영 규칙이다. 하지만 특정 날짜의 모니터링 결과, 특정 콘텐츠 패키지의 초안, 특정 스레드에서 나온 발행 판단은 memory에 넣으면 안 된다. 그런 내용은 Obsidian이나 shared-memory에 남겨야 한다.

## 실제 운영 흐름

운영에서 LLM Wiki는 아래 순서로 쓰는 편이 안정적이다.

1. 새 질문이나 시행착오가 생긴다.
2. [방울이](https://wikidocs.net/345895)가 관련 근거와 기존 기록을 찾는다.
3. 하비가 현재 작업과 연결되는지 판단한다.
4. 뽀동이가 WikiDocs나 운영 문서 구조로 정리한다.
5. 반복 기준이 생기면 skill이나 체크리스트로 옮긴다.
6. 공개 가능한 내용은 WikiDocs 책으로 재작성한다.

이 흐름을 만들면 대화가 끝나도 지식이 사라지지 않는다. 특히 Slack에서 시작된 작업은 그대로 두면 금방 묻힌다. 로이드가 “스터디 기록들 다 가지고 있어?”라고 확인했던 것처럼, 대화 기록은 결국 카테고리와 연결 구조가 있어야 다음 사람이 따라올 수 있다.

## Obsidian에 남길 때의 최소 단위

Obsidian LLM Wiki에 남길 때는 긴 회고보다 재사용 단위가 중요하다.

```text
질문:
왜 이 기록을 남기는가?

상황:
어떤 Slack/세션/작업에서 나온 이야기인가?

판단:
무엇을 현재 기준으로 삼았는가?

근거:
어떤 문서, 링크, 파일, 세션을 봤는가?

다음 기준:
다음에 비슷한 일이 생기면 무엇을 먼저 볼 것인가?
```

이 다섯 가지가 있으면 LLM이 나중에 다시 읽어도 맥락을 회수하기 쉽다. 반대로 긴 대화 원문만 붙여두면 사람도, 에이전트도 다시 쓰기 어렵다.

## 운영 기준

Obsidian LLM Wiki를 살리려면 아래 기준을 둔다.

- 제목에는 상황보다 질문을 담는다.
- 문서 안에 결정, 근거, 다음 기준을 분리해 둔다.
- 민감정보는 원문으로 남기지 않는다.
- 단순 로그는 쌓아두지 말고 재사용 가능한 기준으로 바꾼다.
- 공개 가능한 내용은 WikiDocs 구조로 옮긴다.
- 반복되는 절차는 skill로 빼낸다.
- HaloX 업무는 HALOX Brain, Lecture 자산은 Lecture OS처럼 볼트의 역할을 나눈다.
- Obsidian에 있더라도 최신 발행 상태는 shared-memory나 GitHub source of truth로 교차 확인한다.

## FAQ

### Obsidian과 WikiDocs는 어떻게 나누나요?

Obsidian은 내부 운영 기록과 원천 노트에 가깝다. WikiDocs는 독자가 읽을 수 있게 재구성한 공개 책이다. 같은 사건이라도 Obsidian에는 맥락과 원천을 남기고, WikiDocs에는 독자가 따라 할 수 있는 운영 기준과 사례만 남긴다.

### 모든 대화를 위키에 옮겨야 하나요?

아니다. 중요한 결정, 반복되는 시행착오, 다음 작업에 영향을 주는 기준만 옮기면 된다. 단순 실행 로그나 일회성 대화는 위키보다 세션 기록으로 충분하다.

### shared-memory와 Obsidian 중 무엇을 먼저 봐야 하나요?

HaloX 질문은 HALOX Brain을 먼저 보고, 최신 작업 원본이나 패키지는 shared-memory로 교차 확인한다. 규칙 충돌이 있으면 `team-rules.md`나 `halox-memory-ops.md` 같은 source of truth 문서를 우선한다.

## 다음 글

다음 글에서는 왜 WikiDocs를 먼저 쓰고 블로그/강의를 나중에 뽑는지 다룬다. Obsidian과 shared-memory가 원천 기록을 살리는 층이라면, WikiDocs는 그 기록을 독자가 읽을 수 있는 공개 운영서로 바꾸는 층이다.

[다음 글: 왜 WikiDocs를 먼저 쓰고 블로그/강의를 나중에 뽑을까](https://wikidocs.net/345908)
