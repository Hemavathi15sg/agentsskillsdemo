# Atlassian-GitHub Integration Setup Guide

## Understanding MCP Server vs GitHub.com Integration

### The Key Difference

**MCP Servers work in desktop environments:**
- Claude Desktop app
- VS Code with Claude Code extension
- Claude Code CLI

**MCP Servers DO NOT work on GitHub.com:**
- GitHub.com website uses GitHub Copilot
- GitHub Copilot has its own integration mechanisms
- MCP servers configured for Claude won't transfer to GitHub.com

## Your Current Situation

You have configured the Atlassian MCP server in VS Code:

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

This configuration:
- ✅ Works in VS Code with Claude extensions
- ✅ Works in Claude Desktop
- ✅ Works in Claude Code CLI
- ❌ Does NOT work on GitHub.com website
- ❌ Does NOT automatically integrate with GitHub repositories

## Solution: Connect Atlassian to GitHub

### Option 1: Native Jira-GitHub Integration (Recommended)

This is the official way to connect Jira to GitHub and works everywhere, including GitHub.com.

#### Step-by-Step Setup

1. **Install Jira GitHub App**
   - Go to: https://github.com/marketplace/jira-software-github
   - Click "Set up a plan"
   - Select your GitHub organization
   - Click "Install it for free"

2. **Connect to Your Atlassian Workspace**
   - After installation, you'll be redirected to Atlassian
   - Select your Atlassian site: `ecanarys-team-y31whl7q.atlassian.net`
   - Authorize the connection

3. **Select Repositories**
   - Choose which GitHub repositories to link
   - Click "Continue"

4. **Configure Settings (in Jira)**
   - Go to: https://ecanarys-team-y31whl7q.atlassian.net/jira/settings/apps
   - Find "GitHub" in the apps list
   - Click "Configure"
   - Set up automation rules:
     - Auto-transition issues when PRs are merged
     - Link commits to issues automatically
     - Sync status between platforms

#### How to Use After Setup

**In GitHub commits:**
```bash
git commit -m "ABC-123 Fix login bug"
# Automatically links to Jira ticket ABC-123
```

**In GitHub PR descriptions:**
```markdown
## Summary
Fix authentication issue

## Related Jira Tickets
- ABC-123
- ABC-124

Fixes ABC-123
```

**In Jira issues:**
- Development panel shows linked PRs and branches
- Commits mentioning the issue key appear automatically
- Status can auto-transition based on PR state

### Option 2: Use Both MCP Servers Together (For Claude/VS Code)

If you want to use AI assistants to coordinate between Jira and GitHub, configure both MCP servers.

#### Configuration for Claude Desktop

Edit your Claude Desktop config file:

**Location:**
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

