# 🏗️ Agent Architect Skill

An [Agent Skills](https://agentskills.io/) compatible skill for designing and recommending AI agent architecture components.

Works with **Kimi CLI**, **Claude Code**, **OpenCode**, **OpenClaw**, and any other Agent Skills-compatible tool.

---

## ✨ What it does

This skill helps you design the right agent architecture by recommending components based on your needs.

### Two operating modes:

1. **Direct Scenario Analysis** — Describe your use case, get a recommended component list
2. **Guided Discovery** — Answer 5 structured questions to discover your requirements

## 🚀 Installation

### Project-level (recommended)

```bash
# Clone into your project's skills directory
git clone https://github.com/tiw/agent-architect-skill.git .claude/skills/agent-architect
```

Or copy the files manually to `.claude/skills/agent-architect/` in your project root.

### User-level (global)

```bash
git clone https://github.com/tiw/agent-architect-skill.git ~/.claude/skills/agent-architect
```

## 📖 Usage

Once installed, simply describe your agent use case:

> "I want to build an agent that automatically checks server health and sends Slack alerts when issues are found"

Or enter guided mode by saying:

> "Help me figure out what components I need for my agent"

## 📋 Output includes

- **Required components** (MVP level)
- **Strongly recommended** (Production level)
- **Optional enhancements** (Advanced level)
- **Architecture pattern** recommendation
- **Framework suggestions** (LangGraph / OpenCode / OpenClaw / etc.)
- **Anti-pattern warnings**
- **Evolution roadmap** (Level 1 → 2 → 3)

## 📁 Files

| File | Description |
|------|-------------|
| `SKILL.md` | Core skill instructions — the file the AI reads |
| `REFERENCE.md` | Detailed knowledge base — component catalog, framework comparison, decision guide |

## 🧠 Based on research of

- LangChain / LangGraph
- OpenCode
- OpenClaw
- Claude Code
- CrewAI
- AutoGen
- MCP & A2A protocols

## License

MIT
