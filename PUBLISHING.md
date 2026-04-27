# GitHub 연동 WikiDocs 발행 절차

## 권장 운영 구조

- 원천 메모: Lecture OS / Obsidian
- 공개 원본: GitHub 리포지토리 `hermes-agent-wikidocs`
- 배포 채널: WikiDocs GitHub 연동 책
- 보조 도구: WikiDocs MCP

## WikiDocs에서 책 만들 때 입력값

### 책 제목

AI 개인비서와 업무 자동화 워크플로우: 하비와 소환수들로 배우는 Hermes Agent

### 리포지토리 이름

hermes-agent-wikidocs

### 공개 설정

초기에는 비공개 권장. 목차, 이미지, 페이지 경로 확인 후 공개 전환.

### 책 설명

하비를 메인 창구로 두고 방울이·뽀동이 같은 소환수형 AI 에이전트와 함께 일하는 개인비서·업무 자동화 워크플로우를 정리합니다. Hermes Agent를 기반으로 메모리, 세션, 스킬, MCP, Slack/Google Workspace 연동, 기록 정리, 체크리스트, 복구 흐름까지 실제 업무에 붙여본 경험을 다룹니다.

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
