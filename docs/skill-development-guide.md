# Skill Development Guide

## Introduction

This guide explains how to create reusable skills for AI coding agents (Claude Code, Goose, OpenAI Codex) using the **MCP Code Execution** pattern. Skills enable AI agents to autonomously build and deploy applications with minimal token usage.

## Why Skills Matter

**Traditional Approach: High Token Usage**
```
MCP Server Loaded → 50,000 tokens in context
Every tool call → Full data flows through context
Result: 41% of context consumed before work begins
```

**Skills Approach: Maximum Efficiency**
```
SKILL.md → ~100 tokens
Scripts execute → 0 tokens (run outside context)
Only results → ~10 tokens back to agent
Result: 3% context usage, 97% free for work
```

## Skill Anatomy

### Directory Structure

```
.claude/skills/
└── my-skill-name/
    ├── SKILL.md           # ~100 token instructions (REQUIRED)
    ├── REFERENCE.md       # Detailed docs (loaded on-demand)
    ├── scripts/           # Executable scripts (0 tokens)
    │   ├── deploy.sh
    │   ├── verify.py
    │   └── helpers/
    └── templates/         # Optional configuration templates
        └── config.yaml
```

### SKILL.md Format

**CRITICAL**: Must have YAML frontmatter and follow this structure:

