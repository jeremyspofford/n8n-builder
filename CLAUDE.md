# n8n Workflow Builder

This project enables AI-assisted creation of high-quality n8n workflows using the n8n-MCP server and specialized skills.

## Project Purpose

Build production-ready n8n workflows for a self-hosted n8n instance. Supports all workflow types:

- Business process automation
- AI/LLM workflows with agents
- SaaS integrations
- Scheduled tasks and data sync

## Development Rules

This project has coding standards in `.claude/rules/`:

### Always-On Rules (Auto-Applied)

- **[Markdown Linting](.claude/rules/markdown-linting.md)** - MD040 (language tags) and MD060 (table formatting)
- **[Coding Practices](.claude/rules/coding-practices.md)** - TypeScript standards (no `any`, prefer `interface`)

### Manual Rules (Invoke When Needed)

- **[TDD Workflow](.claude/rules/tdd-workflow.md)** - The "Ralph Loop" for test-driven development

To invoke the TDD workflow, reference the file: "Follow the Ralph Loop from `.claude/rules/tdd-workflow.md`"

## Workflow Building Process

Follow this process when creating workflows:

### 1. Understand Requirements

- What triggers the workflow? (webhook, schedule, manual, chat)
- What data needs to be processed?
- What actions should be taken?
- What error handling is needed?

### 2. Check n8n Connection

```markdown
n8n_health_check
```

### 3. Search for Relevant Resources

```markdown
search_nodes(query="slack")           # Find nodes by keyword
search_templates(query="webhook")     # Find workflow templates
search_templates(searchMode="by_task", taskType="notification")  # Task-based search
```

### 4. Get Node Details

```markdown
get_node(nodeType="nodes-base.slack", detail="standard")  # Get node info
get_node(nodeType="nodes-base.slack", mode="docs")        # Get documentation
```

### 5. Validate Before Creating

```markdown
validate_node(nodeType="nodes-base.slack", config={...})  # Validate node config
validate_workflow(workflow={...})                          # Validate full workflow
```

### 6. Create or Deploy Workflow

```markdown
n8n_create_workflow(name="My Workflow", nodes=[...], connections={...})
# OR deploy from template:
n8n_deploy_template(templateId=123)
```

### 7. Auto-fix and Test

```markdown
n8n_autofix_workflow(workflowId="abc123")   # Fix common issues
n8n_test_workflow(workflowId="abc123")      # Test execution
```

## Available MCP Tools

### Discovery

| Tool | Purpose |
| ---- | ------- |
| `search_nodes` | Find nodes by keyword (max 20 results) |
| `search_templates` | Find templates by keyword, nodes, task, or metadata |

### Configuration

| Tool | Purpose |
| ---- | ------- |
| `get_node` | Get node info (detail: minimal/standard/full, mode: info/docs/search_properties/versions) |
| `get_template` | Get template details (mode: nodes_only/structure/full) |

### Validation

| Tool | Purpose |
| ---- | ------- |
| `validate_node` | Validate node configuration (mode: full/minimal) |
| `validate_workflow` | Full workflow validation (structure, connections, expressions) |

### Workflow Management

| Tool | Purpose |
| ---- | ------- |
| `n8n_create_workflow` | Create new workflow (inactive by default) |
| `n8n_get_workflow` | Get workflow by ID |
| `n8n_list_workflows` | List all workflows |
| `n8n_update_full_workflow` | Replace entire workflow |
| `n8n_update_partial_workflow` | Incremental updates via diff operations |
| `n8n_delete_workflow` | Delete workflow permanently |
| `n8n_test_workflow` | Test/trigger workflow execution |
| `n8n_autofix_workflow` | Auto-fix common validation errors |
| `n8n_validate_workflow` | Validate existing workflow by ID |
| `n8n_workflow_versions` | Version history and rollback |
| `n8n_executions` | Manage executions |
| `n8n_deploy_template` | Deploy template with auto-fix |

### System

| Tool | Purpose |
| ---- | ------- |
| `tools_documentation` | Get MCP tools documentation |
| `n8n_health_check` | Check n8n instance health |
| `n8n_diagnostic` | Detailed troubleshooting |

## Best Practices

### Token Efficiency

- Use `detail="minimal"` for initial exploration
- Use `detail="standard"` for building (default)
- Use `detail="full"` only when you need complete schema
- Use `n8n_update_partial_workflow` instead of full updates (saves 80-90% tokens)

### Validation

- Always validate node configurations before adding to workflow
- Always validate complete workflow before deployment
- Use `n8n_autofix_workflow` to fix common issues automatically

### Templates

- Search templates first - they provide proven patterns
- Use `n8n_deploy_template` to deploy and auto-fix in one step
- Templates include real-world configurations from 2,700+ workflows

## Safety Guidelines

**NEVER edit production workflows directly!**

- Always make a copy before modifying
- Test in development environment first
- Export backups of important workflows
- Workflows are created inactive by default - activate only after testing

## Common Workflow Patterns

### Webhook → Process → Action

```markdown
Webhook → Set/Code → IF → Action (Slack/Email/HTTP)
```

### Schedule → Fetch → Transform → Store

```markdown
Schedule Trigger → HTTP Request → Code → Database/Spreadsheet
```

### AI Agent Workflow

```markdown
Chat Trigger → AI Agent → [Tools: HTTP, Code, etc.] → Response
```

### Error Handling

```markdown
Main Flow → Error Trigger → Notification
```

Use error outputs on nodes and connect to error handling branches.

## Node Type Format

When using MCP tools, use the correct nodeType format:

- For MCP tools: `nodes-base.slack` (without `n8n-` prefix)
- In workflow JSON: `n8n-nodes-base.slack` (with `n8n-` prefix)

The MCP server handles this conversion automatically.

## Expression Syntax

n8n expressions use double curly braces:

```javascript
{{ $json.fieldName }}           // Access current item field
{{ $node["NodeName"].json }}    // Access another node's output
{{ $now }}                      // Current timestamp
{{ $env.VAR_NAME }}             // Environment variable
```

**Critical**: Webhook data is under `$json.body`, not `$json` directly!

```javascript
{{ $json.body.email }}          // Correct for webhook data
{{ $json.email }}               // Wrong - will be undefined
```

## Skills Available

This project uses 7 specialized skills that activate automatically:

1. **n8n-expression-syntax** - Correct expression patterns
2. **n8n-mcp-tools-expert** - MCP tool usage guidance
3. **n8n-workflow-patterns** - Proven workflow architectures
4. **n8n-validation-expert** - Error interpretation and fixing
5. **n8n-node-configuration** - Operation-aware node setup
6. **n8n-code-javascript** - JavaScript in Code nodes
7. **n8n-code-python** - Python in Code nodes

Skills activate based on query context - no manual invocation needed.
