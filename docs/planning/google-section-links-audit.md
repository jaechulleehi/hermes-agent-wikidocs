# Google 검색결과 페이지 섹션 링크 전수 점검

## 결론

- 구글 검색결과의 작은 버튼형 링크는 구조화 데이터 리치 리절트보다 페이지 내 섹션 링크/사이트링크에 가깝다.
- WikiDocs HTML에서 `[TOC]`는 `<div class="toc"><a href="#...">...</a></div>`와 본문 헤딩 id로 변환되어 구글이 섹션 구조를 이해하기 쉽다.
- 이번 점검에서는 TOC.md에 등록된 모든 pages 문서에 `[TOC]`가 없었고, 전체 페이지에 도입부 뒤 `[TOC]`를 추가했다.

## 적용 기준

1. 첫 답변/도입부는 그대로 둔다.
2. 두 번째 `##` 헤딩이 시작되기 직전에 `[TOC]`를 넣는다.
3. 본문 H1은 사용하지 않는다.
4. 검색결과에 노출되면 좋은 문구는 `##`/`###` 헤딩으로 직접 쓴다.

## 수정 결과

- 대상 페이지: 103개
- `[TOC]` 추가 페이지: 103개
- 기존 `[TOC]` 보유 페이지: 0개

## `[TOC]` 추가 파일

