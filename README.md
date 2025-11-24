# MCP Configuration Reference

A collection of Model Context Protocol (MCP) server configurations for various services and tools. These configurations can be used with Kiro IDE or other MCP-compatible clients.

For Amazon Q CLI users, see the [cli-agents/](cli-agents/) directory for specialized agent configurations.

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

### AWS Core Services

#### Single Server Configurations
- **aws-core-mcp.json** - Core AWS services integration using uvx
  - Package: `awslabs.core-mcp-server@latest`
  - Provides foundational AWS service access and operations

- **aws-core-docker-mcp.json** - Core AWS services via Docker
  - Image: `mcp/aws-core-mcp-server`
  - Auto-approves: `prompt_understanding`
  - Containerized alternative to uvx-based core server

#### Combined Configurations
- **aws-core-doc-mcp.json** - Core AWS services + Documentation
  - Combines `awslabs.core-mcp-server` and `awslabs.aws-documentation-mcp-server`
  - One-stop configuration for AWS operations and documentation lookup

- **aws-core-pricing-mcp.json** - Core AWS services + Pricing
  - Combines `awslabs.core-mcp-server` and `awslabs.aws-pricing-mcp-server`
  - Auto-approves pricing tools: `get_pricing_service_codes`, `get_pricing_service_attributes`, `get_pricing_attribute_values`, `get_pricing`
  - Ideal for cost-aware infrastructure planning

### AWS Documentation & Knowledge

- **aws-doc-mcp.json** - AWS documentation access
  - Package: `awslabs.aws-documentation-mcp-server@latest`
  - Search, read, and get recommendations from AWS documentation

- **aws-doc-docker-mcp.json** - AWS documentation via Docker
  - Image: `mcp/aws-documentation`
  - Auto-approves: `search_documentation`, `read_documentation`, `recommend`
  - Containerized documentation access

- **aws-doc-ecs-mcp.json** - AWS documentation + Diagrams
  - Combines documentation server with diagram generation
  - Useful for visualizing AWS architectures while reading docs

- **aws-knowledge-mcp.json** - AWS Knowledge Base access
  - Remote endpoint: `https://knowledge-mcp.global.api.aws`
  - Uses `mcp-proxy` with streamable HTTP transport
  - Access to AWS's centralized knowledge repository

### AWS Container Services

#### ECS (Elastic Container Service)
- **aws-ecs-mcp.json** - AWS ECS management
  - Package: `awslabs-ecs-mcp-server`
  - Environment controls: `ALLOW_WRITE`, `ALLOW_SENSITIVE_DATA`
  - Local ECS operations and monitoring

- **aws-ecs-remote-mcp.json** - AWS ECS remote operations
  - Remote endpoint: `https://ecs-mcp.us-west-2.api.aws/mcp`
  - Uses `mcp-proxy-for-aws@latest`
  - Auto-approves: `get_deployment_status`, `fetch_service_events`, `fetch_task_failures`, `fetch_task_logs`, `detect_image_pull_failures`, `fetch_network_configuration`, `get_task_definition_deletion_blockers`
  - Comprehensive ECS troubleshooting and monitoring

#### EKS (Elastic Kubernetes Service)
- **aws-eks-mcp.json** - AWS EKS operations
  - Package: `awslabs.eks-mcp-server@latest`
  - Local EKS cluster management

- **aws-eks-remote-mcp.json** - AWS EKS remote operations
  - Remote endpoint: `https://eks-mcp.us-west-2.api.aws/mcp`
  - Uses `mcp-proxy-for-aws@latest`
  - Extensive auto-approvals for EKS and Kubernetes operations including:
    - Cluster management: `manage_eks_stacks`, `list_eks_resources`, `describe_eks_resource`
    - Kubernetes operations: `list_k8s_resources`, `read_k8s_resource`, `manage_k8s_resource`, `apply_yaml`
    - Monitoring: `get_pod_logs`, `get_k8s_events`, `get_cloudwatch_logs`, `get_cloudwatch_metrics`
    - IAM: `get_policies_for_role`, `add_inline_policy`
    - Documentation: `search_eks_documentation`, `search_eks_troubleshooting_guide`

### AWS Infrastructure as Code

- **aws-cdk-mcp.json** - AWS CDK operations
  - Package: `awslabs.cdk-mcp-server@latest`
  - Cloud Development Kit integration for infrastructure as code

- **aws-terraform-mcp.json** - AWS Terraform integration
  - Package: `awslabs.terraform-mcp-server@latest`
  - Terraform operations and AWS resource management

- **aws-terraform-docker-mcp.json** - AWS Terraform via Docker
  - Image: `mcp/aws-terraform`
  - Containerized Terraform integration

### AWS Cost Management

- **aws-pricing-mcp.json** - AWS pricing information
  - Package: `awslabs.aws-pricing-mcp-server@latest`
  - Auto-approves: `get_pricing_service_codes`, `get_pricing_service_attributes`, `get_pricing_attribute_values`, `get_pricing`
  - Query AWS service pricing and cost estimates

- **aws-cost-explora-mcp.json** - AWS Cost Explorer integration
  - Package: `awslabs.cost-explorer-mcp-server@latest`
  - Analyze historical AWS spending and usage patterns

### AWS Visualization

- **aws-diagram-mcp.json** - AWS architecture diagrams
  - Package: `awslabs.aws-diagram-mcp-server`
  - Generate visual representations of AWS architectures

### Development Tools

- **browser-mcp.json** - Browser automation
  - Package: `@browsermcp/mcp@latest` (npx)
  - Web interaction and browser automation capabilities

- **chrome-devtools-mcp.json** - Chrome DevTools integration
  - Package: `chrome-devtools-mcp@latest` (npx)
  - Browser debugging and automation via Chrome DevTools Protocol

- **github-docker-mcp.json** - GitHub integration
  - Image: `ghcr.io/github/github-mcp-server`
  - Requires: `GITHUB_TOKEN` environment variable
  - GitHub repository operations and API access

- **k8s-docker-mcp.json** - Kubernetes management
  - Image: `mcp/kubernetes`
  - Kubernetes cluster operations via Docker

- **container-use-mcp.json** - Container utilities
  - Command: `cu stdio`
  - Container usage and management tools

### Productivity & Utilities

- **amap-mcp.json** - Amap (高德地图) mapping service
  - Package: `amap-mcp-server`
  - Requires: `AMAP_MAPS_API_KEY` environment variable
  - Chinese mapping and location services

- **ms-office-mcp.json** - Microsoft Office document processing
  - Package: `mcp-server-office@latest`
  - Auto-approves: `read_docx`, `extract_text`, `get_document_info`
  - Read and process Word documents and other Office files

- **time-docker-mcp.json** - Time and date utilities
  - Image: `mcp/time`
  - Time zone conversions and date calculations

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
