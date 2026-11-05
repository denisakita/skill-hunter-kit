# Vetted skills

Skills the hunter found and inspected, for reference. Every row was vetted with
`hunt.py inspect` on the date and commit shown. Skills change: inspect again before you install
one (`install` refuses an inspection older than 24 hours anyway).

**Safety rules**

1. Install from tier 1 (Anthropic) and tier 2 (big, well-known collections). Be careful with tier 3.
2. Read the Vetting column before you say yes.
3. Don't use "bypass permissions" mode while installing skills.

**How to read the tables**

- **Tier**: 1 = written by Anthropic; 2 = large, active community collection; 3 = anything else.
- **Vetting**: files in the skill, red flags found (`!` serious, `~` informational) and what else
  its plugin installs. "No hooks or MCP" means nothing runs by itself and nothing connects to an
  outside service.
- **Install**: add the marketplace once, then install the plugin:
  `/plugin marketplace add <repo>` then `/plugin install <plugin>@<marketplace>`.
- **Verdict**: ✅ recommended · ⚠️ recommended with a warning · ❌ not recommended · 🔍 found, not
  inspected yet.

## Safest picks

These pass every check: tier 1 or 2, **text only** (instructions Claude reads; no scripts, so
nothing in them can run), 0 serious flags, and their plugin has **no hooks** (nothing runs by
itself) and **no MCP servers** (nothing connects to an outside service). File lists checked on
Nov 5 at the commits below.

| Skill | Tier | Text only | Hooks | MCP | Serious flags |
|---|---|---|---|---|---|
| frontend-design | 1 (Anthropic) | yes, 2 files | none | none | 0 |
| playground | 1 (Anthropic) | yes, 7 files | none | none | 0 |
| interaction-design | 2 | yes, 4 files | none | none | 0 |
| accessibility-compliance | 2 | yes, 5 files | none | none | 0 |
| tailwind-design-system | 2 | yes, 3 files | none | none | 0 (1 harmless word match) |
| postgresql-table-design | 2 | yes, 2 files | none | none | 0 |
| rag-implementation, embedding-strategies | 2 | yes, 2 files each | none | none | 0 |
| problem-statement, jobs-to-be-done, customer-journey-map, user-story-mapping, storyboard, pol-probe, prd-development | 2 | yes, 4 files each | none | none | 0 |

`mcp-builder` (tier 1) is safe too but **not** text only: it ships 2 Python scripts
(`scripts/connections.py`, `scripts/evaluation.py`). Read them before letting Claude run them.

A plugin can also add agents and slash commands. Those are text as well (prompts), but each one
is something more that can trigger, so install only the plugins you need.

## Understand the problem and the user

Use these before designing a feature, so you build the right thing.

