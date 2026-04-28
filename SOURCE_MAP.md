# SOURCE_MAP

> 이 문서는 `에르메스 에이전트(Hermes Agent) 업무 자동화: 나만의 AI 팀 만들기`의 원천 자료 지도다.  
> 기존 발행 블로그, Hermes Agent 공식 문서, 앞으로 쌓일 실무 경험을 책의 장과 페이지에 연결한다.

## 1. 이 문서의 목적

이 책은 추상적인 AI 에이전트 개념서가 아니다. 공식 Hermes Agent 기능을 실제 업무 운영 맥락으로 번역하고, 로이드가 OpenClaw에서 Hermes로 이사하며 겪은 시행착오를 체크리스트와 운영 기준으로 바꾸는 실전 운영서다.

그래서 모든 글은 세 가지 원천 중 하나 이상에 연결되어야 한다.

1. **기존 발행 블로그/초안**: 이미 쌓아둔 Hermes 운영 경험과 시행착오
2. **Hermes Agent 공식 docs**: 기능의 기준, 명령, 정석 사용법
3. **앞으로 쌓일 실무 기록**: 업데이트, 실패, 복구, 새 역할형 에이전트 운영 경험
4. **실제 운영 케이스**: 크론 조사, 방울이 확장조사, 뽀동이 글쓰기, 하비 리뷰, GitHub/WikiDocs 발행처럼 여러 기능과 역할이 한 흐름으로 묶인 사례

## 2. 원천을 책으로 바꾸는 원칙

각 원천은 그대로 붙여 넣지 않는다. 반드시 아래 흐름으로 바꾼다.

```text
실제 상황
→ 처음 착각한 지점
→ 문제가 생긴 이유
→ Hermes에서 바꾼 운영 구조
→ 독자가 따라 할 기준
→ 체크리스트/FAQ/다음 링크
```

공식 문서는 기능 기준으로, 기존 블로그는 실무 사례로, 앞으로의 기록은 업데이트 루프로 활용한다.

## 3. 기존 블로그 1~27편 매핑

