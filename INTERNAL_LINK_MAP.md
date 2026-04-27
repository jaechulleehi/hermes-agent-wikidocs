# INTERNAL_LINK_MAP

> 이 문서는 2차 작업의 첫 단계인 내부 링크 보강 기준이다.  
> 목표는 WikiDocs 책이 페이지 묶음이 아니라 순서와 회귀 링크가 있는 운영서로 읽히게 만드는 것이다.

## 1. 내부 링크 원칙

내부 링크는 단순히 페이지 끝에 `이어서 읽기`를 붙이는 작업이 아니다. 독자가 본문을 읽는 중에 핵심 개념, 기능, 실제 케이스로 자연스럽게 이동할 수 있어야 한다.

내부 링크는 네 종류로 나눈다.

1. **본문 맥락 링크**: 문장 안에서 처음 등장하는 주요 개념/기능을 관련 페이지로 연결하는 링크
2. **순차 링크**: 다음 글로 자연스럽게 넘어가는 링크
3. **개념 링크**: 앞에서 설명한 핵심 개념으로 돌아가는 링크
4. **케이스 링크**: 개념을 실제 운영 사례로 확인하는 링크

각 페이지에는 본문 맥락 링크를 최소 2개 이상 두는 것을 기본으로 한다. `이어서 읽기`는 보조 동선이고, 핵심은 본문 안의 개념 연결이다.

주의: GitHub 연동 WikiDocs의 본문에서는 `파일명.md` 링크가 독자 화면에서 제대로 동작하지 않을 수 있다. 공개 본문 내부 링크는 동기화된 WikiDocs page ID를 기준으로 `https://wikidocs.net/{page_id}` 형태를 사용한다. `TOC.md`는 GitHub 연동 구조를 위해 기존처럼 `pages/*.md` 링크를 유지한다.

권장 문구는 다음과 같다.

```markdown
다음 글에서는 [페이지 제목](파일명.md)에서 ...를 다룬다.

관련해서 [페이지 제목](파일명.md)을 함께 보면 ...의 기준을 잡기 쉽다.

실제 운영 흐름은 [페이지 제목](파일명.md)에서 더 구체적으로 볼 수 있다.
```

## 2. 장별 핵심 링크 축

## 2.1 1장: 왜 AI 챗봇이 아니라 AI 팀인가

1장은 책의 입구다. 독자가 AI 개인비서/AI 팀/Hermes Agent 운영 시스템/OpenClaw 전환을 순서대로 이해해야 한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `01-chapter-1.md` | `00-ai-chatbot-vs-ai-personal-assistant.md` |
| `00-ai-chatbot-vs-ai-personal-assistant.md` | `00-what-is-my-ai-team.md` |
| `00-what-is-my-ai-team.md` | `00-agent-team-reading-guide.md` |
| `00-agent-team-reading-guide.md` | `00-hermes-agent-core-concepts.md` |
| `00-hermes-agent-core-concepts.md` | `09-hermes-is-an-operating-system-not-just-a-chatbot.md` |
| `09-hermes-is-an-operating-system-not-just-a-chatbot.md` | `01-why-we-moved-from-openclaw-to-hermes.md` |
| `01-why-we-moved-from-openclaw-to-hermes.md` | `02-chapter-2.md` |

권장 개념 링크:

- `AI 개인비서` → `00-ai-chatbot-vs-ai-personal-assistant.md`
- `나만의 AI 팀` → `00-what-is-my-ai-team.md`
- `하비/방울이/뽀동이` → `00-agent-team-reading-guide.md`
- `Hermes Agent 주요 개념/기능` → `00-hermes-agent-core-concepts.md`
- `운영 시스템` → `09-hermes-is-an-operating-system-not-just-a-chatbot.md`
- `OpenClaw 전환` → `01-why-we-moved-from-openclaw-to-hermes.md`

## 2.2 2장: AI 개인비서 메인 창구 만들기

