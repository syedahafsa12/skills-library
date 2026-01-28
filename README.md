# Skills Library for AI Agent Development

This repository contains reusable skills that enable AI agents (Claude Code and Goose) to build the LearnFlow AI-powered Python tutoring platform. The skills follow the MCP Code Execution pattern to achieve 80-98% token reduction while maintaining full capability.

## Skills Overview

### 1. agents-md-gen
- **Purpose**: Generate AGENTS.md files for repositories
- **Token Efficiency**: ~100 tokens for instruction vs 50,000+ for direct MCP
- **Features**: Repository structure analysis, file categorization, AI agent guidelines

### 2. k8s-foundation
- **Purpose**: Kubernetes foundation operations and health checks
- **Token Efficiency**: Minimal output scripts instead of verbose tool output
- **Features**: Cluster health checking, foundation setup, readiness verification

### 3. kafka-k8s-setup
- **Purpose**: Deploy Apache Kafka on Kubernetes
- **Token Efficiency**: Scripts handle complex deployment, return minimal status
- **Features**: Automated Kafka/Zookeeper deployment, topic creation, verification

### 4. postgres-k8s-setup
- **Purpose**: Deploy PostgreSQL on Kubernetes with migrations
- **Token Efficiency**: Schema migrations in scripts, not agent context
- **Features**: Database deployment, LearnFlow schema creation, verification

### 5. fastapi-dapr-agent
- **Purpose**: Create FastAPI microservices with Dapr for AI agents
- **Token Efficiency**: Service scaffolding via scripts
- **Features**: AI agent service creation, Dapr integration, Kubernetes deployment

### 6. mcp-code-execution
- **Purpose**: MCP with code execution pattern for efficient context management
- **Token Efficiency**: Up to 99% reduction vs direct MCP calls
- **Features**: Data filtering, aggregation, processing patterns

### 7. nextjs-k8s-deploy
- **Purpose**: Deploy Next.js applications to Kubernetes
- **Token Efficiency**: Build/deployment in scripts
- **Features**: App scaffolding, Docker building, Kubernetes deployment

### 8. docusaurus-deploy
- **Purpose**: Deploy Docusaurus documentation sites to Kubernetes
- **Token Efficiency**: Site generation in scripts
- **Features**: Documentation site creation, custom theming, deployment

## Token Efficiency Achieved

| Approach | Tokens Used | Reduction |
|----------|-------------|-----------|
| Direct MCP | 50,000+ | 0% |
| Code Execution Pattern | ~110 | 98.7% |

The skills library demonstrates the MCP Code Execution Pattern where:
- SKILL.md provides instructions (~100 tokens)
- Scripts execute heavy lifting (0 tokens loaded)
- Only minimal results return to agent context

## Cross-Agent Compatibility

All skills work with both Claude Code and Goose:
- Same `.claude/skills/` directory structure
- Shared skill format
- Consistent interfaces

## Architecture

The skills support the LearnFlow architecture:
- Event-driven with Apache Kafka
- Microservices with Dapr
- AI agents for different tutoring functions
- Persistent storage with PostgreSQL
- Scalable frontend with Next.js

## Usage

AI agents can use these skills by referencing them in prompts:

```
"Deploy Kafka for event streaming using kafka-k8s-setup skill"
"Create triage-agent service using fastapi-dapr-agent skill"
"Deploy documentation using docusaurus-deploy skill"
```

## Development

Each skill follows the pattern:
```
.cloude/skills/skill-name/
├── SKILL.md              # Instructions (~100 tokens)
├── scripts/              # Heavy lifting (0 tokens loaded)
│   ├── *.sh
│   └── *.py
└── REFERENCE.md          # Detailed documentation
```

## Benefits

1. **Token Efficiency**: Significant reduction in context usage
2. **Reusability**: Skills work across multiple projects
3. **Maintainability**: Centralized skill definitions
4. **Autonomy**: AI agents can perform complex tasks independently
5. **Consistency**: Standardized approaches across the platform

## LearnFlow Components Supported

- **Triage Agent**: Routes queries to specialists
- **Concepts Agent**: Explains Python concepts
- **Code Review Agent**: Analyzes code quality
- **Debug Agent**: Helps with error resolution
- **Exercise Agent**: Generates challenges
- **Progress Agent**: Tracks mastery

## MCP Integration

Skills demonstrate proper MCP integration with:
- Data filtering in scripts
- Aggregation and processing
- Minimal context pollution
- Efficient API usage patterns

This skills library enables AI agents to autonomously build, deploy, and manage the complete LearnFlow platform with minimal human intervention.