| 번호 | 현재 WikiDocs 페이지 | 책 위치 | 핵심 경험/사례 | 보강 방향 |
|---|---|---|---|---|
| 01 | `01-why-we-moved-from-openclaw-to-hermes.md` | 1장/8장 | OpenClaw에서 Hermes로 넘어온 배경 | 왜 넘어왔는지와 어떻게 안전하게 이사하는지 분리 |
| 02 | `02-why-harvey-is-the-front-door.md` | 2장 | 메인 창구를 하비로 둔 이유 | 하비보다 AI 개인비서 메인 창구 개념을 먼저 설명 |
| 03 | `04-01-what-should-ai-assistant-remember.md` | 4장 | 같은 비서인데 기억이 다르게 느껴지는 문제 | session/memory/profile/source of truth 구분 강화 |
| 04 | `04-02-session-memory-profile-boundary.md` | 4장 | 파일은 있는데 도구가 못 읽는 문제 | iCloud/Obsidian/도구 권한/경로 문제를 체크리스트화 |
| 05 | `05-why-google-workspace-integration-takes-longer-than-expected.md` | 5장 | Google Workspace 연동이 오래 걸린 경험 | API보다 권한, 계정, 범위 조율이 핵심임을 강조 |
| 06 | `06-how-to-turn-stumbles-into-checklists.md` | 8장 | 삽질을 체크리스트로 바꾸는 방식 | 모든 시행착오를 운영 자산으로 바꾸는 기준 페이지화 |
| 07 | `07-why-we-write-the-wiki-first.md` | 6장 | 위키를 먼저 쓰고 블로그/강의를 나중에 뽑는 이유 | 위키를 source note hub로 설명 |
| 08 | `08-why-good-research-does-not-automatically-become-a-good-blog.md` | 6장 | 좋은 조사 결과가 바로 좋은 글이 되지 않는 문제 | 조사형→정리형→콘텐츠형 흐름을 강화 |
| 09 | `09-hermes-is-an-operating-system-not-just-a-chatbot.md` | 1장 | Hermes를 챗봇이 아니라 운영 시스템으로 본 관점 | 책의 핵심 관점 페이지로 격상 |
| 10 | `10-when-and-how-to-manage-skills-in-hermes.md` | 5장 | 스킬을 언제 만들고 관리할지 | 스킬 vs cron vs 자동화 판단 기준 추가 |
| 11 | `04-05-obsidian-llm-wiki-external-memory.md` | 6장 | Obsidian LLM Wiki 실전 운영 | 위키/메모리/콘텐츠 시스템 연결 강화 |
| 12 | `12-always-on-gateway-is-more-confusing-than-it-looks.md` | 5장/8장 | always-on gateway 혼선 | gateway status, cron, delivery 문제 복구 기준화 |
| 13 | `04-06-openclaw-memory-migration.md` | 4장/8장 | 과거 유산과 현재 기준점 정리 | OpenClaw 잔여물/source of truth 정리와 연결 |
| 14 | `14-most-common-hermes-ops-questions-and-how-to-think.md` | 7장 | Hermes 운영 FAQ | FAQ를 장별 질문/답변으로 재분산 가능 |
| 15 | `15-why-multibot-threads-get-noisy.md` | 7장 | 멀티봇 스레드가 시끄러워지는 문제 | Slack/Telegram/Discord 호출 규칙과 연결 |
| 16 | `16-why-research-agents-rush-to-conclusions.md` | 3장 | 조사형 에이전트가 결론을 서두르는 문제 | 근거/해석/가설 구분 체크리스트 추가 |
| 17 | `17-why-writing-agents-produce-polished-but-weak-drafts.md` | 3장/6장 | 정리형 에이전트가 그럴듯하지만 약한 글을 만드는 문제 | 원자료/목적/독자/CTA 기준 강화 |
| 18 | `18-when-harvey-should-handle-directly-vs-delegate.md` | 2장 | 메인 창구가 직접 처리할지 위임할지 판단 | 위임 기준표 추가 |
| 19 | `19-why-bangwooli-and-ppodongi-fail-differently.md` | 3장 | 조사형과 정리형의 실패 패턴 차이 | 역할 분리의 핵심 근거 페이지로 강화 |
| 20 | `20-why-recovery-playbooks-are-about-order-not-just-docs.md` | 8장 | 복구 플레이북은 문서보다 순서가 중요하다는 경험 | 복구 순서 템플릿 추가 |
| 21 | `21-how-to-make-hermes-operation-checklists-actually-useful.md` | 8장 | 운영 체크리스트가 실제 도움이 되려면 필요한 조건 | checklist-first 운영 원칙 강화 |
| 22 | `22-why-harvey-main-window-is-powerful-and-where-it-bottlenecks.md` | 2장 | 메인 창구 구조의 강점과 병목 | 단일 창구의 장점/한계/분산 기준 정리 |
| 23 | `23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md` | 3장 | 조사형 에이전트의 강점과 흔들림 | 조사 요청 예시/나쁜 요청 예시 추가 |
| 24 | `24-where-ppodongi-organization-agent-is-strong-and-where-it-wobbles.md` | 3장 | 정리형 에이전트의 강점과 흔들림 | 입력 자료/목적/독자 기준 추가 |
| 25 | `25-why-openclaw-to-hermes-needed-a-migration-checklist.md` | 8장 | OpenClaw→Hermes 마이그레이션 체크리스트 필요성 | 실제 이사 방법 페이지로 보강 |
| 26 | `04-08-context-compaction-handoff.md` | 4장 | 긴 대화와 컨텍스트 부패 | context compaction/source of truth 운영 기준 추가 |
| 27 | `27-why-agent-adoption-fails-in-operations.md` | 9장 | 초도 도입 조직이 운영에서 무너지는 이유 | 조직 도입 체크리스트와 온보딩 기준으로 확장 |

## 4. 공식 Hermes Agent docs 매핑

