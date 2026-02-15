# Quick Reference: Atlassian MCP + GitHub Integration

## The Problem You're Experiencing

You have Atlassian MCP configured in VS Code, but it doesn't work on GitHub.com because:

1. **MCP servers are desktop tools** - They only work in Claude Desktop, VS Code, and CLI
2. **GitHub.com uses GitHub Copilot** - Not Claude, so MCP configs don't apply
3. **You need a different approach** - Use native GitHub-Jira integration

## Three Solutions (Choose What Fits Your Needs)

### ✅ Solution 1: Native Integration (Works Everywhere)

**Best for:** Teams who want automatic syncing between Jira and GitHub

**Setup:**
1. Install app: https://github.com/marketplace/jira-software-github
2. Connect to your workspace: `ecanarys-team-y31whl7q.atlassian.net`
3. Done! Now commits/PRs auto-link to Jira issues

**Usage:**
```bash
git commit -m "ABC-123 Fix bug"  # Auto-links to Jira
```

### ✅ Solution 2: Use Both MCP Servers (For AI Assistants)

**Best for:** Using Claude/AI to coordinate between platforms

**Claude Desktop Config** (`~/.config/Claude/claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "atlassian": {
      "type": "http",
      "url": "https://mcp.atlassian.com/v1/sse"
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN"
      }
    }
  }
}
```

**Get GitHub token:** https://github.com/settings/tokens (needs `repo` scope)

**Usage:**
Ask Claude: "Check Jira ABC-123 and create a GitHub issue for it"

### ✅ Solution 3: GitHub Actions (Automated Workflows)

**Best for:** Automatic status syncing without manual intervention

**Create:** `.github/workflows/jira-sync.yml`
```yaml
name: Sync Jira
on:
  pull_request:
    types: [opened, closed]
jobs:
  update-jira:
    runs-on: ubuntu-latest
    steps:
      - uses: atlassian/gajira-transition@v3
        with:
          issue: ${{ github.event.pull_request.title }}
          transition: "Done"
```

## Common Questions

**Q: Why doesn't my VS Code MCP config work on GitHub.com?**
A: GitHub.com is a website, not a desktop app. MCP servers only work in desktop environments (VS Code, Claude Desktop, CLI).

**Q: Can I use my existing Atlassian MCP setup?**
A: Yes! It works in VS Code and Claude Desktop. Just add GitHub MCP server alongside it.

**Q: What's the easiest way to connect them?**
A: Use Solution 1 (Native Integration). Install the GitHub app in Jira, and everything auto-links.

**Q: Do I need all three solutions?**
A: No. Choose based on needs:
- Just want auto-linking? → Solution 1
- Want AI assistance? → Solution 2
- Want automated workflows? → Solution 3

## Key Concepts

| Concept | Explanation |
|---------|-------------|
| **MCP Server** | Desktop tool that gives AI access to external services |
| **VS Code MCP** | Works only in VS Code, not on websites |
| **GitHub.com** | Website that uses GitHub Copilot (different from Claude) |
| **Native Integration** | Official Jira-GitHub connection (works everywhere) |
| **GitHub Actions** | Automation workflows in your repository |

## Quick Test

After setting up Solution 1 (Native Integration):

1. Create a branch:
   ```bash
   git checkout -b feature/ABC-123-test
   ```

2. Make a commit:
   ```bash
   git commit -m "ABC-123 Test integration"
   ```

3. Push and create PR:
   ```bash
   git push -u origin feature/ABC-123-test
   gh pr create --title "[ABC-123] Test integration"
   ```

4. Check Jira issue ABC-123 → You should see the commit/PR linked!

## Need Help?

- Full setup guide: `.github/skills/atlassian-github-integration/references/setup-guide.md`
- Skill documentation: `.github/skills/atlassian-github-integration/SKILL.md`
- Main README: `README.md`

## TL;DR

Your Atlassian MCP works in VS Code ✅
It doesn't work on GitHub.com ❌
**Solution:** Install Jira GitHub app for website integration ✅
