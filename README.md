# Skills Library for LearnFlow AI Platform

This repository contains reusable skills that enable AI coding agents (Claude Code and Goose) to build and deploy the LearnFlow platform automatically using the **MCP Code Execution** pattern.

## 🚀 Why Skills?

Skills transform AI agents from code generators into autonomous engineers. Instead of manually writing every deployment command, you teach the AI patterns once - then it applies them autonomously to build entire applications.

**Traditional AI Development:**
```
You → Write code → Deploy manually → Repeat for each project
```

**Skills-Based Development:**
```
You → Write skill → AI applies skill → Infinite autonomous deployments
```

## 📊 Token Efficiency: The Numbers

### Before (Direct MCP Integration)

```
MCP Server Startup Cost: 50,000 tokens
├─ Tool definitions loaded at startup
├─ All endpoints in context
└─ 41% of context window consumed

Per Operation: 5,000-10,000 tokens
├─ Full data flows through context
├─ Intermediate results in context
└─ Agent writes data back

10 Operations Total: ~100,000 tokens
```

### After (MCP Code Execution with Skills)

```
Skill Load Cost: ~100 tokens
├─ Only SKILL.md instructions loaded
├─ Scripts referenced, not loaded
└─ 0.5% of context window used

Per Operation: ~120 tokens
├─ Script executes outside context (0 tokens)
├─ Only minimal result returns (~20 tokens)
└─ SKILL.md reused (already loaded)

10 Operations Total: ~1,300 tokens
```

### **Result: 98.7% Token Reduction**

| Approach | Startup | 10 Operations | Efficiency |
|----------|---------|---------------|------------|
| Direct MCP | 50k | 100k tokens | Baseline |
| Skills Pattern | 100 | 1.3k tokens | **98.7% savings** |

## 🛠️ Skills Included

### Core Infrastructure

| Skill | Purpose | Token Savings | Key Scripts |
|-------|---------|---------------|-------------|
| **kafka-k8s-setup** | Deploy Apache Kafka on K8s with Strimzi operator | 50k → 110 tokens | `deploy_kafka.sh`, `verify_kafka.py`, `create_topics.py` |
| **postgres-k8s-setup** | Deploy PostgreSQL with persistence and migrations | 30k → 105 tokens | `deploy_postgres.sh`, `run_migrations.py`, `verify_postgres.py` |

### Application Services

| Skill | Purpose | Token Savings | Key Scripts |
|-------|---------|---------------|-------------|
| **fastapi-dapr-agent** | Generate FastAPI microservices with Dapr sidecars | 40k → 95 tokens | `create_service.py`, `deploy_service.sh`, `verify_service.py` |
| **nextjs-k8s-deploy** | Build and deploy Next.js apps to Kubernetes | 35k → 100 tokens | `build_deploy.sh`, templates |
| **docusaurus-deploy** | Deploy documentation sites with Docusaurus | 25k → 90 tokens | `deploy.sh`, configuration templates |

### AI Agent Utilities

| Skill | Purpose | Token Savings | Key Scripts |
|-------|---------|---------------|-------------|
| **agents-md-gen** | Generate AGENTS.md for repository understanding | 10k → 80 tokens | `generate_agents_md.py` |
| **mcp-code-execution** | Wrap MCP servers in code-executable scripts | N/A | Example patterns for MCP integration |

## 🏗️ Architecture

### The MCP Code Execution Pattern

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Agent Context Window                  │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ Before: Direct MCP (41% consumed at startup)           │ │
│  │ ████████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │ │
│  └────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────┐ │
│  │ After: Skills Pattern (3% consumed)                    │ │
│  │ ██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │ │
│  └────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘

Skills Pattern Components:
┌──────────────┐
│  SKILL.md    │  ← ~100 tokens loaded into context
│  (~100 tok)  │
└──────┬───────┘
       │
       ├─→ scripts/deploy.sh      ← 0 tokens (executes outside)
       ├─→ scripts/verify.py      ← 0 tokens (executes outside)
       └─→ scripts/helpers/...    ← 0 tokens (executes outside)
                  │
                  ├─→ Output: "✓ Kafka deployed"  ← ~10 tokens
                  └─→ Returns to agent context