2장은 메인 창구/위임/병목/호출 규칙을 연결한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `02-chapter-2.md` | `02-why-harvey-is-the-front-door.md` |
| `02-why-harvey-is-the-front-door.md` | `18-when-harvey-should-handle-directly-vs-delegate.md` |
| `18-when-harvey-should-handle-directly-vs-delegate.md` | `22-why-harvey-main-window-is-powerful-and-where-it-bottlenecks.md` |
| `22-why-harvey-main-window-is-powerful-and-where-it-bottlenecks.md` | `03-chapter-3.md` |

권장 회귀 링크:

- 메인 창구가 시끄러워지는 문제 → `15-why-multibot-threads-get-noisy.md`
- 하비가 일을 나누는 실제 흐름 → `33-slack-thread-harvey-delegation-case.md`
- 역할형 에이전트 분리 기준 → `03-role-based-agent-splitting.md`

## 2.3 3장: 조사형/정리형/실행형 에이전트 나누기

3장은 역할형 에이전트의 핵심 장이다. 실패 패턴을 기준으로 링크를 촘촘히 둔다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `03-chapter-3.md` | `03-role-based-agent-splitting.md` |
| `03-role-based-agent-splitting.md` | `23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md` |
| `23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md` | `24-where-ppodongi-organization-agent-is-strong-and-where-it-wobbles.md` |
| `24-where-ppodongi-organization-agent-is-strong-and-where-it-wobbles.md` | `19-why-bangwooli-and-ppodongi-fail-differently.md` |
| `19-why-bangwooli-and-ppodongi-fail-differently.md` | `16-why-research-agents-rush-to-conclusions.md` |
| `16-why-research-agents-rush-to-conclusions.md` | `17-why-writing-agents-produce-polished-but-weak-drafts.md` |
| `17-why-writing-agents-produce-polished-but-weak-drafts.md` | `04-chapter-4.md` |

권장 케이스 링크:

- 방울이 확장조사/뽀동이 글쓰기 handoff → `31-bangwooli-ppodongi-content-handoff.md`
- Slack 스레드 업무 분배 → `33-slack-thread-harvey-delegation-case.md`
- 이미지형 에이전트 운영 → `28-how-image-agent-creates-wikidocs-visuals.md`

## 2.4 4장: 기억/컨텍스트/프로필 경계

4장은 오래 굴러가는 AI 팀의 기억 체계를 설명한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `04-chapter-4.md` | `03-why-same-harvey-feels-like-different-memory.md` |
| `03-why-same-harvey-feels-like-different-memory.md` | `04-why-tools-cannot-read-files-that-exist.md` |
| `04-why-tools-cannot-read-files-that-exist.md` | `13-what-to-do-with-legacy-while-keeping-current-truth-clear.md` |
| `13-what-to-do-with-legacy-while-keeping-current-truth-clear.md` | `26-why-agents-get-fuzzy-in-long-conversations-and-how-hermes-holds-up.md` |
| `26-why-agents-get-fuzzy-in-long-conversations-and-how-hermes-holds-up.md` | `05-chapter-5.md` |

권장 회귀 링크:

- source of truth 정리 → `13-what-to-do-with-legacy-while-keeping-current-truth-clear.md`
- 장기 운영에서 흐려지는 컨텍스트 → `26-why-agents-get-fuzzy-in-long-conversations-and-how-hermes-holds-up.md`
- 복구 순서와 연결 → `20-why-recovery-playbooks-are-about-order-not-just-docs.md`

## 2.5 5장: 외부 도구/MCP/자동화 운영

5장은 기능 목록이 아니라 자동화 운영 체계로 읽혀야 한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `05-chapter-5.md` | `05-why-google-workspace-integration-takes-longer-than-expected.md` |
| `05-why-google-workspace-integration-takes-longer-than-expected.md` | `10-when-and-how-to-manage-skills-in-hermes.md` |
| `10-when-and-how-to-manage-skills-in-hermes.md` | `12-always-on-gateway-is-more-confusing-than-it-looks.md` |
| `12-always-on-gateway-is-more-confusing-than-it-looks.md` | `05-daily-briefing-bot-workflow.md` |
| `05-daily-briefing-bot-workflow.md` | `06-chapter-6.md` |