| Skill | What it helps with | Source · tier · ★ | Install | Vetting | Verdict |
|---|---|---|---|---|---|
| problem-statement | Write who is blocked, what they are trying to do, why it matters and how it feels | deanpeters/Product-Manager-Skills · 2 · 7.9k | `problem-statement@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (license not recognised by GitHub, check before work use) |
| jobs-to-be-done | Find the user's real jobs, pains and gains (functional, social, emotional) | deanpeters/Product-Manager-Skills · 2 · 7.9k | `jobs-to-be-done@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (same license note) |
| customer-journey-map | Map stages, touchpoints, actions and emotions; find where the experience breaks | deanpeters/Product-Manager-Skills · 2 · 7.9k | `customer-journey-map@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (same license note) |
| user-story-mapping | Lay out the user's flow as activities, steps and tasks, then slice an MVP | deanpeters/Product-Manager-Skills · 2 · 7.9k | `user-story-mapping@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (same license note) |
| storyboard | A six-frame story of the user going from problem to solution, for reviews and demos | deanpeters/Product-Manager-Skills · 2 · 7.9k | `storyboard@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (same license note) |
| opportunity-solution-tree | From an outcome to opportunities, solutions and tests | deanpeters/Product-Manager-Skills · 2 · 7.9k | `opportunity-solution-tree@pm-skills` | not inspected | 🔍 |
| problem-framing-canvas | A clearer problem statement before jumping to solutions | deanpeters/Product-Manager-Skills · 2 · 7.9k | `problem-framing-canvas@pm-skills` | not inspected | 🔍 |
| lean-ux-canvas | Frame the problem, surface assumptions, decide what to learn next | deanpeters/Product-Manager-Skills · 2 · 7.9k | `lean-ux-canvas@pm-skills` | not inspected | 🔍 |

Marketplace for all of these: `/plugin marketplace add deanpeters/Product-Manager-Skills`. Each
skill is its own plugin, so you install only the ones you want.

## Design and prototype

| Skill | What it helps with | Source · tier · ★ | Install | Vetting | Verdict |
|---|---|---|---|---|---|
| frontend-design | Visual direction, typography, and a plan, review, build, critique process that avoids template-looking UI | anthropics/skills · 1 · 187k | `example-skills@anthropic-agent-skills` | Nov 5, `ca45fc496a34`: 2 files, 0 flags, no hooks or MCP. The plugin also installs 18 other example skills | ✅ |
| playground | Clickable single-file HTML prototypes with controls and a live preview | anthropics/claude-plugins-official · 1 · 46k | `playground@claude-plugins-official` | Nov 5, `fa4eb54397ee`: 7 files, 0 flags, no hooks or MCP. Apache-2.0 | ✅ |
| interaction-design | Microinteractions, motion and timing, loading states, transitions, feedback patterns | wshobson/agents · 2 · 42k | `ui-design@claude-code-workflows` | Nov 5, `7e2fb559cf4b`: 4 files, 0 flags. The plugin adds 3 agents, 4 commands and 8 other skills; no hooks or MCP. MIT | ✅ |
| accessibility-compliance | WCAG 2.2, ARIA patterns, inclusive design | wshobson/agents · 2 · 42k | `ui-design@claude-code-workflows` (same plugin) | Nov 5, `7e2fb559cf4b`: 5 files, 0 flags | ✅ |
| design-system-patterns | Design tokens, theming, component architecture | wshobson/agents · 2 · 42k | `ui-design@claude-code-workflows` (same plugin) | its plugin's parts were scanned clean; this skill's own files not inspected | 🔍 |
| tailwind-design-system | Design systems with Tailwind CSS v4 and tokens, framework-neutral | wshobson/agents · 2 · 42k | `frontend-mobile-development@claude-code-workflows` | Nov 5, `7e2fb559cf4b`: 3 files; 1 flag, harmless (the word "keychain" in a list of mobile features). The plugin adds 2 agents, 1 command, 3 React skills | ✅ |
| pol-probe | A "proof of life" probe: the cheapest throwaway test of one risky assumption before building | deanpeters/Product-Manager-Skills · 2 · 7.9k | `pol-probe@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags, no hooks or MCP | ✅ (same license note) |
| prd-development | A structured PRD from problem framing to success criteria | deanpeters/Product-Manager-Skills · 2 · 7.9k | `prd-development@pm-skills` | Nov 5, `cfcb697f14b8`: 4 files, 0 flags. Refers to files outside its folder: install the plugin, don't copy the folder | ✅ (same license note) |

Also in this kit: the **product-engineer** skill (UX checklist, ranked ideas, prototypes in the
real stack) and Claude's **Design** canvas for mockups.

## Build and data

| Skill | What it helps with | Source · tier · ★ | Install | Vetting | Verdict |
|---|---|---|---|---|---|
| postgresql-table-design | PostgreSQL schemas: types (including money as numeric), indexes, constraints | wshobson/agents · 2 · 42k | `database-design@claude-code-workflows` | Nov 5, `7e2fb559cf4b`: 2 files, 0 flags. The plugin adds 2 agents; no hooks or MCP | ✅ |

## AI engineering