**Add both servers:**
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
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN_HERE"
      }
    }
  }
}
```

**Get GitHub Personal Access Token:**
1. Go to: https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes:
   - `repo` (full repository access)
   - `read:org` (read organization data)
   - `workflow` (if you need to manage workflows)
4. Generate and copy the token
5. Paste it in the config above

#### Configuration for VS Code

**For VS Code with Claude Code:**

Create or edit `.vscode/settings.json` in your project:

```json
{
  "claude.mcpServers": {
    "atlassian": {
      "type": "http",
      "url": "https://mcp.atlassian.com/v1/sse"
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

**Set environment variable:**
```bash
# In your .bashrc or .zshrc
export GITHUB_TOKEN="ghp_YOUR_TOKEN_HERE"

# Or create .env file in your project
echo "GITHUB_TOKEN=ghp_YOUR_TOKEN_HERE" > .env
```

### Option 3: GitHub Actions for Automation

Create workflows that sync between platforms automatically.

**Example: `.github/workflows/jira-sync.yml`**

```yaml
name: Sync with Jira

on:
  pull_request:
    types: [opened, closed, reopened]
  issues:
    types: [opened, closed, reopened]

jobs:
  sync-jira:
    runs-on: ubuntu-latest
    steps:
      - name: Extract Jira Issue Key
        id: jira-key
        run: |
          # Extract ABC-123 from PR/issue title
          KEY=$(echo "${{ github.event.pull_request.title || github.event.issue.title }}" | grep -oP '\b[A-Z]+-\d+\b' | head -1)
          echo "key=$KEY" >> $GITHUB_OUTPUT

      - name: Update Jira Issue
        if: steps.jira-key.outputs.key != ''
        uses: atlassian/gajira-transition@v3
        with:
          issue: ${{ steps.jira-key.outputs.key }}
          transition: ${{ github.event.action == 'closed' && 'Done' || 'In Progress' }}
        env:
          JIRA_BASE_URL: https://ecanarys-team-y31whl7q.atlassian.net
          JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
          JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}

      - name: Add Comment to Jira
        if: steps.jira-key.outputs.key != ''
        uses: atlassian/gajira-comment@v3
        with:
          issue: ${{ steps.jira-key.outputs.key }}
          comment: |
            GitHub PR: ${{ github.event.pull_request.html_url || github.event.issue.html_url }}
            Status: ${{ github.event.action }}
            Updated at: ${{ github.event.pull_request.updated_at || github.event.issue.updated_at }}
```

**Setup secrets in GitHub:**
1. Go to your repository settings
2. Navigate to Secrets and variables → Actions
3. Add:
   - `JIRA_USER_EMAIL`: Your Atlassian email
   - `JIRA_API_TOKEN`: Your Atlassian API token

**Get Atlassian API Token:**
1. Go to: https://id.atlassian.com/manage-profile/security/api-tokens
2. Click "Create API token"
3. Give it a name and copy the token
4. Store it in GitHub secrets

## Practical Examples

### Example 1: Create PR with Jira Link

```bash
# Create a branch with Jira key
git checkout -b feature/ABC-123-add-authentication

# Make your changes
git add .
git commit -m "ABC-123 Implement OAuth2 authentication"

# Push and create PR
git push -u origin feature/ABC-123-add-authentication
gh pr create --title "[ABC-123] Add OAuth2 authentication" \
  --body "## Summary
Implements OAuth2 authentication flow

## Jira Issue
https://ecanarys-team-y31whl7q.atlassian.net/browse/ABC-123

## Changes
- Added OAuth2 provider
- Implemented callback handler
- Added session management

Closes ABC-123"
```

### Example 2: Using Claude with Both MCP Servers

Once both MCP servers are configured, you can ask Claude:

```
"Check Jira ticket ABC-123 and create a matching GitHub issue in the repository"
```

Claude will:
1. Use Atlassian MCP to fetch ABC-123 details
2. Use GitHub MCP to create a corresponding issue
3. Format the content appropriately
4. Link both items together

```
"Update Jira ticket ABC-456 to 'In Progress' because I just opened PR #123"
```

Claude will:
1. Use GitHub MCP to verify PR #123 exists
2. Use Atlassian MCP to transition ABC-456 status
3. Add a comment with the PR link

### Example 3: Bulk Sync

```
"Find all open GitHub issues with 'needs-jira' label and create corresponding Jira tickets"
```

Claude will:
1. Use GitHub MCP to search for labeled issues
2. For each issue, use Atlassian MCP to create a ticket
3. Update GitHub issues with Jira references
4. Remove the 'needs-jira' label

## Troubleshooting

### "Atlassian MCP works in VS Code but not GitHub.com"

**This is expected behavior.** MCP servers are client-side tools that only work in:
- Claude Desktop
- VS Code with Claude extensions
- Command-line Claude Code

**To use on GitHub.com:** Use Option 1 (Native Jira-GitHub Integration) instead.

### "Cannot authenticate with Atlassian MCP"

1. Check your Atlassian API token is valid
2. Verify your email and cloud ID in config
3. Ensure you have access to the Jira workspace
4. Try regenerating the API token

### "GitHub MCP not found"

1. Ensure Node.js is installed: `node --version`
2. The MCP server installs on first use with `npx`
3. Check internet connection
4. Try manual install: `npm install -g @modelcontextprotocol/server-github`

### "Rate limit exceeded"

**For GitHub:**
- Use authenticated requests (personal access token)
- Authenticated rate limit: 5,000 requests/hour
- Wait or increase delay between requests

**For Atlassian:**
- Free tier: Rate limits vary
- Use batch operations when possible
- Cache responses to reduce API calls

## Best Practices

1. **Always use Jira keys in branch names and commits**
   - Format: `ABC-123 Description`
   - Enables automatic linking

2. **Keep PR descriptions structured**
   - Include Jira link
   - Summary of changes
   - Testing notes
   - Clear acceptance criteria

3. **Automate status transitions**
   - PR opened → "In Progress"
   - PR merged → "Done"
   - PR closed → "Cancelled"

4. **Use consistent labels**
   - Map Jira issue types to GitHub labels
   - Use priority labels that match Jira

5. **Document your mapping**
   - Create a CONVENTIONS.md file
   - Define status mappings
   - List label mappings

## Next Steps

1. ✅ Install Jira GitHub App (Option 1)
2. ✅ Configure both MCP servers (Option 2)
3. ✅ Set up GitHub Actions (Option 3)
4. ✅ Test the integration with a sample PR
5. ✅ Train your team on conventions
6. ✅ Document your specific workflows

## Additional Resources

- [Jira-GitHub Integration Docs](https://support.atlassian.com/jira-cloud-administration/docs/integrate-with-github/)
- [GitHub Actions for Jira](https://github.com/marketplace?type=actions&query=jira)
- [Atlassian MCP Server](https://github.com/atlassian/atlassian-mcp-server)
- [GitHub MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/github)
- [MCP Protocol](https://modelcontextprotocol.io/)