권장 케이스 링크:

- 크론 조사에서 WikiDocs 발행까지 → `30-cron-research-to-agent-content-workflow.md`
- GitHub/WikiDocs 발행 흐름 → `32-github-wikidocs-content-publishing-workflow.md`
- 복구 플레이북 → `20-why-recovery-playbooks-are-about-order-not-just-docs.md`

## 2.6 6장: WikiDocs/블로그/강의 콘텐츠 시스템

6장은 대화와 조사 결과가 콘텐츠 자산으로 바뀌는 장이다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `06-chapter-6.md` | `11-how-to-use-obsidian-llm-wiki-in-real-operations.md` |
| `11-how-to-use-obsidian-llm-wiki-in-real-operations.md` | `07-why-we-write-the-wiki-first.md` |
| `07-why-we-write-the-wiki-first.md` | `08-why-good-research-does-not-automatically-become-a-good-blog.md` |
| `08-why-good-research-does-not-automatically-become-a-good-blog.md` | `28-how-image-agent-creates-wikidocs-visuals.md` |
| `28-how-image-agent-creates-wikidocs-visuals.md` | `07-chapter-7.md` |

권장 케이스 링크:

- 하망이 이미지 제작 케이스 → `29-hamangi-wikidocs-image-production-case.md`
- 방울이/뽀동이 콘텐츠 handoff → `31-bangwooli-ppodongi-content-handoff.md`
- GitHub/WikiDocs 발행 흐름 → `32-github-wikidocs-content-publishing-workflow.md`
- SEO/GEO 치트시트 자산화 → `34-hermes-seo-geo-cheatsheet-content-asset-case.md`

## 2.7 7장: 운영 FAQ/실패 패턴/멀티봇 규칙

7장은 운영 중 자주 생기는 질문을 분류하고 호출 규칙을 정리한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `07-chapter-7.md` | `14-most-common-hermes-ops-questions-and-how-to-think.md` |
| `14-most-common-hermes-ops-questions-and-how-to-think.md` | `15-why-multibot-threads-get-noisy.md` |
| `15-why-multibot-threads-get-noisy.md` | `08-chapter-8.md` |

권장 회귀 링크:

- 메인 창구 구조 → `02-why-harvey-is-the-front-door.md`
- Slack 스레드 분배 케이스 → `33-slack-thread-harvey-delegation-case.md`
- 조직 도입 실패 → `27-why-agent-adoption-fails-in-operations.md`

## 2.8 8장: 체크리스트/마이그레이션/복구 플레이북

8장은 시행착오를 다음 실행 기준으로 바꾸는 장이다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `08-chapter-8.md` | `06-how-to-turn-stumbles-into-checklists.md` |
| `06-how-to-turn-stumbles-into-checklists.md` | `20-why-recovery-playbooks-are-about-order-not-just-docs.md` |
| `20-why-recovery-playbooks-are-about-order-not-just-docs.md` | `21-how-to-make-hermes-operation-checklists-actually-useful.md` |
| `21-how-to-make-hermes-operation-checklists-actually-useful.md` | `25-why-openclaw-to-hermes-needed-a-migration-checklist.md` |
| `25-why-openclaw-to-hermes-needed-a-migration-checklist.md` | `09-chapter-9.md` |

권장 회귀 링크:

- OpenClaw 전환 이유 → `01-why-we-moved-from-openclaw-to-hermes.md`
- always-on gateway 혼선 → `12-always-on-gateway-is-more-confusing-than-it-looks.md`
- 현재 기준/source of truth → `13-what-to-do-with-legacy-while-keeping-current-truth-clear.md`

## 2.9 9장: 조직 도입과 AI 에이전트 운영 확장

