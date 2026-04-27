# GitHub 연동 WikiDocs 발행 절차

## 권장 운영 구조

- 원천 메모: Lecture OS / Obsidian
- 공개 원본: GitHub 리포지토리 `hermes-agent-wikidocs`
- 배포 채널: WikiDocs GitHub 연동 책
- 보조 도구: WikiDocs MCP

## WikiDocs에서 책 만들 때 입력값

### 책 제목

에르메스 에이전트(Hermes Agent) 업무 자동화: 나만의 AI 팀 만들기

### 부제

AI 개인비서와 역할형 에이전트로 만드는 실전 업무 워크플로우

### 리포지토리 이름

hermes-agent-wikidocs

### 공개 설정

초기에는 비공개 권장. 목차, 이미지, 페이지 경로 확인 후 공개 전환.

### 책 설명

에르메스 에이전트(Hermes Agent)를 기반으로 업무 자동화와 나만의 AI 팀을 만드는 실전 기록입니다. AI 개인비서를 메인 창구로 두고 조사/정리/실행 같은 역할형 에이전트와 함께 일하며, 메모리·세션·스킬·프로필 경계, Slack·Obsidian·Google Workspace·WikiDocs 연동, 반복 업무의 체크리스트화와 복구 흐름까지 정리합니다.

## 연결 후 로컬 작업

WikiDocs가 GitHub 리포지토리를 생성한 뒤:

```bash
git remote add origin https://github.com/jaechulleehi/hermes-agent-wikidocs.git
git branch -M main
git push -u origin main
```

이미 remote가 있다면:

```bash
git remote set-url origin https://github.com/jaechulleehi/hermes-agent-wikidocs.git
git push -u origin main
```

## 반영 확인

1. GitHub에 README.md, TOC.md, pages/, assets/가 올라갔는지 확인
2. WikiDocs 책 목차가 번호 순서대로 보이는지 확인
3. 이미지가 깨지지 않는지 확인
4. 첫 페이지와 대표 페이지 3개를 열어 문체와 링크 확인

## 운영 규칙

- 수정은 GitHub에서 먼저 한다.
- WikiDocs UI에서는 본문을 직접 수정하지 않는다.
- WikiDocs MCP는 조회, 상태 확인, 블로그 보조 발행에 사용한다.
- 기존 GitHub Pages 블로그는 최신본 안내/아카이브 역할로 낮춘다.

## 전자책/PDF 호환 작성 기준

WikiDocs 전자책 변환까지 고려해 모든 페이지는 아래 기준을 따른다.

- 페이지 제목은 TOC.md에서 관리하므로, pages/*.md 본문에는 1레벨(`#`) 헤딩을 쓰지 않는다.
- 본문 헤딩은 2레벨(`##`)부터 사용한다.
- 모든 헤딩 위아래에는 빈 줄을 둔다.
- 이미지 위아래에는 빈 줄을 둔다.
- 이미지는 외부 URL보다 `assets/`에 저장한 PNG/JPG를 상대 경로로 참조한다.
- GIF 애니메이션은 PDF 변환 오류 가능성이 있으므로 피한다.
- HTML 코드는 쓰지 않고 마크다운 문법을 우선한다.
- 긴 팁/콜아웃 블록은 PDF에서 잘릴 수 있으므로 일반 본문으로 풀어 쓴다.
- Windows 경로의 역슬래시(`\`)는 코드 블록으로 감싸거나 `/`로 바꾼다.
- 각주를 사용할 경우 책 전체에서 중복되지 않는 이름을 쓴다.
- `-----` 같은 줄 구분선은 PDF 변환 오류 가능성이 있으므로 사용하지 않는다.
- 웹 전용/ PDF 전용 내용이 필요할 때만 `[[PDF_EXCLUDE]]`, `[[PDF_INCLUDE]]`를 사용한다.
