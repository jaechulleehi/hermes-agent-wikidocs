## GitHub와 WikiDocs로 콘텐츠를 발행하고 고치는 흐름

에르메스 에이전트(Hermes Agent) 업무 자동화에서 GitHub와 [WikiDocs](https://wikidocs.net/345908)를 나누는 이유는 원본과 공개 배포 채널을 분리하기 위해서다. GitHub는 책 원고의 source of truth이고, WikiDocs는 독자가 보는 공개 화면이다. 뽀동이가 원고를 고치고 검증을 통과시키면, GitHub 변경이 WikiDocs 발행 흐름으로 이어진다.

이 구조를 쓰는 이유는 단순하다. 공개 글은 한 번 쓰고 끝나지 않는다. 제목, TOC, 이미지, SEO/GEO, 전자책 규칙, 내부 링크가 계속 바뀐다. 이력과 기준이 남아야 안전하게 고칠 수 있다. 그래서 GitHub/WikiDocs 발행은 6장의 [WikiDocs를 먼저 쓰는 콘텐츠 시스템](https://wikidocs.net/345908)을 운영으로 고정하는 단계다.

## 실제 업무 상황

로이드가 “이 장 가자”라고 하면 뽀동이는 해당 장의 TOC와 본문을 확인하고, 세션/Slack/shared-memory/SOURCE_MAP에서 근거를 찾고, 책형 구조로 다시 쓴다. 그다음 전자책/WikiDocs 규칙을 검증한 뒤 commit/push한다. WikiDocs 화면은 GitHub에 연결된 공개 배포 결과다.

이때 WikiDocs에서 바로 수정하는 것보다 GitHub에서 수정하는 편이 좋다. [source of truth](https://wikidocs.net/345915)가 하나로 남기 때문이다.

## 발행 전 확인할 것

| 영역 | 확인 항목 |
|---|---|
| TOC | 링크가 존재하고 제목 흐름이 자연스러운가 |
| 본문 | 첫 답변, 운영 기준, FAQ, 다음 링크가 있는가 |
| 링크 | 본문 내부 링크가 WikiDocs page ID URL인가 |
| 이미지 | 상대 경로가 존재하고 위아래 빈 줄이 있는가 |
| 형식 | pages 본문에 H1이 없는가 |
| 문체 | AI 번역투, 중점 문자, 블로그 scaffolding이 남지 않았는가 |
| 안전 | 토큰, 계정값, 내부 경로, 채널 ID가 제거되었는가 |
| Git | commit/push 후 HEAD와 origin/main이 같은가 |

## 수정 기준

GitHub/WikiDocs 발행에서 중요한 것은 “지금 보이는 화면”보다 “어느 원본을 고쳤는가”다. 본문 링크가 WikiDocs에서 안 먹는 문제도 이 기준으로 해결했다. GitHub 원고의 `.md` 링크는 저장소 안에서는 자연스럽지만, WikiDocs 공개 화면에서는 page ID URL이 필요했다. 그래서 TOC는 GitHub 구조를 유지하고, 본문 링크는 WikiDocs URL로 바꿨다.

이런 판단은 단순 기술 수정이 아니라 발행 구조의 기준이다. 독자가 보는 공개 화면과 작성자가 관리하는 원본 구조가 다를 수 있기 때문이다.

## 운영 기준

1. GitHub를 원본으로 둔다.
2. WikiDocs는 공개 배포 채널로 본다.
3. 큰 수정 전에는 TOC와 SOURCE_MAP을 확인한다.
4. 본문 링크는 공개 화면에서 동작하는지 본다.
5. 검증 스크립트가 통과하기 전에는 commit하지 않는다.
6. push 후 git 상태와 HEAD/origin/main을 확인한다.
7. WikiDocs sync가 늦으면 수동 동기화 가능성을 안내한다.

## FAQ

### WikiDocs에서 바로 수정하면 안 되나요?

GitHub 연동 책에서는 GitHub가 원본이다. WikiDocs 화면에서 바로 고치면 원본과 공개본이 갈라질 수 있다. 수정은 GitHub에서 하고 WikiDocs는 배포 결과로 보는 것이 안전하다.

### 글 하나만 바꿔도 SOURCE_MAP을 수정해야 하나요?

항상은 아니다. 하지만 새 운영 케이스가 추가되거나 공식 docs 매핑이 바뀌거나 장의 역할이 달라지면 SOURCE_MAP과 BOOK_STRUCTURE도 함께 봐야 한다.

### push만 하면 발행이 끝인가요?

대부분은 GitHub webhook으로 반영된다. 다만 WikiDocs sync가 늦을 수 있으므로 필요한 경우 공개 페이지를 확인하거나 수동 동기화를 안내한다.

## 이어서 읽기

발행 흐름이 Slack 요청에서 시작되는 방식은 [Slack 스레드에서 하비가 일을 분배하는 방식](https://wikidocs.net/345995)에서 이어서 본다.
