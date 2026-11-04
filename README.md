# Skill Hunter kit

A Claude Code plugin with three pieces:

- **`skill-hunter`** (skill): finds the Claude skills and plugins a project or role needs,
  vets them for quality and safety, and installs only the ones you approve.
- **`product-engineer`** (skill): studies your app, comes up with ideas that make the user
  experience better (fixes for friction and new features), ranks them by impact and effort,
  and builds working prototypes of the best ones.
- **`ai-engineer`** (agent): equips itself with the right skills for the task, then builds
  and tests LLM features, RAG, agents, MCP servers and evals, plus the frontend and backend
  around them.

## Install

In Claude Code, on any computer:

```
/plugin install skill-hunter --marketplace denisakita/skill-hunter-kit
```

Or in two steps (older Claude Code versions):

```
/plugin marketplace add denisakita/skill-hunter-kit
/plugin install skill-hunter@skill-hunter-kit
```

The repository is private, so the computer needs a GitHub login that can read it:
`gh auth login`, or an SSH key that works without a passphrase prompt. Claude Code uses the
git credentials already on the machine and never asks for a token.

Once installed, the pieces are named `skill-hunter:skill-hunter`,
`skill-hunter:product-engineer` and `skill-hunter:ai-engineer`. Needs `python3` (3.9+). A
logged-in `gh` CLI gives higher GitHub rate limits and is required for `search --github`.

## Use it

- `/skill-hunter:skill-hunter what skills does this repo need?`
- `/skill-hunter:skill-hunter find skills for my role: product engineer`
- `/skill-hunter:product-engineer give me ideas to improve the onboarding, then prototype the best one`
- `make the checkout flow less confusing and show me a prototype` (product-engineer picks this up)
- `use the ai-engineer agent to add a RAG chatbot over docs/ with evals`

## Safety

Three rules, which the skills also repeat every time they show you a shortlist:

1. **Install from tier 1 (Anthropic) and tier 2 (big, well-known collections). Be careful
   with tier 3 (random GitHub repos).**
2. **Read the "Vetting" column before you say yes.**
3. **Don't use "bypass permissions" mode while installing skills.**

The install command enforces them. It refuses, and says why, when:

| It refuses when... | To go ahead anyway |
|---|---|
| the skill wasn't inspected in the last 24 hours | run `inspect` first |
| the repo changed after you inspected it | nothing: it installs exactly the commit you inspected |
| the skill is tier 3 | `--allow-tier3`, only after you said yes to that specific skill |
| inspection found red flags | `--accept-flags`, only after each flag was explained and you accepted it |
| a Claude settings file sets `defaultMode` to `bypassPermissions` | switch back to the default mode |

It never runs what it downloads, saves every file non-executable, refuses paths that escape
the skill folder, and records where each skill came from in `.skill-hunter.json`.

What it can't do: the risky-pattern scan matches known patterns and can miss a clever trick,
so Claude also reads every script, hook and plugin component before recommending a skill.
Plugins you install with `/plugin` can bring agents, commands and hooks that run
automatically; `inspect` lists them so you know before you install.

## Roles

`kit --role` hunts the areas a role needs and shows, for each one, what you already have
and the best tier 1-2 candidates:

| Role | Areas |
|---|---|
| `product-engineer` | prototyping, UX/UI design, product discovery, design systems, accessibility, user research, prioritization, frontend, analytics and experiments, testing, code review, git |
| `designer` | UX/UI design, design systems, prototyping, accessibility, feedback, user research, frontend, product discovery |
| `ai-engineer` | Claude API, prompt engineering, agents, RAG, MCP, evals, LLM ops, API and backend, frontend, database, testing, debugging, security |

## The hunt script

The skill runs `skills/skill-hunter/scripts/hunt.py`; you can also run it yourself:

```bash
python3 skills/skill-hunter/scripts/hunt.py kit --role product-engineer
```

```bash
python3 skills/skill-hunter/scripts/hunt.py detect ~/code/my-app
```

```bash
python3 skills/skill-hunter/scripts/hunt.py inspect anthropics/skills skills/frontend-design/SKILL.md
```

| Command | What it does |
|---|---|
| `detect [DIR]` | reads manifests and folders, lists the skill areas the project needs and stack-specific searches |
| `installed [DIR]` | lists skills already installed (personal, project, plugins) |
| `catalog [--refresh]` | builds the index of ~1,450 skills and plugins (cached 7 days in `~/.cache/skill-hunter`) |
| `search TERM... [--github]` | ranks matching skills; `--github` adds GitHub-wide results (tier 3) |
| `kit [--role R \| --areas a,b]` | top candidates per area, with what you already have |
| `inspect REPO PATH [--file F]` | scans a skill's files (and its plugin's agents, commands, hooks, MCP config) for risky lines, prints its SKILL.md, records it as vetted; `--file` prints one file in full |
| `install REPO PATH --dest DIR` | copies the exact version you inspected, after the safety checks; never runs it |

## Where it hunts

| Tier | Sources |
|---|---|
| 1 | `anthropics/skills`, `anthropics/claude-plugins-official` |
| 2 | third-party plugins in the official directory, `obra/superpowers`, `wshobson/agents`, `ComposioHQ/awesome-claude-skills`, `deanpeters/Product-Manager-Skills`, `nextlevelbuilder/ui-ux-pro-max-skill` |
| 3 | GitHub code search for `SKILL.md` (20+ stars, not archived); Composio's app connectors |

Add your own sources as tier 2 with `SKILL_HUNTER_SOURCES="owner/repo,owner/repo"`.

## Without the plugin

To use it on one computer without the plugin system, copy the folders instead:

```bash
mkdir -p ~/.claude/skills ~/.claude/agents && cp -R skills/* ~/.claude/skills/ && cp agents/*.md ~/.claude/agents/
```

## License

MIT, see [LICENSE](LICENSE).
