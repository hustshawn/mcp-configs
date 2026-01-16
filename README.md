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
- **npx servers**: `"command": "npx", "args": ["package@latest"]`
- **HTTP servers**: `"type": "http", "url": "https://endpoint.api.aws"` (direct connection to remote MCP endpoints)

## Available Configurations

### AWS Core Services

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-core-mcp.json` | uvx | Core AWS services integration using `awslabs.core-mcp-server@latest` |
| `aws-core-docker-mcp.json` | Docker | Core AWS services via `mcp/aws-core-mcp-server` container |
| `aws-core-doc-mcp.json` | uvx | Combined: Core AWS + Documentation servers |
| `aws-core-pricing-mcp.json` | uvx | Combined: Core AWS + Pricing servers with auto-approved pricing tools |

### AWS Documentation & Knowledge

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-doc-mcp.json` | uvx | AWS documentation access via `awslabs.aws-documentation-mcp-server@latest` |
| `aws-doc-docker-mcp.json` | Docker | AWS documentation via `mcp/aws-documentation` container |
| `aws-doc-ecs-mcp.json` | uvx | Combined: Documentation + Diagram generation servers |
| `aws-knowledge-mcp.json` | HTTP | Direct connection to AWS Knowledge Base at `https://knowledge-mcp.global.api.aws` |

### AWS Container Services

#### ECS (Elastic Container Service)

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-ecs-mcp.json` | uvx | Local ECS operations via `awslabs-ecs-mcp-server` with write/sensitive data controls |
| `aws-ecs-remote-mcp.json` | uvx (proxy) | Remote ECS via `https://ecs-mcp.us-west-2.api.aws/mcp` with extensive auto-approvals for troubleshooting |

#### EKS (Elastic Kubernetes Service)

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-eks-mcp.json` | uvx | Local EKS operations via `awslabs.eks-mcp-server@latest` |
| `aws-eks-remote-mcp.json` | uvx (proxy) | Remote EKS via `https://eks-mcp.us-west-2.api.aws/mcp` with comprehensive auto-approvals for cluster management, K8s operations, monitoring, and IAM |

### AWS Infrastructure as Code

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-cdk-mcp.json` | uvx | AWS CDK operations via `awslabs.cdk-mcp-server@latest` |
| `aws-terraform-mcp.json` | uvx | Terraform integration via `awslabs.terraform-mcp-server@latest` |
| `aws-terraform-docker-mcp.json` | Docker | Terraform via `mcp/aws-terraform` container |
| `aws-serverless-mcp.json` | uvx | Serverless operations via `awslabs.aws-serverless-mcp-server@latest` with write and sensitive data access |

### AWS Cost Management

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-pricing-mcp.json` | uvx | AWS pricing queries via `awslabs.aws-pricing-mcp-server@latest` with auto-approved pricing tools |
| `aws-cost-explora-mcp.json` | uvx | Cost Explorer analysis via `awslabs.cost-explorer-mcp-server@latest` |

### AWS Visualization

| Configuration | Type | Description |
|--------------|------|-------------|
| `aws-diagram-mcp.json` | uvx | Architecture diagrams via `awslabs.aws-diagram-mcp-server` |

### Development Tools

| Configuration | Type | Description |
|--------------|------|-------------|
| `browser-mcp.json` | npx | Browser automation via `@browsermcp/mcp@latest` |
| `chrome-devtools-mcp.json` | npx | Chrome DevTools Protocol integration via `chrome-devtools-mcp@latest` |
| `github-docker-mcp.json` | Docker | GitHub operations via `ghcr.io/github/github-mcp-server` (requires `GITHUB_TOKEN`) |
| `k8s-docker-mcp.json` | Docker | Kubernetes management via `mcp/kubernetes` container |
| `container-use-mcp.json` | CLI | Container utilities via `cu stdio` command |

### Productivity & Utilities

| Configuration | Type | Description |
|--------------|------|-------------|
| `amap-mcp.json` | uvx | Amap (高德地图) mapping service (requires `AMAP_MAPS_API_KEY`) |
| `ms-office-mcp.json` | uvx | Microsoft Office document processing via `mcp-server-office@latest` |
| `time-docker-mcp.json` | Docker | Time and date utilities via `mcp/time` container |