\`\`\`markdown
---
name: skill-name
description: Brief one-line description of what this skill does
---

# Skill Name

## When to Use
- Bullet points explaining when an AI agent should invoke this skill
- Specific triggers or user requests
- Use cases

## Instructions
1. Step 1: Run this script with these parameters
2. Step 2: Verify the result
3. Step 3: Confirm success before proceeding

## Prerequisites
- Required tools or dependencies
- Cluster requirements
- Access credentials

## Validation
- [ ] Success criterion 1
- [ ] Success criterion 2
- [ ] Expected state after completion

See [REFERENCE.md](./REFERENCE.md) for advanced configuration.
\`\`\`

## The MCP Code Execution Pattern

### Pattern Overview

Instead of loading MCP tools directly into agent context:

**Before (Direct MCP)**
```javascript
// This all flows through agent context
const data = await mcp.getDocument('huge-file');
const filtered = data.filter(item => item.status === 'active');
// 50,000 tokens consumed
```

**After (Skill + Script)**

SKILL.md:
\`\`\`markdown
## Instructions
1. Run: \`python scripts/filter_data.py <document_id> <filter_criteria>\`
2. Review output
\`\`\`

scripts/filter_data.py:
\`\`\`python
# Script runs outside agent context
data = mcp_client.get_document(doc_id)
filtered = [item for item in data if item.status == filter_criteria]
print(f"Found {len(filtered)} items")  # Only this enters context
\`\`\`

**Result**: 100 tokens instead of 50,000

## Creating Your First Skill

### Example: Kubernetes Pod Lister

**Step 1: Create Directory**
\`\`\`bash
mkdir -p .claude/skills/k8s-pod-list
cd .claude/skills/k8s-pod-list
\`\`\`

**Step 2: Write SKILL.md**
\`\`\`markdown
---
name: k8s-pod-list
description: List Kubernetes pods across namespaces with status filtering
---

# Kubernetes Pod Lister

## When to Use
- User asks "show me pods" or "list running pods"
- Need to check cluster health
- Debugging deployment issues

## Instructions
1. List all pods: \`python scripts/list_pods.py\`
2. Filter by namespace: \`python scripts/list_pods.py --namespace <name>\`
3. Filter by status: \`python scripts/list_pods.py --status Running\`

## Prerequisites
- kubectl configured and authenticated
- Python 3.8+ with kubectl installed

## Validation
- [ ] Script returns pod list without errors
- [ ] Output is formatted and readable
- [ ] Status filters work correctly
\`\`\`

**Step 3: Create Script**
\`\`\`python
#!/usr/bin/env python3
# scripts/list_pods.py
import subprocess
import json
import sys
import argparse

def list_pods(namespace=None, status=None):
    cmd = ["kubectl", "get", "pods"]

    if namespace:
        cmd.extend(["-n", namespace])
    else:
        cmd.append("--all-namespaces")

    cmd.extend(["-o", "json"])

    result = subprocess.run(cmd, capture_output=True, text=True)
    pods = json.loads(result.stdout)["items"]

    # Filter by status if specified
    if status:
        pods = [p for p in pods if p["status"]["phase"] == status]

    # Output minimal summary (NOT full JSON)
    print(f"Found {len(pods)} pods")
    for pod in pods[:10]:  # Show first 10
        name = pod["metadata"]["name"]
        ns = pod["metadata"]["namespace"]
        phase = pod["status"]["phase"]
        print(f"  {ns}/{name}: {phase}")

    if len(pods) > 10:
        print(f"  ... and {len(pods) - 10} more")

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--namespace", help="Filter by namespace")
    parser.add_argument("--status", help="Filter by status (Running, Pending, etc)")
    args = parser.parse_args()

    list_pods(args.namespace, args.status)
\`\`\`

**Step 4: Make Executable**
\`\`\`bash
chmod +x scripts/list_pods.py
\`\`\`

**Step 5: Test**
\`\`\`bash
# Test locally first
python scripts/list_pods.py --status Running

# Then test with Claude Code
claude "Show me all running pods"
\`\`\`

## Best Practices

### 1. Minimal Output

**❌ BAD: Returns full data**
\`\`\`python
print(json.dumps(all_pods))  # 10,000 tokens
\`\`\`

**✅ GOOD: Returns summary**
\`\`\`python
print(f"Found {len(all_pods)} pods, {running} running")  # 15 tokens
\`\`\`

### 2. Clear Error Messages

**❌ BAD: Stack trace**
\`\`\`python
raise Exception("Failed")  # Agent sees full stack trace
\`\`\`

**✅ GOOD: Actionable error**
\`\`\`python
if not cluster_reachable:
    print("✗ Cannot connect to cluster. Run 'kubectl cluster-info'")
    sys.exit(1)
\`\`\`

### 3. Idempotency

Scripts should be safe to run multiple times:

\`\`\`bash
# Create namespace if it doesn't exist
kubectl create namespace myapp --dry-run=client -o yaml | kubectl apply -f -
\`\`\`

### 4. Verification Steps

Always include a verification step:

\`\`\`markdown
## Instructions
1. Deploy: \`./scripts/deploy.sh\`
2. Verify: \`python scripts/verify.py\`  ← CRITICAL
3. Only proceed if verification passes
\`\`\`

### 5. Use REFERENCE.md for Details

Keep SKILL.md under 150 tokens. Move detailed docs to REFERENCE.md:

**SKILL.md**
\`\`\`markdown
## Instructions
1. Deploy Kafka: \`./scripts/deploy.sh\`
2. See [REFERENCE.md](./REFERENCE.md) for custom configuration
\`\`\`

**REFERENCE.md**
\`\`\`markdown
# Kafka Deployment Reference

## Configuration Options

### Replication Factor
Default: 1 (development)
Production: 3

Edit scripts/deploy.sh and change:
\`--set replication.factor=3\`

### Resource Limits
...
\`\`\`

## Common Patterns

### Pattern 1: Deploy + Verify

\`\`\`markdown
## Instructions
1. Deploy: \`./scripts/deploy.sh <component>\`
2. Wait for ready: \`python scripts/wait_ready.py <component>\`
3. Verify health: \`python scripts/verify.py <component>\`
\`\`\`

### Pattern 2: Conditional Execution

\`\`\`bash
# Check if already deployed
if kubectl get deployment myapp -n default &>/dev/null; then
    echo "✓ Already deployed"
else
    echo "Deploying..."
    kubectl apply -f manifests/
fi
\`\`\`

### Pattern 3: MCP Server Wrapper

\`\`\`python
# scripts/mcp_client.py - Wraps MCP server calls
from mcp import Client

def get_filtered_data(doc_id, filter_type):
    client = Client("mcp-server-name")
    raw_data = client.get_document(doc_id)

    # Filter CLIENT-SIDE, not in agent context
    filtered = [item for item in raw_data if item.type == filter_type]

    # Return minimal summary
    return {
        "total": len(raw_data),
        "filtered": len(filtered),
        "sample": filtered[:3]  # Just 3 examples
    }
\`\`\`

## Testing Your Skills

### Local Testing

\`\`\`bash
# 1. Test script directly
cd .claude/skills/my-skill
./scripts/deploy.sh

# 2. Check output format
python scripts/verify.py | wc -l  # Should be < 20 lines

# 3. Test with both agents
claude "Use my-skill to deploy"
goose "Use my-skill to deploy"
\`\`\`

### Validation Checklist

- [ ] SKILL.md has valid YAML frontmatter
- [ ] SKILL.md is under 200 tokens
- [ ] Scripts execute without errors
- [ ] Scripts return minimal output (< 50 lines)
- [ ] Works on both Claude Code and Goose
- [ ] Includes verification step
- [ ] Handles errors gracefully
- [ ] Is idempotent (safe to run multiple times)

## Advanced Topics

### Multi-Step Skills

For complex workflows, break into multiple skills:

\`\`\`
.claude/skills/
├── kafka-deploy/        # Step 1: Deploy Kafka
├── kafka-topic-create/  # Step 2: Create topics
└── kafka-producer/      # Step 3: Test producer
\`\`\`

### Cross-Agent Compatibility

Both Claude Code and Goose read `.claude/skills/`:

\`\`\`bash
# Same skills work on both!
claude "Deploy Kafka"
goose "Deploy Kafka"
\`\`\`

### Token Efficiency Metrics

Measure your skills:

**Before (Direct MCP)**
- Startup: 50,000 tokens
- Per operation: 5,000 tokens
- Total for 10 ops: 100,000 tokens

**After (Skills)**
- Startup: 0 tokens
- Per operation: 100 tokens (SKILL.md) + 20 tokens (output)
- Total for 10 ops: 1,200 tokens

**Efficiency: 98.8% reduction**

## Troubleshooting

### Skill Not Loading

\`\`\`bash
# Check YAML syntax
head -5 .claude/skills/my-skill/SKILL.md

# Should show:
# ---
# name: my-skill
# description: ...
# ---
\`\`\`

### Script Fails

\`\`\`bash
# Test script directly
cd .claude/skills/my-skill
bash -x scripts/deploy.sh  # Debug mode

# Check permissions
chmod +x scripts/*.sh
\`\`\`

### Agent Doesn't Use Skill

Make sure "When to Use" is clear:

\`\`\`markdown
## When to Use
- User says "deploy Kafka" ← EXACT PHRASE
- User wants event streaming ← CONCEPT
- Setting up microservices ← CONTEXT
\`\`\`

## Examples from LearnFlow

See the LearnFlow hackathon project for real-world examples:

- `kafka-k8s-setup` - Deploy stateful infrastructure
- `fastapi-dapr-agent` - Generate microservice templates
- `postgres-k8s-setup` - Database deployment with migrations
- `nextjs-k8s-deploy` - Frontend containerization

## Resources

- [AAIF Standards](https://aaif.io/)
- [MCP Code Execution Blog](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)
- [Goose Skills Guide](https://block.github.io/goose/docs/guides/context-engineering/using-skills)

## Summary

**Creating a skill:**
1. Create `.claude/skills/<name>/` directory
2. Write SKILL.md with YAML frontmatter (~100 tokens)
3. Create executable scripts (return minimal output)
4. Add REFERENCE.md for detailed docs
5. Test with both Claude Code and Goose

**Key principle:** Scripts do the work (0 tokens), only results flow back.

Your skills become **reusable intelligence** that works across projects and AI agents.