| Skill | What it helps with | Source · tier · ★ | Install | Vetting | Verdict |
|---|---|---|---|---|---|
| rag-implementation | Retrieval strategies (dense, sparse, hybrid), reranking, a worked RAG pipeline | wshobson/agents · 2 · 42k | `llm-application-dev@claude-code-workflows` | Nov 4, `7e2fb559cf4b`: 2 files, 0 flags. The plugin adds 3 agents, 3 commands and 7 skills; 1 informational flag (`pip install` in a Dockerfile example); no hooks or MCP | ✅ |
| embedding-strategies | Choosing embedding models and chunking | wshobson/agents · 2 · 42k | `llm-application-dev@claude-code-workflows` (same plugin) | Nov 4: 2 files, 0 flags | ✅ |
| mcp-builder | Designing and building MCP servers in Python or TypeScript | anthropics/skills · 1 · 187k | `example-skills@anthropic-agent-skills` | Nov 4, `ca45fc496a34`: 11 files, 0 serious flags; informational: `pip install` steps and example URLs in its evaluation script | ✅ |

## Not recommended

| Skill | Source · tier | Why not |
|---|---|---|
| superpowers (whole plugin) | obra/superpowers · 2 · 312k · MIT | Nov 5, `22f0b463e7a5`. **Safe, but not a fit.** Its session-start hook was read in full: it only reads the plugin's own `using-superpowers` skill and adds that text (about 3 KB) to every session, marked "EXTREMELY_IMPORTANT", so every session follows its workflow (brainstorm, plan, test-first, review). No network, no file writes, no MCP, no agents or commands. Its 15 skills overlap the amf skills (debugging, testing, verification, code review) and would compete with them. Scripts only in brainstorming (a local web server), systematic-debugging (an example script) and writing-skills. |
| impeccable (plugin) | pbakaus/impeccable · 3 (not an indexed source) · 103k · Apache-2.0 | Nov 5, `5af4319c4e7a`. Well made and open about its risks, but it fails the safety rules. Its **hooks run a compiled engine at session start, after every Edit or Write, and when Claude stops**. On first run the launcher **downloads that binary** from the project's GitHub releases into `~/.impeccable/bin/` (SHA-256 checked, fails closed). Its own docs say the hooks "run outside model approval", even when you deny commands, and that the engine runs "unsandboxed with the agent's environment and network access". It fetches design directions from `impeccable.style` and has anonymous telemetry. Bundles 4 agents and 12 scripts (1.75 MB; two too large to scan; minified code with base64 and eval). The design guidance in `reference/*.md` is plain text and can be read without installing anything. |
| brainstorming | obra/superpowers · 2 · 312k | Nov 5, `22f0b463e7a5`: 12 flags. Nothing malicious found: they come from a **local web server** it starts on your Mac (its own random access token), a shell command it runs to open the browser, and a logo loaded from `primeradiant.com`. Installing the `superpowers` plugin also adds 14 skills and a **hook that runs at every session start**. Too much running in the background, and the Design canvas already covers visual mockups. |
| ui-ux-pro-max (plugin) | nextlevelbuilder/ui-ux-pro-max-skill · 2 · 145k | Nov 5, `ccce51cba388`. The `design` skill it bundles has 26 flags: it reads `GEMINI_API_KEY`, `MUAPI_API_KEY` and `ATLASCLOUD_API_KEY` and calls `api.muapi.ai`, `api.atlascloud.ai` and Google's AI service. The `ui-ux-pro-max` skill itself is huge (74 files, 3.5 MB, 5 Python scripts; its 4 flags are in tests). |
| Docker Hub Automation | ComposioHQ/awesome-claude-skills · 3 | Tier 3, and it works through Composio's hosted servers. |
| angular-migration | wshobson/agents · 2 | For moving off the old AngularJS, not for current Angular. |

## Gaps

No tier 1 or 2 skill was found for usability heuristics, information architecture or UX writing
(microcopy). The product-engineer skill's UX checklist covers the basics of the first two.

## Using them for a new feature

One path from idea to something the team can try, whatever the feature is:

1. **problem-statement**: who is blocked and why it matters, in a few lines.
2. **jobs-to-be-done**: what the user is really trying to get done.
3. **user-story-mapping** or **customer-journey-map**: the flow, step by step, and the smallest
   slice worth building.
4. **storyboard**: six frames you can show the team before any design.
5. **product-engineer** with **frontend-design**, **interaction-design** and
   **accessibility-compliance**: mockups on the Design canvas, then a prototype in code.
6. **pol-probe**: when one assumption is risky, test just that, cheaply, first.