- `pages/00-chapter-0.md`
- `pages/00-hermes-agent-core-concepts.md`
- `pages/00-hermes-agent-official-github-docs.md`
- `pages/00-hermes-agent-install-setup.md`
- `pages/00-hermes-agent-cli-first-chat.md`
- `pages/00-hermes-agent-provider-model-config.md`
- `pages/01-why-we-moved-from-openclaw-to-hermes.md`
- `pages/00-hermes-agent-vs-claude-code-codex.md`
- `pages/00-hermes-agent-docker-gateway.md`
- `pages/00-hermes-agent-channels.md`
- `pages/00-hermes-agent-update-validation.md`
- `pages/00-hermes-agent-mac-mini-always-on.md`
- `pages/01-chapter-1.md`
- `pages/00-ai-chatbot-vs-ai-personal-assistant.md`
- `pages/00-what-is-my-ai-team.md`
- `pages/00-agent-team-reading-guide.md`
- `pages/09-hermes-is-an-operating-system-not-just-a-chatbot.md`
- `pages/02-chapter-2.md`
- `pages/02-why-harvey-is-the-front-door.md`
- `pages/18-when-harvey-should-handle-directly-vs-delegate.md`
- `pages/22-why-harvey-main-window-is-powerful-and-where-it-bottlenecks.md`
- `pages/03-chapter-3.md`
- `pages/03-role-based-agent-splitting.md`
- `pages/23-where-bangwooli-research-agent-is-strong-and-where-it-wobbles.md`
- `pages/24-where-ppodongi-organization-agent-is-strong-and-where-it-wobbles.md`
- `pages/19-why-bangwooli-and-ppodongi-fail-differently.md`
- `pages/16-why-research-agents-rush-to-conclusions.md`
- `pages/17-why-writing-agents-produce-polished-but-weak-drafts.md`
- `pages/04-chapter-4.md`
- `pages/04-01-what-should-ai-assistant-remember.md`
- `pages/04-02-session-memory-profile-boundary.md`
- `pages/04-03-agents-user-md-memory-role.md`
- `pages/04-04-shared-memory-team-memory.md`
- `pages/04-05-obsidian-llm-wiki-external-memory.md`
- `pages/04-06-openclaw-memory-migration.md`
- `pages/04-07-openviking-rag-memory-layer.md`
- `pages/04-08-context-compaction-handoff.md`
- `pages/04-09-harvey-memory-orchestrator.md`
- `pages/04-10-soul-md-agent-personality.md`
- `pages/05-chapter-5.md`
- `pages/05-why-google-workspace-integration-takes-longer-than-expected.md`
- `pages/12-always-on-gateway-is-more-confusing-than-it-looks.md`
- `pages/05-daily-briefing-bot-workflow.md`
- `pages/35-four-ways-to-assign-work-in-hermes-agent.md`
- `pages/05-hermes-agent-mcp-external-tools.md`
- `pages/05-github-cli-mcp-api.md`
- `pages/05-notion-google-workspace-wikidocs-integration.md`
- `pages/05-hermes-agent-built-in-tools-toolsets.md`
- `pages/05-hermes-agent-terminal-backend-risk.md`
- `pages/05-hermes-agent-tool-result-verification.md`
- `pages/06-skills-chapter.md`
- `pages/10-when-and-how-to-manage-skills-in-hermes.md`
- `pages/06-02-repeat-work-to-skill.md`
- `pages/06-03-role-agent-skills.md`
- `pages/06-04-skill-patching-qa.md`
- `pages/06-05-public-internal-github-skills.md`
- `pages/06-06-skill-vs-memory-mcp-cron-gateway.md`
- `pages/06-chapter-6.md`
- `pages/07-why-we-write-the-wiki-first.md`
- `pages/08-why-good-research-does-not-automatically-become-a-good-blog.md`
- `pages/28-how-image-agent-creates-wikidocs-visuals.md`
- `pages/07-chapter-7.md`
- `pages/14-most-common-hermes-ops-questions-and-how-to-think.md`
- `pages/15-why-multibot-threads-get-noisy.md`
- `pages/08-chapter-8.md`
- `pages/06-how-to-turn-stumbles-into-checklists.md`
- `pages/20-why-recovery-playbooks-are-about-order-not-just-docs.md`
- `pages/21-how-to-make-hermes-operation-checklists-actually-useful.md`
- `pages/09-hermes-agent-security-checklist.md`
- `pages/09-hermes-agent-command-approval-yolo-allowlist.md`
- `pages/09-hermes-agent-gateway-permission-sandbox-isolation.md`
- `pages/09-hermes-agent-checkpoint-rollback-recovery.md`
- `pages/25-why-openclaw-to-hermes-needed-a-migration-checklist.md`
- `pages/09-chapter-9.md`
- `pages/27-why-agent-adoption-fails-in-operations.md`
- `pages/10-chapter-10.md`
- `pages/29-hamangi-wikidocs-image-production-case.md`
- `pages/30-cron-research-to-agent-content-workflow.md`
- `pages/31-bangwooli-ppodongi-content-handoff.md`
- `pages/32-github-wikidocs-content-publishing-workflow.md`
- `pages/33-slack-thread-harvey-delegation-case.md`
- `pages/34-hermes-seo-geo-cheatsheet-content-asset-case.md`
- `pages/36-repeat-document-production-agent-workflow.md`
- `pages/37-email-triage-reply-draft-agent-workflow.md`
- `pages/38-research-to-comparison-decision-memo-workflow.md`
- `pages/40-daily-briefing-weekly-report-cron-workflow.md`
- `pages/41-bibungi-product-requirements-agent-workflow.md`
- `pages/42-bonggu-execution-deployment-verification-workflow.md`
- `pages/43-files-notes-long-term-memory-workflow.md`
- `pages/44-meeting-notes-to-report-slide-workflow.md`
- `pages/45-image-generation-review-reuse-workflow.md`
- `pages/46-google-workspace-email-mcp-workflow.md`
- `pages/47-slack-customer-support-draft-approval-workflow.md`
- `pages/48-blog-newsletter-content-repurposing-workflow.md`
- `pages/49-recurring-research-to-decision-meeting-workflow.md`
- `pages/50-cron-error-skill-memory-improvement-case.md`
- `pages/51-meeting-email-to-sdr-pipeline-workflow.md`
- `pages/a0-hermes-agent-appendix-tools.md`
- `pages/a1-hermes-agent-glossary.md`
- `pages/a2-hermes-agent-security-checklist.md`
- `pages/a3-ai-team-design-worksheet.md`
- `pages/a4-official-docs-alignment-checklist.md`
- `pages/a5-practical-automation-templates.md`

