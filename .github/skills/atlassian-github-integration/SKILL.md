---
name: atlassian-github-integration
description: 'Coordinate and sync work between Atlassian Jira and GitHub. Use this skill when users want to link Jira issues with GitHub PRs/issues, sync status between platforms, or manage cross-platform workflows. Triggers on requests like "link Jira ticket to PR", "sync status with Jira", "create GitHub issue from Jira ticket", or any Atlassian-GitHub coordination task.'
---

# Atlassian-GitHub Integration

Coordinate work between Atlassian Jira and GitHub repositories using MCP tools.

## Overview

This skill helps manage the connection between Jira issues and GitHub work items (issues, pull requests, commits) to maintain synchronized project tracking across both platforms.

## Available MCP Tools

### Atlassian MCP Tools
| Tool | Purpose |
|------|---------|
| `mcp__atlassian__get_issue` | Fetch Jira issue details |
| `mcp__atlassian__search_issues` | Search Jira issues |
| `mcp__atlassian__update_issue` | Update Jira issue status/fields |
| `mcp__atlassian__create_issue` | Create new Jira issues |
| `mcp__atlassian__add_comment` | Add comments to Jira issues |

### GitHub MCP Tools
| Tool | Purpose |
|------|---------|
| `mcp__github-mcp-server__issue_read` | Get GitHub issue details |
| `mcp__github-mcp-server__list_issues` | List GitHub issues |
| `mcp__github-mcp-server__pull_request_read` | Get PR details |
| `mcp__github-mcp-server__list_pull_requests` | List pull requests |

## Integration Patterns

### 1. Link Jira Issue to GitHub PR

When creating or updating a GitHub PR, reference the Jira issue:

**PR Title Format:**
```
[ABC-123] Add user authentication feature
```

**PR Description Format:**
```markdown
## Summary
Implements user authentication with OAuth2.

## Jira Issue
- Issue: [ABC-123](https://your-workspace.atlassian.net/browse/ABC-123)
- Status: In Progress
- Assignee: @username

## Changes
- Added OAuth2 provider configuration
- Implemented login/logout endpoints
- Added user session management

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed
```

### 2. Sync Status Between Platforms

**Workflow: GitHub → Jira**
```
PR opened → Jira issue: "In Progress"
PR merged → Jira issue: "Done"
PR closed (not merged) → Jira issue: "Cancelled"
```

**Workflow: Jira → GitHub**
```
Jira issue "In Review" → Add reviewers to GitHub PR
Jira issue "Blocked" → Add "blocked" label to GitHub issue
Jira issue "Done" → Close related GitHub issues
```

### 3. Create GitHub Issue from Jira Ticket

When syncing a Jira ticket to GitHub:

1. **Fetch Jira issue details** using `mcp__atlassian__get_issue`
2. **Transform to GitHub format:**
   - Title: `[JIRA-KEY] {summary}`
   - Body: Convert Jira description to GitHub markdown
   - Labels: Map Jira issue type to GitHub labels
   - Assignees: Map Jira assignees to GitHub users

3. **Create GitHub issue** using `mcp__github-mcp-server__create_issue`
4. **Link back to Jira:** Add GitHub issue URL as comment in Jira

### 4. Create Jira Issue from GitHub

When syncing a GitHub issue to Jira:

1. **Fetch GitHub issue** using `mcp__github-mcp-server__issue_read`
2. **Transform to Jira format:**
   - Summary: GitHub issue title
   - Description: GitHub issue body (convert markdown to Jira format)
   - Issue Type: Map GitHub labels to Jira issue types
   - Priority: Infer from GitHub labels (high-priority, critical, etc.)

3. **Create Jira issue** using `mcp__atlassian__create_issue`
4. **Link back to GitHub:** Update GitHub issue with Jira key

## Automation Examples

### Example 1: Link PR to Jira

**User Request:** "Link this PR to Jira ticket ABC-123"

**Actions:**
1. Get current GitHub PR details
2. Fetch Jira issue ABC-123 details
3. Update PR description to include Jira link
4. Add comment to Jira issue with PR URL
5. Update Jira status to "In Progress" if not already

### Example 2: Sync Status on PR Merge

**User Request:** "Mark Jira ticket as done now that PR is merged"