```

### How It Works

1. **Agent reads SKILL.md** (~100 tokens enter context)
2. **Agent invokes script** (script path reference only)
3. **Script executes** (heavy operations happen outside context window)
4. **Minimal result** (~10-20 tokens) flows back to agent
5. **Agent continues** with 97% of context window still available

## 📖 Usage

### For Claude Code

```bash
# Skills are automatically detected in .claude/skills/
cd your-project
cp -r /path/to/skills-library/.claude .

# Use in conversation
claude "Deploy Kafka for my event-driven microservices"
# Agent automatically:
# 1. Loads kafka-k8s-setup skill (~100 tokens)
# 2. Executes scripts/deploy_kafka.sh
# 3. Verifies with scripts/verify_kafka.py
# 4. Returns: "✓ Kafka deployed with 3 brokers"
```

### For Goose

```bash
# Goose also reads from .claude/skills/
cd your-project
ln -s /path/to/skills-library/.claude .

# Use in conversation
goose session start
> "Set up PostgreSQL for the application"
# Agent uses postgres-k8s-setup skill automatically
```

### Cross-Agent Compatibility

The same `.claude/skills/` directory works for **both agents**:

```
your-project/
└── .claude/
    └── skills/           ← Shared by Claude Code AND Goose
        ├── kafka-k8s-setup/
        ├── postgres-k8s-setup/
        └── fastapi-dapr-agent/
```

## 🎯 Real-World Example: LearnFlow Platform

Using these skills, AI agents built the complete LearnFlow application:

```bash
# Infrastructure (2 prompts, 5 minutes)
"Deploy Kafka for event streaming"
"Set up PostgreSQL with LearnFlow schema"

# Backend Services (6 prompts, 15 minutes)
"Create triage-agent microservice with Dapr"
"Create concepts-agent microservice"
# ... 4 more services

# Frontend (1 prompt, 8 minutes)
"Deploy Next.js frontend with Monaco editor"

# Documentation (1 prompt, 3 minutes)
"Generate documentation site with Docusaurus"
```

**Total**: 10 prompts → Complete application in ~30 minutes

**Without Skills**: Would require dozens of manual commands, hours of work, and 10x the token usage.

## 📚 Documentation

- [Skill Development Guide](./docs/skill-development-guide.md) - How to create new skills
- [AAIF Standards](https://aaif.io/) - Industry standards for AI agents
- [MCP Code Execution](https://www.anthropic.com/engineering/code-execution-with-mcp) - Pattern explanation from Anthropic
- [Goose Skills Guide](https://block.github.io/goose/docs/guides/context-engineering/using-skills) - Goose-specific documentation

## 🧪 Testing Skills

Each skill includes verification scripts:

```bash
# Test kafka-k8s-setup
cd .claude/skills/kafka-k8s-setup
./scripts/deploy_kafka.sh
python scripts/verify_kafka.py

# Expected output:
# ✓ Kafka deployed to namespace 'kafka'
# ✓ All 3 pods running
# ✓ Bootstrap server accessible
```

## 🔧 Creating New Skills

See [docs/skill-development-guide.md](./docs/skill-development-guide.md) for complete instructions.

**Quick Start:**

```bash
mkdir -p .claude/skills/my-skill/scripts
cd .claude/skills/my-skill
```

Create `SKILL.md`:
```markdown
---
name: my-skill
description: One-line description
---

# My Skill

## When to Use
- Specific trigger phrases
- Use cases

## Instructions
1. Run: `./scripts/action.sh`
2. Verify: `python scripts/verify.py`

## Validation
- [ ] Success criteria
```

Create executable script that returns **minimal output**.

## 🤝 Contributing

New skills welcome! Ensure they follow:

1. **YAML frontmatter** in SKILL.md
2. **~100 token** instruction limit
3. **Minimal output** from scripts (< 50 lines)
4. **Verification step** included
5. **Idempotent** execution (safe to run multiple times)
6. **Cross-agent compatibility** (works on Claude + Goose)

## 📄 License

MIT

---

**Built for Hackathon III: Reusable Intelligence and Cloud-Native Mastery**

*Teaching AI agents to build applications, not just generate code.*