0장 기초 가이드는 공식 문서와 검색 초입 질문을 연결하는 관문이다. 공식 GitHub/공식 Docs/Installation/Quickstart/OpenClaw migration/Docker/Gateway/Claude Code/Codex 비교 질문은 0장에서 먼저 받고, 1장 이후는 AI 개인비서와 역할형 에이전트 운영 구조로 이어진다.

공식 문서는 기능의 기준이다. 책에서는 공식 문서를 그대로 반복하지 않고, 실제 운영 패턴과 시행착오로 번역한다.

| 공식 docs 주제 | 책 위치 | 공식 기능 요약 | 우리 책에서 다룰 실무 해석 | 필요한 페이지/보강 |
|---|---|---|---|---|
| Daily Briefing Bot | 5장/6장/8장 | cron, web search, summarization, messaging delivery | 조사형→정리형→예약 실행→메시지 전달의 기본 자동화 패턴 | `05-daily-briefing-bot-workflow.md` 보강 |
| Automate Anything with Cron | 5장/8장 | 프롬프트 기반 예약 실행 | 반복 업무를 cron으로 만들 때 필요한 self-contained prompt와 검증 기준 | cron 운영 페이지 필요 |
| Cron Troubleshooting | 8장 | gateway, schedule, delivery 문제 해결 | 예약 작업이 안 돌 때 확인 순서 | gateway/cron 복구 체크리스트 필요 |
| Delegation & Parallel Work | 3장/5장 | sub-agent 병렬 작업 | 조사형/정리형/실행형 분리의 공식 기능 기반 | 역할 분리 페이지에 공식 연결 추가 |
| Use MCP with Hermes | 5장 | 외부 도구 연결 | MCP를 기능이 아니라 업무 흐름 연결 수단으로 설명 | MCP 실무 기준 페이지 필요 |
| Working with Skills | 5장 | 재사용 절차 관리 | 스킬을 만들지 cron/자동화로 만들지 판단하는 기준 | 스킬 판단표 보강 |
| Messaging Platforms | 2장/7장 | Telegram/Discord 등 메시징 연결 | 메인 창구, delivery target, 멀티봇 호출 규칙 | 메시징 운영 페이지 필요 |
| Memory | 4장 | 지속 기억 | session/memory/skill/wiki/source of truth의 경계 | 기억 경계 페이지 보강 |
| Migrate from OpenClaw | 1장/8장 | OpenClaw에서 Hermes로 전환 | 왜 옮겼고 어떻게 이사/검증/복구했는지 | 마이그레이션 체크리스트 보강 |
| Tips & Best Practices | 전 장 | 운영 팁 | 좋은 프롬프트보다 좋은 운영 구조가 먼저라는 기준 | 각 장 FAQ/체크리스트에 분산 |
| Team Telegram Assistant | 2장/7장/9장 | 팀용 메신저 assistant | 개인 AI 비서가 팀 채널로 확장될 때의 권한/호출 규칙 | 조직 도입 장 보강 |
| Use Voice Mode with Hermes | 2장/9장 | 음성 인터페이스 | 메인 창구가 텍스트 밖으로 확장될 때의 기준 | 후순위 |
| Build a Plugin | 5장/9장 | 플러그인 개발 | 기능 확장이 필요할 때 스킬/MCP/plugin 중 선택 | 후순위 |
| GitHub PR Review Agent | 3장/5장/9장 | PR 리뷰 자동화 | 실행형/검토형 에이전트의 조직 적용 사례 | 후순위 |

## 5. 실제 운영 케이스 수집 기준

실제 운영 케이스는 10장에 우선 모은다. 단, 단순 일화나 성공담이 아니라 독자가 따라 할 수 있는 업무 자동화 패턴이어야 한다.

케이스를 수집할 때는 다음 항목을 남긴다.

