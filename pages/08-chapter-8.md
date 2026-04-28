## 9장. Hermes Agent 보안/마이그레이션/복구 플레이북

[Hermes Agent](https://wikidocs.net/346055)를 오래 운영하려면 좋은 답변보다 먼저 보안/마이그레이션/복구 기준이 필요하다. 같은 실수를 반복하지 않게 만드는 체크리스트, 시스템을 옮길 때 기준을 잃지 않게 하는 마이그레이션 절차, 문제가 생겼을 때 무엇부터 볼지 정하는 복구 플레이북, 그리고 실행 권한과 보안 경계를 지키는 운영 기준이 Hermes Agent 업무 자동화의 안전망이다.

![9장 Hermes Agent 보안/마이그레이션/복구 플레이북](../assets/images/chapter-heroes/ch08-checklist-migration-recovery-codex.webp)

9장은 앞 장의 [운영 FAQ와 멀티봇 규칙](https://wikidocs.net/345916)을 실행 가능한 순서로 바꾼다. 질문이 반복되면 FAQ가 되고, FAQ가 반복되면 체크리스트가 되고, 체크리스트가 실제 장애를 만나면 복구 플레이북이 된다. 여기에 공식 Hermes Agent 보안 기준인 사용자 권한, 위험 명령 승인, gateway allowlist, sandbox, credential filtering, checkpoint/rollback을 붙여야 한다. 반복되는 확인 순서는 [Hermes Agent Skill](https://wikidocs.net/346235)로 남길 수 있고, 도구 장애는 [외부 도구/MCP/채널 연동](https://wikidocs.net/345907)의 process/runtime/delivery 기준으로 되돌아간다. 이 흐름이 있어야 AI 개인비서와 역할형 에이전트 팀이 “잘 되는 날만 잘 되는 시스템”이 아니라 “문제가 생겨도 돌아오는 시스템”이 된다.

## 이 장에서 다루는 문제

| 순서 | 글 | 핵심 질문 |
|---|---|---|
| 09-1 | 시행착오를 운영 체크리스트로 바꾸는 법 | 삽질 기록을 다음 사람이 바로 쓰는 기준으로 바꿀 수 있을까 |
| 09-2 | 복구 플레이북은 왜 문서보다 순서가 중요할까 | 문제가 생겼을 때 무엇부터 확인해야 할까 |
| 09-3 | Hermes Agent 운영 체크리스트는 어떻게 써야 할까 | 체크리스트가 실제 운영 속도를 높이려면 어떤 모양이어야 할까 |
| 09-4 | Hermes Agent 보안 체크리스트는 어떻게 만들까 | 공식 security model을 운영 체크리스트로 어떻게 바꿀까 |
| 09-5 | Hermes Agent 위험 명령 승인과 YOLO mode는 어떻게 관리할까 | 빠른 실행과 안전 경계를 어떻게 나눌까 |
| 09-6 | Hermes Agent gateway 권한과 실행 격리는 어떻게 나눌까 | 호출 권한, 실행 권한, credential 권한을 어떻게 분리할까 |
| 09-7 | Hermes Agent checkpoint와 rollback은 복구 흐름에서 어떻게 쓸까 | 실행 전후 되돌릴 기준점을 어떻게 확보할까 |
| 09-8 | OpenClaw에서 Hermes로 넘어올 때 무엇을 점검해야 할까 | 전환 과정에서 memory/profile/runtime/source of truth를 어떻게 잃지 않을까 |

## 왜 체크리스트가 필요한가

운영 실수는 대개 처음 보는 문제가 아니다. gateway가 살아 있는지 확인하지 않고 cron을 고치려 하거나, GitHub가 원본인데 WikiDocs 화면만 보며 수정 상태를 판단하거나, memory에 넣으면 안 되는 작업 로그를 장기 기억으로 남기려는 일이 반복된다.

체크리스트는 이런 실수를 “다음에 조심하자”로 끝내지 않는다. 다음 작업자가 바로 따라 할 확인 순서로 바꾼다. 예를 들어 WikiDocs 발행 전에는 H1 사용 여부, TOC 링크, 이미지 경로, 본문 raw `.md` 링크, 중점 문자, 보호 정보 패턴을 확인한다. 이 순서가 있으면 긴 세션이 압축되어도 작업 품질이 유지된다.

## 실제 운영에서 배운 복구 순서

OpenClaw에서 Hermes로 넘어올 때도 핵심은 이름 변경이 아니었다. 예전 설정, cron, gateway, memory, profile, skill, agent routing 기록이 한 번에 움직였고, 일부는 자동으로 옮겨지지 않고 archive/manual review 대상으로 남았다. 이때 필요한 것은 “무엇을 다 옮겼나”가 아니라 “무엇을 믿고, 무엇을 검토하고, 무엇을 새 기준으로 다시 만들 것인가”였다.

복구도 같다. 문제가 생겼을 때 바로 재설치하거나 재시작하면 원인이 사라질 수 있다. 먼저 증상, process, 최근 변경, profile/runtime 경계, source of truth를 확인해야 한다. 그런 다음 수정하고, 마지막에 검증과 기록을 남긴다.

## 이 장에서 얻을 기준

- 사건 기록은 시간순으로 남기고, 체크리스트는 확인 순서로 남긴다.
- 복구 플레이북은 명령어 모음이 아니라 판단 순서다.
- 마이그레이션은 기존 값을 모두 복사하는 일이 아니라 현재 운영 기준으로 재분류하는 일이다.
- 보안은 단일 옵션이 아니라 호출자, 실행 위치, credential, 복구 경로를 나누는 일이다.
- 실행 전에는 증상을 보존하고, 실행 후에는 검증 결과를 남긴다.
- 공유 문서에는 내부 경로와 값이 아니라 상황, 판단, 기준, 결과만 남긴다.

## 다음 장으로 가기 전 체크 질문

- 같은 문제가 세 번 이상 반복되는데 아직 FAQ에만 남아 있지는 않은가?
- 체크리스트가 “확인한다”가 아니라 실제 확인 항목으로 쓰여 있는가?
- 복구 플레이북이 고치기 전에 볼 순서를 말하고 있는가?
- 위험 명령 승인, YOLO mode, allowlist, sandbox 기준이 문서에 남아 있는가?
- checkpoint/rollback 또는 git 기준으로 되돌릴 경로가 있는가?
- OpenClaw 같은 과거 유산과 현재 Hermes 기준이 한 문서 안에 섞여 있지는 않은가?

## 이어서 읽기

시행착오를 운영 자산으로 바꾸는 방법은 [시행착오를 운영 체크리스트로 바꾸는 법](https://wikidocs.net/345917)에서 시작한다. 복구 순서는 [복구 플레이북은 왜 문서보다 순서가 중요할까](https://wikidocs.net/345918), 보안 경계는 [Hermes Agent 보안 체크리스트](https://wikidocs.net/346259), 되돌릴 기준점은 [checkpoint와 rollback](https://wikidocs.net/346262), 마이그레이션 기준은 [OpenClaw에서 Hermes로 넘어올 때 무엇을 점검해야 할까](https://wikidocs.net/345920)에서 이어서 본다.
