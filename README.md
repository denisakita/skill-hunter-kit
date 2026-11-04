# Skill Hunter kit

Two pieces for Claude Code:

- **`skills/skill-hunter`**: a skill that finds the Claude skills and plugins a project needs,
  vets them for quality and safety, and installs the ones you approve.
- **`agents/ai-engineer.md`**: an AI engineer subagent that loads skill-hunter, equips itself
  with the right skills for the task, then builds and tests the feature (LLM features, RAG,
  agents, MCP servers, evals, and the normal code around them).

## Install

```bash
mkdir -p ~/.claude/skills ~/.claude/agents
```

```bash
cp -R skills/skill-hunter ~/.claude/skills/
```

```bash
cp agents/ai-engineer.md ~/.claude/agents/
```

Start a new Claude Code session so it picks both up. Needs `python3` (3.9+). The `gh` CLI,
logged in, gives higher GitHub rate limits and is required for `search --github`.

## Use it

In Claude Code:

- `/skill-hunter what skills does this repo need?`
- `/skill-hunter get me the full programming and AI engineering kit`
- `find me skills for a FastAPI + RAG project`
- `use the ai-engineer agent to add a RAG chatbot over docs/ with evals`

The script works on its own too:

```bash
python3 skills/skill-hunter/scripts/hunt.py detect ~/code/my-app
```

```bash
python3 skills/skill-hunter/scripts/hunt.py kit --areas testing,security,rag,evals
```

```bash
python3 skills/skill-hunter/scripts/hunt.py search mcp server
```

```bash
python3 skills/skill-hunter/scripts/hunt.py inspect anthropics/skills skills/mcp-builder/SKILL.md
```

| Command | What it does |
|---|---|
| `detect [DIR]` | reads manifests and folders, lists the skill areas the project needs |
| `installed [DIR]` | lists skills already installed (personal, project, plugins) |
| `catalog [--refresh]` | builds the index of ~1,400 skills and plugins (cached 7 days in `~/.cache/skill-hunter`) |
| `search TERM... [--github]` | ranks matching skills; `--github` adds GitHub-wide results |
| `kit [--areas a,b] [--top N]` | top candidates for every programming and AI-engineering area, with what you already have |
| `inspect REPO PATH [--file F]` | scans a skill's files (and its plugin's agents, commands, hooks, MCP config) for risky lines, prints its SKILL.md; `--file` prints one file in full |
| `install REPO PATH --dest DIR` | copies a vetted skill folder, pinned to a commit; never runs it |

## Where it hunts

| Tier | Sources |
|---|---|
| 1 | `anthropics/skills`, `anthropics/claude-plugins-official` |
| 2 | third-party plugins in the official directory, `obra/superpowers`, `wshobson/agents`, `ComposioHQ/awesome-claude-skills` |
| 3 | GitHub code search for `SKILL.md` (20+ stars, not archived) |

Add your own sources as tier 2 with `SKILL_HUNTER_SOURCES="owner/repo,owner/repo"`.

## License

MIT, see [LICENSE](LICENSE).