9장은 개인 AI 팀이 조직 운영으로 확장될 때의 기준을 설명한다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `09-chapter-9.md` | `27-why-agent-adoption-fails-in-operations.md` |
| `27-why-agent-adoption-fails-in-operations.md` | `10-chapter-10.md` |

권장 회귀 링크:

- 메인 창구 구조 → `02-chapter-2.md`
- 역할형 에이전트 분리 → `03-chapter-3.md`
- 운영 FAQ/멀티봇 규칙 → `07-chapter-7.md`
- 체크리스트/복구 플레이북 → `08-chapter-8.md`

## 2.10 10장: Hermes Agent 실제 운영 케이스

10장은 앞 장의 개념을 실제 흐름으로 보여주는 장이다. 각 케이스에서 관련 개념 장으로 역링크를 둔다.

권장 순차 링크:

| 현재 페이지 | 다음 링크 |
|---|---|
| `10-chapter-10.md` | `29-hamangi-wikidocs-image-production-case.md` |
| `29-hamangi-wikidocs-image-production-case.md` | `30-cron-research-to-agent-content-workflow.md` |
| `30-cron-research-to-agent-content-workflow.md` | `31-bangwooli-ppodongi-content-handoff.md` |
| `31-bangwooli-ppodongi-content-handoff.md` | `32-github-wikidocs-content-publishing-workflow.md` |
| `32-github-wikidocs-content-publishing-workflow.md` | `33-slack-thread-harvey-delegation-case.md` |
| `33-slack-thread-harvey-delegation-case.md` | `34-hermes-seo-geo-cheatsheet-content-asset-case.md` |

권장 역링크:

| 케이스 | 연결할 개념 페이지 |
|---|---|
| 하망이 이미지 제작 | `28-how-image-agent-creates-wikidocs-visuals.md`, `08-why-good-research-does-not-automatically-become-a-good-blog.md` |
| 크론 조사 → WikiDocs 발행 | `05-daily-briefing-bot-workflow.md`, `10-when-and-how-to-manage-skills-in-hermes.md` |
| 방울이/뽀동이 handoff | `23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md`, `24-where-ppodongi-organization-agent-is-strong-and-where-it-wobbles.md` |
| GitHub/WikiDocs 발행 | `07-why-we-write-the-wiki-first.md`, `21-how-to-make-hermes-operation-checklists-actually-useful.md` |
| Slack 스레드 분배 | `02-why-harvey-is-the-front-door.md`, `15-why-multibot-threads-get-noisy.md` |
| SEO/GEO 치트시트 자산화 | `08-why-good-research-does-not-automatically-become-a-good-blog.md`, `32-github-wikidocs-content-publishing-workflow.md` |

## 3. 현재 우선 수정 대상

현재 audit 기준 내부 링크가 없는 페이지는 아래다.

```text
pages/01-chapter-1.md
pages/02-chapter-2.md
pages/03-chapter-3.md
pages/04-chapter-4.md
pages/05-chapter-5.md
pages/06-chapter-6.md
pages/07-chapter-7.md
pages/08-chapter-8.md
pages/09-chapter-9.md
pages/10-chapter-10.md
pages/34-hermes-seo-geo-cheatsheet-content-asset-case.md
pages/99-appendix.md
```

2차 Phase A에서는 TOC에 포함된 11개 페이지를 먼저 해결한다. `99-appendix.md`는 TOC 미포함 부록 후보이므로 별도 판단한다.

## 4. 완료 기준

Phase A 완료 기준:

```text
no_internal_md_links: 0 또는 TOC 포함 페이지 기준 0
missing_toc_links: 0
missing_images: 0
middle_dot: 0
```

부록 후보인 `99-appendix.md`는 다음 중 하나로 처리한다.

1. TOC에 공식 부록으로 포함하고 내부 링크를 연결한다.
2. 실제 본문 가치가 없으면 제거한다.
3. 후속 후보 자료라면 `drafts/`로 이동한다.

현재 권장안은 2차 내부 링크 보강 후 별도 판단이다.
