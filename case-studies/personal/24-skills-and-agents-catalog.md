# 24 · Catalogue: every skill and agent I keep, and why

Complement to [case 14](14-ai-workflow-tooling.md). Skills are kept in a private repo and linked
into each CLI by an installer; this is the full list, work-related or not.

## Skills I wrote for my own work
| Skill | Purpose | Case |
|---|---|---|
| `cbd-check` | Audit a quotation package, verdict + issue table | [18](../work/18-quotation-package-audit-skill.md) |
| `company-mail` | Read / summarise / reply company mail; backend first, browser fallback | [19](../work/19-mail-skills-with-guardrails.md) |
| `gci-send-mail` | Monthly all-department data request | [19](../work/19-mail-skills-with-guardrails.md) |
| `deliver-to-mis`, `kpi2026`, `minutemeeting`, `wi-generator` | Recurring delivery / reporting / documents | [23](../work/23-work-skills.md) |
| `delegate-to-opencode`, `delegate-to-codex` | Send read-only or bounded work to other models and **verify the result myself** | [14](14-ai-workflow-tooling.md) |
| `session-usage` | How much work went to which model this session | 14 |
| `handoff` | Compact a session into a document another agent can resume from | — |

## Engineering-practice skills (adapted from public skill sets)
| Skill | Use |
|---|---|
| `grill-me`, `grill-with-docs` | Interview me about a plan until the decision tree is resolved; the second also updates the glossary / ADRs |
| `tdd` | Red-green-refactor loop |
| `diagnose`, `qa` | Disciplined bug diagnosis; conversational bug reporting |
| `design-an-interface`, `prototype` | "Design it twice"; throwaway prototype to answer a design question |
| `improve-codebase-architecture`, `request-refactor-plan`, `zoom-out` | Find deepening opportunities; plan refactors as tiny commits; get the big picture |
| `to-prd`, `to-issues`, `triage` | Conversation → PRD → independently grabbable issues → state-machine triage |
| `ubiquitous-language` | Extract a domain glossary (same idea as case 20) |
| `review-2axis` | Review a change on two axes in parallel: repo standards and the original spec |
| `git-guardrails-claude-code`, `setup-pre-commit` | Hooks that block destructive git commands; pre-commit checks |
| `migrate-to-shoehorn`, `scaffold-exercises`, `setup-matt-pocock-skills` | Specific migrations / teaching scaffolds / repo conventions |

## Writing and communication skills
`exec-talk` (rewrite for management, answer first), `edit-article`, `writing-fragments`,
`writing-beats`, `writing-shape` (mine raw fragments → arrange as beats → shape an article),
`teach` (teach me a concept inside the workspace), `caveman` (very terse output mode).

## Plugin-provided
Debugging and review (`debug-mantra`, `post-mortem`, `scrutinize`), data (`duckdb-skills`: query
files with SQL, search past sessions), `dataviz`, `session-report`, `frontend-design`, `impeccable`.

## Custom sub-agents (12)
Domain: frontend, backend, excel-data, trading, automation-bot, windows-delivery.
Pipeline: requirements-analyst → solution-architect → database-architect → qa-engineer →
reviewer + security-reviewer. **No agent approves its own work**: the developer sets "ready",
QA says PASS/FAIL, the reviewer approves, the security reviewer clears. The pipeline is for risky
or cross-stack changes; small work skips it (see [how-i-work.md](../../how-i-work.md)).

## Rule that keeps this manageable
House rules are duplicated into every agent (a sub-agent starts with no memory), so changing a
rule means sweeping all copies and re-running the installer — otherwise a stale copy fails silently.
