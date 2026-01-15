# n8n Workflow Builder

AI-powered toolkit for creating high-quality n8n workflows using the n8n-MCP server and specialized skills.

## What is this?

This project provides AI-assisted workflow creation for self-hosted n8n instances. It combines:

- **n8n-MCP Server Integration** - Direct API access to search nodes, validate configs, and manage workflows
- **7 Specialized Skills** - Automatic activation of domain-specific knowledge for expressions, patterns, validation, and code
- **Template Library** - Access to 2,700+ real-world workflow patterns

## Supported Workflow Types

- Business process automation
- AI/LLM agent workflows
- SaaS integrations (Slack, email, databases)
- Scheduled tasks and data synchronization
- Webhook processing

## Key Features

| Feature | Description |
|---------|-------------|
| Node Discovery | Search 400+ n8n nodes by keyword |
| Template Search | Find proven patterns by task type |
| Auto-validation | Validate nodes and workflows before deployment |
| Auto-fix | Automatically resolve common configuration issues |
| Version Control | Workflow history and rollback support |

## Setup

1. Ensure your n8n instance is running with API access enabled
2. Copy the MCP configuration template:

   ```bash
   cp .mcp.json.example .mcp.json
   ```

3. Edit `.mcp.json` with your n8n instance URL and API key
4. Start building with AI assistance:
   - Describe your automation needs
   - AI searches for relevant nodes/templates
   - Validates and creates the workflow
   - Auto-fixes common issues

## MCP Tools Available

### Discovery
- `search_nodes` - Find nodes by keyword
- `search_templates` - Find templates by keyword, nodes, task, or metadata

### Configuration
- `get_node` - Get node info and documentation
- `get_template` - Get template details

### Validation
- `validate_node` - Validate node configuration
- `validate_workflow` - Full workflow validation

### Workflow Management
- `n8n_create_workflow` - Create new workflow
- `n8n_update_partial_workflow` - Incremental updates
- `n8n_test_workflow` - Test/trigger execution
- `n8n_autofix_workflow` - Auto-fix common errors
- `n8n_workflow_versions` - Version history and rollback

## Common Workflow Patterns

### Webhook → Process → Action
```
Webhook → Set/Code → IF → Action (Slack/Email/HTTP)
```

### Schedule → Fetch → Transform → Store
```
Schedule Trigger → HTTP Request → Code → Database/Spreadsheet
```

### AI Agent Workflow
```
Chat Trigger → AI Agent → [Tools] → Response
```

## Expression Syntax

n8n expressions use double curly braces:

```javascript
{{ $json.fieldName }}           // Access current item field
{{ $node["NodeName"].json }}    // Access another node's output
{{ $now }}                      // Current timestamp
{{ $json.body.email }}          // Webhook data (under body!)
```

## Safety Guidelines

- Workflows are created **inactive by default**
- Always test before activating in production
- Export backups of important workflows
- Use `n8n_workflow_versions` for rollback capability

## Specialized Skills

This project uses 7 skills that activate automatically based on context:

1. **n8n-expression-syntax** - Correct expression patterns
2. **n8n-mcp-tools-expert** - MCP tool usage guidance
3. **n8n-workflow-patterns** - Proven workflow architectures
4. **n8n-validation-expert** - Error interpretation and fixing
5. **n8n-node-configuration** - Operation-aware node setup
6. **n8n-code-javascript** - JavaScript in Code nodes
7. **n8n-code-python** - Python in Code nodes

## License

MIT
