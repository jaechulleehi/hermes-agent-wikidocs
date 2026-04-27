# GitHub 연동 WikiDocs 발행 절차

## 권장 운영 구조

- 원천 메모: Lecture OS / Obsidian
- 공개 원본: GitHub 리포지토리 `hermes-agent-wikidocs`
- 배포 채널: WikiDocs GitHub 연동 책
- 보조 도구: WikiDocs MCP

## WikiDocs에서 책 만들 때 입력값

### 책 제목

에르메스 에이전트(Hermes Agent) 운영 입문

### 리포지토리 이름

hermes-agent-wikidocs

### 공개 설정

초기에는 비공개 권장. 목차, 이미지, 페이지 경로 확인 후 공개 전환.

### 책 설명

AI 에이전트를 단순 챗봇이 아니라 실제 업무 시스템으로 운영하기 위한 Hermes Agent 구축·운영 가이드. 멀티 에이전트 역할 분리, MCP 자동화, 메모리 관리, Slack/Google Workspace 연동, 운영 체크리스트와 복구 플레이북을 다룹니다.

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
