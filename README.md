# Agent Skills Demo

This repository demonstrates GitHub Copilot Agent Skills and MCP (Model Context Protocol) server integrations.

## What are Agent Skills?

Agent Skills are folders of instructions that teach GitHub Copilot how to perform specialized coding tasks automatically. Skills enhance Copilot's capabilities by providing context-specific guidance for common development workflows.

## MCP Server Integration

### What is MCP?

The Model Context Protocol (MCP) is an open protocol that enables AI assistants to securely connect to external data sources and tools. MCP servers provide context and capabilities to AI assistants like Claude and GitHub Copilot.

### Configured MCP Servers

This repository demonstrates integration with the following MCP servers:

#### 1. Atlassian MCP Server

The Atlassian MCP server connects to Jira and Confluence, enabling AI assistants to interact with your Atlassian workspace.

**VS Code Configuration:**
```json
{
  "atlassian/atlassian-mcp-server": {
    "type": "http",
    "url": "https://mcp.atlassian.com/v1/sse",
    "gallery": "https://api.mcp.github.com",
    "version": "1.0.0"
  }
}
```

**Connected Workspace:**
- Jira URL: https://ecanarys-team-y31whl7q.atlassian.net/jira/projects

#### 2. GitHub MCP Server

The GitHub MCP server enables AI assistants to interact with GitHub repositories, issues, pull requests, and more.

### Connecting Atlassian MCP to GitHub

To integrate your Atlassian MCP server with GitHub for seamless issue tracking and project management:

#### Option 1: Native Atlassian-GitHub Integration

1. **Configure Jira-GitHub Integration:**
   - Go to your Jira project settings
   - Navigate to Apps → GitHub → Connect GitHub
   - Authenticate with your GitHub account
   - Select the repositories you want to link

2. **Link Jira Issues to GitHub:**
   - Reference Jira issues in commit messages: `fixes ABC-123`
   - Reference Jira issues in PR descriptions
   - Create automation rules in Jira to update issues based on GitHub events

#### Option 2: Use Both MCP Servers Together

You can use both Atlassian and GitHub MCP servers simultaneously in Claude Code or GitHub Copilot:

**Combined Configuration Example (for Claude Desktop):**
```json
{
  "mcpServers": {
    "atlassian": {
      "type": "http",
      "url": "https://mcp.atlassian.com/v1/sse",
      "gallery": "https://api.mcp.github.com",
      "version": "1.0.0"
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>"
      }
    }
  }
}
```

**For GitHub Copilot in VS Code:**
GitHub Copilot has built-in GitHub integration. To add Atlassian MCP:
1. Install the Atlassian extension for VS Code
2. Configure MCP servers in your workspace settings
3. Use skills to coordinate between both platforms

#### Option 3: Automation with GitHub Actions

Create workflows that sync between Atlassian and GitHub:

```yaml
name: Sync Jira with GitHub
on:
  issues:
    types: [opened, closed]
  pull_request:
    types: [opened, closed, merged]

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Update Jira Issue
        uses: atlassian/gajira-transition@v3
        with:
          issue: ${{ github.event.issue.title }}
          transition: "Done"
```

## Available Skills

### GitHub Issues Management
- **Location:** `.github/skills/github-issues/`
- **Purpose:** Create, update, and manage GitHub issues using MCP tools
- **Usage:** Ask Copilot to "create a bug issue" or "update issue #123"

### Atlassian-GitHub Integration
- **Location:** `.github/skills/atlassian-github-integration/`
- **Purpose:** Coordinate work between Jira and GitHub
- **Usage:** Ask Copilot to "sync Jira issue ABC-123 with GitHub PR"

## Getting Started

1. **Clone this repository**
2. **Configure your MCP servers** (see sections above)
3. **Open in VS Code** with GitHub Copilot or Claude Code
4. **Try the skills** by asking natural language questions

## Example Prompts

- "Create a GitHub issue for the login bug"
- "Show me Jira issues assigned to me"
- "Link this PR to Jira ticket ABC-123"
- "Create a Jira ticket from GitHub issue #45"

## Troubleshooting

### Atlassian MCP Not Working in GitHub.com

If your Atlassian MCP server works in VS Code but not on GitHub.com:

1. **GitHub.com uses GitHub Copilot, not Claude:** MCP servers configured for Claude Desktop won't automatically work on GitHub.com
2. **Solution:** Use GitHub's native integrations:
   - Install the Jira GitHub app from the GitHub Marketplace
   - Configure it in your repository settings
   - Use automation rules to sync status between platforms

3. **For Claude Code CLI:** MCP servers configured for Claude Desktop work in Claude Code CLI
4. **For GitHub Copilot in VS Code:** Install the Atlassian extension and configure workspace MCP settings

### Authentication Issues

- Ensure your Atlassian API tokens are valid
- Check that GitHub personal access tokens have the required scopes
- Verify network connectivity to both services

## Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [GitHub Copilot Skills](https://docs.github.com/copilot/concepts/agents/about-agent-skills)
- [Atlassian MCP Server](https://github.com/atlassian/atlassian-mcp-server)
- [GitHub-Jira Integration](https://github.com/marketplace/jira-software-github)

## Contributing

Feel free to add more skills and integration examples to this repository!
