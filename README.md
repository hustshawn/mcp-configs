# MCP Configuration Reference

A collection of Model Context Protocol (MCP) server configurations for various services and tools. These configurations can be used with Kiro IDE or other MCP-compatible clients.

## What is MCP?

Model Context Protocol (MCP) is an open standard that enables AI assistants to securely connect to external data sources and tools. Each configuration file in this repository defines how to connect to different MCP servers.

## Usage

### With Kiro IDE

1. Copy the desired configuration files to your project's `.kiro/settings/` directory
2. Merge the contents with your existing `mcp.json` file, or rename the file to `mcp.json`
3. Install required dependencies (see [Prerequisites](#prerequisites))
4. Restart Kiro or reconnect MCP servers from the MCP Server view

### Configuration Structure

Each configuration file follows this structure:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "command-to-run",
      "args": ["arg1", "arg2"],
      "env": {
        "ENV_VAR": "value"
      },
      "disabled": false,
      "autoApprove": ["tool1", "tool2"],
      "timeout": 60000
    }
  }
}
```

Common patterns include:
- **uvx servers**: `"command": "uvx", "args": ["package@latest"]`
- **Docker servers**: `"command": "docker", "args": ["run", "-i", "--rm", "image"]`

## Available Configurations

### AWS Services

- **aws-core-mcp.json** - Core AWS services integration
- **aws-core-docker-mcp.json** - Core AWS services integration via Docker (auto-approves: prompt_understanding)
- **aws-core-cost-mcp.json** - Combined core AWS services and AWS pricing information
- **aws-core-doc-mcp.json** - Combined core AWS services and documentation
- **aws-cdk-mcp.json** - AWS CDK operations
- **aws-cost-analyzer-mcp.json** - AWS cost analysis tools
- **aws-cost-explora-mcp.json** - AWS cost explorer integration
- **aws-diagram-mcp.json** - AWS architecture diagrams
- **aws-doc-mcp.json** - AWS documentation access
- **aws-doc-docker-mcp.json** - AWS documentation access via Docker (auto-approves: search_documentation, read_documentation)
- **aws-doc-ecs-mcp.json** - AWS ECS documentation
- **aws-ecs-mcp.json** - AWS ECS management
- **aws-eks-mcp.json** - AWS EKS operations
- **aws-knowledge-mcp.json** - AWS Knowledge Base access
- **aws-terraform-mcp.json** - AWS Terraform integration

### Development Tools

- **github-docker-mcp.json** - GitHub integration via Docker
- **k8s-docker-mcp.json** - Kubernetes management via Docker
- **container-use-mcp.json** - Container utilities

### Productivity & Utilities

- **amap-mcp.json** - Amap (高德地图) integration
- **ms-office-mcp.json** - Microsoft Office document processing
- **time-docker-mcp.json** - Time and date utilities

## Prerequisites

### For uvx-based servers

```bash
# Install uv and uvx
pip install uv
# or via homebrew
brew install uv
```

### For Docker-based servers

```bash
# Ensure Docker is installed and running
docker --version
```

### Environment Variables

Some configurations require environment variables. Create a `.kiro/settings/.env` file with:

```bash
# GitHub (for github-docker-mcp.json)
GITHUB_TOKEN=your_github_token

# Amap (for amap-mcp.json)  
AMAP_MAPS_API_KEY=your_amap_api_key

# AWS (for AWS-related configs)
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=your_preferred_region
```

## Installation Examples

### Single Configuration

```bash
# Copy a specific config
cp aws-core-mcp.json .kiro/settings/mcp.json
```

### Multiple Configurations

Some configuration files already combine multiple servers. For example, `aws-core-doc-mcp.json` includes both core AWS services and documentation:

```json
{
  "mcpServers": {
    "awslabs.core-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.core-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "autoApprove": [],
      "disabled": false
    },
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "default"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

You can also manually merge configurations by combining the `mcpServers` objects from multiple files.

## Configuration Options

- **command**: The executable to run the MCP server
- **args**: Command line arguments
- **env**: Environment variables for the server
- **disabled**: Set to `true` to disable the server
- **autoApprove**: List of tools to auto-approve without user confirmation
- **timeout**: Timeout in milliseconds for server operations

## Troubleshooting

### Server Won't Start

1. Check that required dependencies are installed (uvx, Docker, etc.)
2. Verify environment variables are set correctly
3. Check the MCP Server view in Kiro for error messages

### Permission Issues

- Ensure Docker daemon is running for Docker-based servers
- Check that environment files have correct permissions
- Verify API keys and tokens are valid

## Contributing

Feel free to submit additional MCP server configurations via pull requests. Please ensure:

1. Configuration files are properly formatted JSON
2. Include necessary environment variable documentation
3. Test the configuration before submitting

## License

This repository is provided as-is for reference purposes. Individual MCP servers may have their own licenses and terms of use.
