# Skill Development Guide

This guide explains how to create effective skills for AI agents using the MCP Code Execution pattern.

## The MCP Code Execution Pattern

### The Problem
Direct MCP integration causes token bloat:
- MCP tool definitions consume 10,000+ tokens at startup
- Large datasets flow through context multiple times
- A 10,000-row dataset can consume 50,000+ tokens

### The Solution
Wrap MCP calls in code execution scripts:
```
SKILL.md (~100 tokens) → scripts/ (0 tokens loaded) → minimal result
```

## Skill Structure

Each skill should follow this structure:
```
.cloude/skills/skill-name/
├── SKILL.md              # Instructions (~100 tokens)
├── scripts/              # Heavy lifting (0 tokens loaded)
│   ├── *.sh
│   └── *.py
└── REFERENCE.md          # Detailed documentation
```

### SKILL.md Requirements
- YAML frontmatter with name and description
- Clear usage instructions
- Validation criteria
- Links to REFERENCE.md

Example:
```yaml
---
name: example-skill
description: Example skill description
---
# Example Skill

## When to Use
- When user wants to do X
- When setting up Y

## Instructions
1. Run: `bash scripts/example.sh`
2. Verify: `python scripts/verify.py`

## Validation
- [ ] Task completed successfully
- [ ] All components running
```

### Script Best Practices
- Return minimal output to agent context
- Handle errors gracefully
- Provide clear status messages
- Use consistent naming conventions

## Token Efficiency Patterns

### 1. Filter Pattern
Instead of returning large datasets, filter in scripts:
```python
# Inefficient - returns full dataset to context
def get_data():
    return mcp.get_large_dataset()  # Returns 10,000 records

# Efficient - filters in script, returns summary
def get_filtered_data():
    full_data = mcp.get_large_dataset()
    filtered = [item for item in full_data if meets_criteria(item)]
    return {"count": len(filtered), "sample": filtered[:5]}
```

### 2. Aggregate Pattern
Process data in scripts, return insights:
```python
# Inefficient - returns raw data
def get_student_scores():
    return mcp.get_all_scores()  # Thousands of records

# Efficient - aggregates in script
def get_score_summary():
    scores = mcp.get_all_scores()
    return {
        "average": sum(scores) / len(scores),
        "highest": max(scores),
        "lowest": min(scores)
    }
```

### 3. Process Pattern
Transform data in scripts:
```python
# Inefficient - processing in context
def analyze_trends():
    return mcp.get_raw_logs()  # Full logs to context

# Efficient - processing in script
def get_trend_insights():
    logs = mcp.get_raw_logs()
    trends = process_logs_for_trends(logs)
    return {"key_trends": trends, "time_periods": get_time_periods(logs)}
```

## Cross-Agent Compatibility

Skills should work with both Claude Code and Goose:
- Use standard shell commands
- Include proper error handling
- Test with both agents
- Follow platform-agnostic patterns

## Validation and Testing

Each skill should include:
- Clear validation criteria in SKILL.md
- Verification scripts
- Example usage scenarios
- Error handling tests

## Common Pitfalls to Avoid

1. **Verbose Output**: Scripts should return minimal information
2. **Complex Logic in Context**: Move processing to scripts
3. **Missing Error Handling**: Always handle potential failures
4. **Hardcoded Values**: Use parameters and environment variables
5. **Inconsistent Naming**: Follow established patterns

## Example: Good vs Bad

### Bad Example
```bash
#!/bin/bash
# This script returns verbose output to context
echo "Starting deployment..."
kubectl get pods -A  # Returns hundreds of lines
helm install my-app chart/  # May return lots of output
kubectl describe deployment my-app  # Returns detailed status
```

### Good Example
```bash
#!/bin/bash
# This script returns minimal output
echo "Deploying my-app..."
if helm install my-app chart/; then
    if kubectl wait deployment/my-app --for condition=Available=True --timeout=300s; then
        echo "✓ Deployment successful"
    else
        echo "✗ Deployment failed - timeout"
        exit 1
    fi
else
    echo "✗ Helm install failed"
    exit 1
fi
```

## Troubleshooting

### Token Usage Too High
- Move data processing from agent context to scripts
- Implement filtering and aggregation
- Reduce output verbosity

### Skill Not Triggering
- Check YAML frontmatter format
- Verify file placement in `.claude/skills/`
- Ensure name matches what agent is requesting

### Scripts Failing
- Test scripts independently
- Check permissions and dependencies
- Add proper error handling

## Performance Metrics

Track these metrics for each skill:
- Token usage reduction percentage
- Execution time
- Success rate
- Agent satisfaction scores

## Advanced Patterns

### MCP Client Wrapper
Create reusable MCP client patterns:
```python
class MCPClient:
    def efficient_get_data(self, dataset_id, filter_func=None):
        """Get data efficiently with optional filtering"""
        raw_data = self.mcp_get(dataset_id)
        if filter_func:
            filtered_data = [d for d in raw_data if filter_func(d)]
        else:
            filtered_data = raw_data
        return self.summarize(filtered_data)
```

### Configuration Management
Use configuration files for flexibility:
```bash
# Use config file instead of hardcoded values
source scripts/config.sh
kubectl create ns ${NAMESPACE:-"default"}
```

This guide helps ensure all skills follow best practices for token efficiency, cross-agent compatibility, and maintainability.