```text
케이스 제목:
실제 업무 상황:
시작점: cron / Slack 요청 / GitHub 변경 / 외부 도구 이벤트 중 무엇인가
참여 역할: 하비 / 방울이 / 뽀동이 / 하망이 / 실행형 에이전트
사용 도구: Slack / cron / GitHub / WikiDocs / Obsidian / MCP 등
처음 헷갈린 지점:
역할 분리 기준:
발행 또는 검증 기준:
민감정보 제거 필요 여부:
관련 장: 개념 설명 장 / 실제 케이스 장 / FAQ 또는 체크리스트 장
본문 반영 후보:
```

10장에 들어간 케이스는 이후 7장 FAQ 또는 8장 체크리스트로 이어질 수 있어야 한다. 실제 케이스가 쌓이는데 운영 기준으로 전환되지 않으면 책이 사례 모음으로 흩어진다.

## 6. 앞으로 나올 이야기 수집 기준

앞으로 생기는 Hermes Agent 업데이트나 우리 운영 경험은 바로 본문에 섞지 않는다. 먼저 아래 기준으로 기록한다.

```text
주제:
발생한 실제 상황:
공식 docs와 연결되는가:
영향받는 장:
실패/시행착오:
바뀐 운영 기준:
체크리스트 반영 여부:
본문 반영 후보:
민감정보 제거 필요 여부:
```

## 7. 앞으로 나올 이야기 후보

| 후보 주제 | 들어갈 장 | 왜 필요한가 | 현재 상태 |
|---|---|---|---|
| Claude Code로 SSH 접속 후 복구할 때 제일 먼저 봐야 할 것 | 8장 | 복구 순서와 실제 실행형 에이전트 운영 사례 | 후보 |
| 하비 메인 창구 구조에서 병목을 줄이는 법 | 2장 | 단일 창구의 한계와 위임 기준 보강 | 후보 |
| profile 경계와 state db를 실무에서 보는 법 | 4장 | 기억/상태/프로필 혼선 보강 | 후보 |
| 팀형 handoff 문서는 어느 수준이어야 이어지는가 | 7장/9장 | 멀티봇/조직 운영에서 handoff 품질 기준 필요 | 후보 |
| tmux SSH Tailscale 운영 유의사항 | 8장 | 원격 복구와 운영 습관 사례 | 후보 |
| Hermes 운영 FAQ는 언제 위키보다 체크리스트로 내려와야 할까 | 7장/8장 | 문서와 체크리스트의 경계 | 후보 |
| 이미지형 에이전트는 예쁜 그림보다 메시지가 먼저다 | 3장/6장 | 이미지/썸네일 운영 경험 반영 | 후보 |
| 콘텐츠형 에이전트는 채널별 목적을 어떻게 나눌까 | 3장/6장 | 블로그/WikiDocs/강의/카드뉴스 변환 기준 | 후보 |
| 크론 조사에서 WikiDocs 발행까지 이어지는 콘텐츠 운영 흐름 | 10장 | 자동화/역할 분리/발행 기준이 한 번에 보이는 대표 케이스 | 1차 반영 |
| 방울이 확장조사와 뽀동이 글쓰기 handoff | 10장 | 조사형과 정리형 에이전트 협업 기준을 보여주는 케이스 | 1차 반영 |
| GitHub와 WikiDocs로 발행하고 수정하는 흐름 | 10장/6장 | GitHub source of truth와 WikiDocs 공개 채널 기준을 보여주는 케이스 | 1차 반영 |
| Slack 스레드에서 하비가 일을 분배하는 방식 | 10장/7장 | 메인 창구, 호출 규칙, 역할 분리 기준이 드러나는 케이스 | 1차 반영 |
| SEO/GEO 치트시트를 콘텐츠 자산으로 만드는 흐름 | 10장/6장 | 이미지 제작, QA, skill화, 배포 문안, WikiDocs 기록이 이어지는 콘텐츠 자산화 케이스 | 1차 반영 |

## 8. 장별 보강 체크리스트