**Actions:**
1. Extract Jira key from PR title/description
2. Call `mcp__atlassian__update_issue` with transition to "Done"
3. Add comment to Jira: "Completed in PR #123"
4. Optionally: Add deployment/release information

### Example 3: Create Tracking Issue

**User Request:** "Create a GitHub issue for Jira ticket XYZ-456"

**Actions:**
```
1. Fetch Jira issue XYZ-456
2. Map fields:
   - Title: "[XYZ-456] {jira.summary}"
   - Body:
     "## From Jira Ticket
     **Jira Issue:** [XYZ-456](https://workspace.atlassian.net/browse/XYZ-456)
     **Status:** {jira.status}
     **Priority:** {jira.priority}

     ## Description
     {jira.description}

     ## Acceptance Criteria
     {jira.acceptanceCriteria}"

3. Create GitHub issue
4. Add comment to Jira with GitHub issue URL
```

## Naming Conventions

### Commit Messages
```
[ABC-123] Brief description of change

Detailed explanation if needed.

Jira: https://workspace.atlassian.net/browse/ABC-123
```

### Branch Names
```
feature/ABC-123-user-authentication
bugfix/XYZ-456-login-error
hotfix/DEF-789-critical-security-fix
```

### PR Titles
```
[ABC-123] Add user authentication
[XYZ-456] Fix login error on mobile
[DEF-789] Security patch for XSS vulnerability
```

## Status Mapping

### Jira Status → GitHub State
| Jira Status | GitHub Action |
|-------------|---------------|
| To Do | Issue: open, Label: "todo" |
| In Progress | Issue: open, Label: "in progress" |
| In Review | PR: open, Request reviewers |
| Done | Issue/PR: closed |
| Cancelled | Issue/PR: closed, Label: "wontfix" |
| Blocked | Label: "blocked" |

### GitHub State → Jira Status
| GitHub Event | Jira Transition |
|--------------|-----------------|
| PR opened | "In Progress" |
| PR ready for review | "In Review" |
| PR approved | "Ready to Merge" |
| PR merged | "Done" |
| PR closed (not merged) | "Cancelled" |
| Issue opened | "To Do" |
| Issue closed | "Done" |

## Best Practices

1. **Always include Jira keys** in PR titles and commit messages
2. **Keep descriptions in sync** between platforms
3. **Use consistent labels** that map between Jira and GitHub
4. **Update both platforms** when status changes
5. **Link related work** (mention related PRs/issues in both systems)
6. **Automate where possible** using GitHub Actions or Jira automation rules

## Troubleshooting

### Common Issues

**Jira issue not found:**
- Verify the issue key is correct (e.g., ABC-123)
- Check user has permission to access the issue
- Ensure MCP server has valid Atlassian credentials

**GitHub API rate limits:**
- Use personal access tokens with appropriate scopes
- Batch operations when possible
- Check rate limit status before bulk operations

**Status sync fails:**
- Verify Jira workflow allows the transition
- Check field permissions in both systems
- Ensure issue types support the status change

## Configuration Requirements

### Atlassian MCP Server
```json
{
  "atlassian": {
    "type": "http",
    "url": "https://mcp.atlassian.com/v1/sse",
    "credentials": {
      "api_token": "<atlassian-api-token>",
      "email": "<your-email>",
      "cloud_id": "<cloud-id>"
    }
  }
}
```

### GitHub MCP Server
```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_PERSONAL_ACCESS_TOKEN": "<token-with-repo-scope>"
    }
  }
}
```

## Tips

- **Batch operations:** When syncing multiple items, fetch all data first, then perform updates
- **Error handling:** Always verify issue/PR exists before attempting updates
- **User mapping:** Maintain a mapping file for Jira users → GitHub usernames
- **Custom fields:** Check for custom Jira fields that should be synced
- **Webhooks:** Consider using webhooks for real-time sync instead of polling

## Resources

- [Jira REST API Documentation](https://developer.atlassian.com/cloud/jira/platform/rest/v3/)
- [GitHub REST API Documentation](https://docs.github.com/rest)
- [MCP Protocol Specification](https://modelcontextprotocol.io/)
- [GitHub-Jira Integration App](https://github.com/marketplace/jira-software-github)