## Configuration Details

### Auto-Approved Tools by Configuration

Some configurations come with pre-approved tools for seamless operation:

**aws-knowledge-mcp.json**
- `aws___read_documentation`, `aws___search_documentation`

**aws-pricing-mcp.json / aws-core-pricing-mcp.json**
- `get_pricing_service_codes`, `get_pricing_service_attributes`, `get_pricing_attribute_values`, `get_pricing`

**aws-ecs-remote-mcp.json**
- `get_deployment_status`, `fetch_service_events`, `fetch_task_failures`, `fetch_task_logs`, `detect_image_pull_failures`, `fetch_network_configuration`, `get_task_definition_deletion_blockers`

**aws-eks-remote-mcp.json**
- Cluster: `manage_eks_stacks`, `list_eks_resources`, `describe_eks_resource`, `get_eks_insights`, `get_eks_vpc_config`
- Kubernetes: `list_k8s_resources`, `read_k8s_resource`, `manage_k8s_resource`, `apply_yaml`, `generate_app_manifest`, `list_api_versions`
- Monitoring: `get_pod_logs`, `get_k8s_events`, `get_cloudwatch_logs`, `get_cloudwatch_metrics`, `get_eks_metrics_guidance`
- IAM: `get_policies_for_role`, `add_inline_policy`
- Docs: `search_eks_documentation`, `search_eks_troubleshooting_guide`

**ms-office-mcp.json**
- `read_docx`, `extract_text`, `get_document_info`

**aws-doc-docker-mcp.json**
- `search_documentation`, `read_documentation`, `recommend`

**aws-core-docker-mcp.json**
- `prompt_understanding`

## Prerequisites

### For uvx-based servers

```bash
# Install uv and uvx
pip install uv
# or via homebrew
brew install uv
```

### For npx-based servers

```bash
# Ensure Node.js and npm are installed
node --version
npm --version
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
AWS_PROFILE=default
```

## Installation Examples

### Single Configuration

```bash
# Copy a specific config
cp aws-core-mcp.json .kiro/settings/mcp.json
```

### Combined Configurations

Some configuration files already combine multiple servers:

- `aws-core-doc-mcp.json` - Core AWS + Documentation
- `aws-core-pricing-mcp.json` - Core AWS + Pricing
- `aws-doc-ecs-mcp.json` - Documentation + Diagrams

### Manual Merge

To combine configurations manually, merge the `mcpServers` objects from multiple files:

```json
{
  "mcpServers": {
    "server-from-file-1": { ... },
    "server-from-file-2": { ... }
  }
}
```

## Configuration Options

| Option | Description |
|--------|-------------|
| `command` | The executable to run the MCP server (`uvx`, `docker`, `npx`, etc.) |
| `args` | Command line arguments |
| `env` | Environment variables for the server |
| `type` | Connection type (`stdio` for command-based, `http` for direct HTTP) |
| `url` | URL for HTTP-type connections |
| `disabled` | Set to `true` to disable the server |
| `autoApprove` | List of tools to auto-approve without user confirmation |
| `timeout` | Timeout in milliseconds for server operations |

## Troubleshooting

### Server Won't Start

1. Check that required dependencies are installed (uvx, Docker, npx, etc.)
2. Verify environment variables are set correctly
3. Check the MCP Server view in Kiro for error messages

### Permission Issues

- Ensure Docker daemon is running for Docker-based servers
- Check that environment files have correct permissions
- Verify API keys and tokens are valid

### HTTP Connection Issues

For HTTP-type servers (like `aws-knowledge-mcp.json`), ensure:
- Network connectivity to the endpoint
- Valid AWS credentials if required
- No firewall blocking the connection

## Contributing

Feel free to submit additional MCP server configurations via pull requests. Please ensure:

1. Configuration files are properly formatted JSON
2. Include necessary environment variable documentation
3. Test the configuration before submitting

## License

This repository is provided as-is for reference purposes. Individual MCP servers may have their own licenses and terms of use.