| 장 | 지금 역할 | 실무 사례 보강 질문 |
|---|---|---|
| 1장 | 관점 전환 | 우리가 실제로 언제 “챗봇으로는 부족하다”고 느꼈는가? |
| 2장 | 메인 창구 | 하비 메인 창구가 편했던 순간과 병목이 된 순간은 무엇인가? |
| 3장 | 역할 분리 | 방울이/뽀동이/실행형/이미지형/콘텐츠형이 각각 어디서 실패했는가? |
| 4장 | 기억 경계 | memory/session/profile/source of truth 때문에 헷갈린 실제 사례는 무엇인가? |
| 5장 | 도구/자동화 | 공식 docs 기능을 붙일 때 권한, gateway, cron, delivery에서 어디가 막혔는가? |
| 6장 | 콘텐츠 시스템 | 좋은 조사 결과가 글로 약해진 사례, 위키가 자산이 된 사례는 무엇인가? |
| 7장 | 운영 FAQ | 멀티봇 스레드가 시끄러워진 실제 상황과 호출 규칙은 무엇인가? |
| 8장 | 복구/마이그레이션 | OpenClaw→Hermes 이사 중 실제로 체크해야 했던 순서는 무엇인가? |
| 9장 | 조직 확장 | 조직 도입에서 모델보다 운영 병목이 먼저 드러난 사례는 무엇인가? |
| 10장 | 실제 운영 케이스 | 크론/Slack/GitHub/WikiDocs와 역할형 에이전트가 한 흐름으로 묶인 실제 사례는 무엇인가? |

## 9. 페이지 리라이트 기준

각 페이지를 리라이트할 때 아래를 확인한다.

- 첫 문단에 독자 문제와 답이 있는가?
- 공식 docs 내용과 우리 경험이 구분되어 있는가?
- 실제 시행착오가 최소 1개 이상 들어가는가?
- 체크리스트나 판단 기준으로 끝나는가?
- 하비/방울이/뽀동이 같은 내부 이름보다 공개 개념이 먼저 나오는가?
- 민감한 토큰, webhook, 개인 경로, 계정 정보가 제거되어 있는가?
- 다음 글로 이어지는 연결 문장이 있는가?
- 실제 운영 케이스라면 발행 기준, 민감정보 제거 기준, 재사용 체크리스트가 들어가는가?

## 10. 4장 기억 시스템 개편 메모

- 4장은 `AI 에이전트 기억 시스템은 어떻게 설계해야 할까`로 확장했다.
- Obsidian LLM Wiki는 6장 콘텐츠 시스템이 아니라 4장 외부 장기 기억층으로 이동했다.
- OpenViking/RAG는 memory를 대체하는 기능이 아니라 Obsidian/shared-memory에 쌓인 지식을 회수하는 외부 메모리 강화층으로 설명한다.
- 하비는 단순 라우터가 아니라 memory/shared-memory/Obsidian/skill/session_search/OpenViking 중 어디에 정보를 둘지 판단하는 기억 오케스트레이터로 다룬다.

## 11. 현재 결론

현재 책의 기초 구조는 좋다. 다만 아직 “배치”가 끝난 단계에 가깝고, 최종 품질은 각 글 안에 실제 경험과 시행착오를 얼마나 잘 넣느냐에 달려 있다.

따라서 다음 작업은 무작정 새 글을 늘리는 것이 아니라, 이 `SOURCE_MAP.md`를 기준으로 각 장을 하나씩 리라이트하는 것이다.

추천 순서는 다음과 같다.

1. 1장 입문 글 2개와 기존 1장 글 2개를 실무 사례 중심으로 다듬는다.
2. 3장 역할형 에이전트 글을 조사형/정리형/실행형/이미지형/콘텐츠형까지 확장한다.
3. 5장에 공식 docs 기반 cron/MCP/skills/gateway 내용을 실무 패턴으로 보강한다.
4. 10장 실제 운영 케이스를 크론 조사, handoff, GitHub/WikiDocs 발행, Slack 분배 흐름 중심으로 보강한다.
5. 8장에 OpenClaw→Hermes 이사 방법과 복구 체크리스트를 실제 절차로 보강한다.
6. 이후 6장 콘텐츠 시스템과 9장 조직 확장을 확장한다.
