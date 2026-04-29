## Hermes Agent 도구 실행 결과는 어떻게 검증할까

Hermes Agent 도구 실행의 끝은 “명령이 성공했다”가 아니다. 독자가 실제로 필요한 것은 실행 결과가 의도한 범위 안에서 반영됐고, 공개/전달/복구 기준까지 확인됐다는 확신이다. 그래서 5장의 마지막 기준은 도구 검증이다.

![Hermes Agent 도구 실행 결과 검증](../assets/images/chapter-heroes/ch5-10-tool-result-verification-codex.png)

도구 검증은 단순한 테스트가 아니다. MCP tool 호출, CLI 실행, API 조회, cron 결과, gateway delivery, WikiDocs 공개 반영은 서로 다른 실패 지점을 가진다. 같은 “성공” 메시지라도 실제 사용자가 보는 결과는 아직 틀릴 수 있다.

## 도구별 검증 기준

| 도구 흐름 | 성공처럼 보이는 신호 | 반드시 더 볼 것 |
|---|---|---|
| CLI | exit code 0 | 실제 파일 diff, working tree, 출력 의미 |
| MCP | tool result 반환 | 계정/scope, 쓰기 반영, 오류 일부 누락 여부 |
| API | HTTP 200 | pagination, rate limit, response schema, retry |
| cron | job completed | fresh session output, delivery target, 다음 실행 |
| gateway | process running | 실제 채널/스레드 delivery, 최근 로그 |
| WikiDocs sync | GitHub 원본 반영 | 공개 페이지 본문/링크/이미지 반영 |

검증은 “한 번 더 확인”이 아니라 각 도구가 가진 blind spot을 보완하는 단계다.

## 세 단계 검증: 실행 전/실행 중/실행 후

도구 검증은 실행 후에만 하지 않는다.

```text
실행 전: 범위, 권한, 작업 디렉터리, 원본 기준, rollback 기준 확인
실행 중: 로그, exit code, partial failure, timeout, background process 확인
실행 후: diff, 공개 반영, delivery, 링크/이미지, 다음 액션 확인
```

예를 들어 WikiDocs 원고를 고칠 때는 실행 전 TOC와 파일 범위를 확인한다. 실행 중에는 patch가 의도한 파일에만 들어갔는지 본다. 실행 후에는 Markdown 검증, GitHub 원본 반영, WikiDocs 공개 page ID 링크까지 확인한다.

## 좋은 완료 보고의 조건

좋은 완료 보고는 “했습니다”로 끝나지 않는다. 사용자가 다시 확인해야 할 부담을 줄여야 한다.

| 나쁜 보고 | 좋은 보고 |
|---|---|
| 수정했습니다 | 어떤 파일을 수정했고 무엇이 바뀌었는지 말한다 |
| 테스트 통과했습니다 | 어떤 검증을 돌렸고 결과가 0건인지 말한다 |
| 배포했습니다 | 원격 반영과 공개 화면 확인 여부를 구분한다 |
| 문제 없습니다 | 남은 위험/수동 확인 필요 여부를 함께 말한다 |

이 기준은 [운영 체크리스트](https://wikidocs.net/345919)와도 이어진다. 체크리스트는 작업자를 귀찮게 하는 문서가 아니라 완료 보고의 품질을 일정하게 만드는 장치다.

## 도구 검증 체크리스트

1. 실행 대상과 파일 범위가 요청과 맞는가?
2. 변경 전후 diff를 확인했는가?
3. 공식 검증 또는 프로젝트 검증 스크립트를 돌렸는가?
4. 외부 서비스라면 실제 공개/전달/동기화까지 확인했는가?
5. 실패했을 때 돌아갈 경로가 있는가?
6. 결과 보고에 완료/남음/다음이 분리되어 있는가?

## 사례: WikiDocs 발행 검증

이 책의 WikiDocs 작업은 도구 검증의 좋은 예다. GitHub가 원본 기준이고 WikiDocs가 공개 배포 채널이므로, 로컬 Markdown이 맞는 것만으로는 끝나지 않는다.

```text
1. TOC와 page 파일을 수정한다.
2. H1, heading spacing, 이미지, raw .md 링크, 불필요한 구분 문자 검증을 돌린다.
3. GitHub 원본에 commit/push한다.
4. WikiDocs TOC가 새 페이지를 받았는지 확인한다.
5. 새 page ID를 받아 본문 링크를 공개 URL로 보정한다.
6. 공개 페이지를 부분 확인한다.
```

이 흐름은 번거로워 보이지만, 독자가 보는 공개 책의 품질을 지키는 최소 기준이다. 특히 GitHub와 WikiDocs처럼 원본 기준과 배포 채널이 나뉘는 구조에서는 마지막 공개 확인이 중요하다.

## 함께 연결해서 볼 기준

검증은 마지막 보고 문구만 다듬는 일이 아니다. [terminal backend](https://wikidocs.net/346289)를 썼다면 파일/프로세스/로그를 확인해야 하고, [MCP 외부 도구](https://wikidocs.net/346231)를 썼다면 계정 권한과 실제 반영 위치를 확인해야 한다. 반복되는 검증 절차는 [Hermes Agent Skill](https://wikidocs.net/345904)로 남겨야 다음 작업에서 같은 실수를 줄일 수 있다.

## FAQ

### exit code 0이면 성공 아닌가요?

명령 실행 자체는 성공일 수 있다. 하지만 원하는 파일이 바뀌었는지, 공개 화면에 반영됐는지, delivery가 맞는 채널로 갔는지는 별도 확인이 필요하다.

### 모든 작업에 검증 스크립트가 필요한가요?

작은 문장 수정에는 간단한 diff 확인으로 충분할 수 있다. 다만 공개 발행, 자동화, 파일 대량 수정, 권한 변경은 검증 기준을 명시하는 편이 좋다.
