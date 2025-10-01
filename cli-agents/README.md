# Amazon Q CLI Multi-Agents Setup

This directory contains specialized agent configurations for Amazon Q CLI, enabling you to switch between different AI assistants optimized for specific tasks and domains.

## What are Amazon Q CLI Agents?

Amazon Q CLI agents are specialized AI assistants with:
- Custom prompts and personalities
- Pre-configured MCP (Model Context Protocol) servers
- Specific tool permissions and settings
- Domain-specific knowledge and capabilities

## Available Agents

### aws-docs.json
**Purpose:** AWS documentation specialist  
**Capabilities:** 
- Access to comprehensive AWS documentation
- AWS Knowledge Base integration
- Best practices and architectural guidance
- Service configurations and troubleshooting

### aws-costs.json
**Purpose:** AWS cost analysis and optimization  
**Capabilities:**
- Cost analysis and pricing information
- Resource optimization recommendations
- Billing insights and cost management

## Agent Configuration Structure

Each agent configuration follows this JSON schema:

```json
{
  "$schema": "https://raw.githubusercontent.com/aws/amazon-q-developer-cli/refs/heads/main/schemas/agent-v1.json",
  "name": "agent-name",
  "description": "Agent description",
  "prompt": "System prompt defining agent behavior",
  "mcpServers": {
    "server-name": {
      "command": "uvx|docker",
      "args": ["server-args"],
      "autoApprove": ["tool1", "tool2"]
    }
  },
  "tools": ["*"],
  "allowedTools": ["fs_read"],
  "resources": ["file://path/to/resource"],
  "toolsSettings": {}
}
```

## Usage

### Managing Agents

Use these commands during a chat session to manage agents:

```bash
# List all available agents
/agent list

# Switch to a specific agent for current session
/agent swap aws-docs

# Create a new agent
/agent create my-new-agent

# Generate an agent configuration using AI
/agent generate my-ai-agent

# Edit an existing agent configuration
/agent edit aws-docs

# Set default agent for new sessions
/agent set-default aws-docs

# View agent configuration schema
/agent schema
```

### Using Agents from Command Line

You can specify an agent when starting a chat session:

```bash
# Start chat with specific agent
q chat --agent aws-docs
```

### Setting Default Agent

Configure a default agent that will be used automatically:

```bash
# Set default agent for all new conversations
/agent set-default aws-docs

# View current default agent (use q settings from terminal)
q settings chat.defaultAgent

# Reset to built-in default (use q settings from terminal)
q settings chat.defaultAgent ""
```

### Agent Selection Priority

Amazon Q CLI uses this priority order to select agents:

1. **Command-line specified**: `q chat --agent my-agent`
2. **User-configured default**: Set via `q settings chat.defaultAgent agent-name`
3. **Built-in default**: Fallback agent with basic configuration

## Agent File Locations

### Local Agents (Current Project)
```
.amazonq/cli-agents/
├── project-specific-agent.json
└── dev-helper.json
```

### Global Agents (User-wide)
```
~/.aws/amazonq/cli-agents/
├── aws-docs.json
├── aws-costs.json
└── general-assistant.json
```

**Precedence:** Local agents override global agents with the same name.

## Creating Custom Agents

### Basic Agent Template

```json
{
  "$schema": "https://raw.githubusercontent.com/aws/amazon-q-developer-cli/refs/heads/main/schemas/agent-v1.json",
  "name": "my-agent",
  "description": "Custom agent description",
  "prompt": "You are a specialized assistant for...",
  "tools": ["*"],
  "allowedTools": ["fs_read", "fs_write"],
  "resources": []
}
```

### Adding MCP Servers

Reference MCP server configurations from the parent directory:

```json
{
  "mcpServers": {
    "aws-core": {
      "command": "uvx",
      "args": ["awslabs.core-mcp-server@latest"],
      "autoApprove": []
    }
  }
}
```

### Tool Configuration

Control tool permissions and settings:

```json
{
  "allowedTools": ["fs_read", "execute_bash"],
  "toolsSettings": {
    "execute_bash": {
      "allowedCommands": ["git status", "npm test"],
      "allowReadOnly": true
    },
    "fs_write": {
      "allowedPaths": ["./src/**", "./docs/**"]
    }
  }
}
```

## Best Practices

### Agent Design
- **Single Purpose:** Design agents for specific domains or tasks
- **Clear Prompts:** Write detailed system prompts that define behavior
- **Minimal Permissions:** Grant only necessary tool access
- **Auto-approve:** Use sparingly for trusted, read-only operations

### File Organization
- Use descriptive names (e.g., `aws-security.json`, `python-dev.json`)
- Group related agents in subdirectories if needed
- Document agent purposes in descriptions

### Security
- Review tool permissions carefully
- Avoid auto-approving write operations
- Use `allowedPaths` and `allowedCommands` restrictions
- Test agents in safe environments first

## Example Workflows

### Development Agent
```json
{
  "name": "dev-helper",
  "description": "Development assistant with code analysis",
  "prompt": "You are a senior software engineer...",
  "allowedTools": ["fs_read", "fs_write", "execute_bash"],
  "toolsSettings": {
    "execute_bash": {
      "allowedCommands": ["npm.*", "git status", "git diff"]
    }
  }
}
```

### Documentation Agent
```json
{
  "name": "doc-writer",
  "description": "Technical documentation specialist",
  "prompt": "You are a technical writer...",
  "allowedTools": ["fs_read", "fs_write"],
  "resources": ["file://docs/**/*.md"]
}
```

## Troubleshooting

### Agent Not Found
- Check file location (local vs global)
- Verify JSON syntax with `q agent validate <name>`
- Ensure proper file permissions

### MCP Server Issues
- Verify MCP server dependencies are installed
- Check server logs for connection errors
- Test MCP configurations independently

### Permission Errors
- Review `allowedTools` and `toolsSettings`
- Check file path permissions
- Verify environment variables for MCP servers

## Related Documentation

- [Agent File Locations](https://github.com/aws/amazon-q-developer-cli/blob/main/docs/agent-file-locations.md)
- [Built-in Tools](https://github.com/aws/amazon-q-developer-cli/blob/main/docs/built-in-tools.md)
- [MCP Configuration Reference](../README.md)

## Commands Reference

| Command | Description |
|---------|-------------|
| `/agent list` | Show all available agents |
| `/agent swap <name>` | Switch to agent for current session |
| `/agent create <name>` | Create a new agent |
| `/agent generate <name>` | Generate agent configuration using AI |
| `/agent edit <name>` | Edit agent configuration |
| `/agent set-default <name>` | Set default agent for new sessions |
| `/agent schema` | View agent configuration schema |
| `q chat --agent <name>` | Start chat with specific agent |
