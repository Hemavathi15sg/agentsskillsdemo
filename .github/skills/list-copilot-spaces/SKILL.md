---
name: list-copilot-spaces
description: 'List and discover GitHub Copilot Spaces. Use this skill when users want to view available Copilot Spaces, find spaces they have access to, or learn about space management. Triggers on requests like "list copilot spaces", "show my spaces", "what spaces do I have", or any Copilot Spaces discovery task.'
---

# List Copilot Spaces

Discover and manage GitHub Copilot Spaces - reusable, context-driven workspaces that provide project-specific context to GitHub Copilot.

## What are Copilot Spaces?

GitHub Copilot Spaces are curated workspaces that bundle together project-specific context including:
- Code repositories
- Documentation files
- Pull requests and issues
- Design notes and specifications
- Custom instructions and guidelines

When you interact with Copilot within a Space, its responses are grounded in this curated context, resulting in more accurate, consistent, and project-aware AI assistance.

## Available Methods

### Web Dashboard
The primary way to list and manage Copilot Spaces:

**URL**: [https://github.com/copilot/spaces](https://github.com/copilot/spaces)

This dashboard shows:
- All spaces you have access to
- Spaces you've created
- Spaces shared within your organization
- Starred spaces
- Space metadata (name, description, visibility)

### IDE Integration
Access spaces directly from your IDE if you have GitHub MCP server configured:
- View available spaces in Copilot panel
- Switch between spaces
- See space context in real-time

### API Access (Advanced)
For automation and integration with external tools, use GitHub's API endpoints to programmatically list spaces (requires proper authentication and permissions).

## Workflow

1. **Determine intent**: List all spaces, find specific space, or learn about space features?
2. **Choose method**: Web dashboard (most common), IDE integration, or API
3. **Navigate**: Direct user to appropriate resource
4. **Provide context**: Explain what spaces are and their benefits

## Common Use Cases

### List All Available Spaces
**User Request**: "Show me all my Copilot Spaces"

**Response**:
To see all your GitHub Copilot Spaces:

1. Visit [https://github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Sign in with your GitHub account if not already authenticated
3. Browse all spaces you have access to, including:
   - Your personal spaces
   - Organization-shared spaces
   - Spaces you've starred

### Find Specific Space
**User Request**: "How do I find a space for Project X?"

**Response**:
1. Go to [https://github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Use the search bar to filter by space name
3. Check the filters to narrow by:
   - Your spaces
   - Organization spaces
   - Starred spaces

### Learn About Space Features
**User Request**: "What can I do with Copilot Spaces?"

**Response**:
GitHub Copilot Spaces provide:

**Curated Context**
- Add repositories, files, issues, PRs, docs
- Copilot uses all this as reference for better suggestions

**Custom Instructions**
- Define code style preferences
- Specify frameworks and best practices
- Set project-specific guidelines

**Collaboration**
- Share spaces within teams or organizations
- Control visibility (public, shared, private)
- Centralize project knowledge

**Auto-sync**
- Linked GitHub content stays up-to-date automatically
- Always have the latest context

**IDE Integration**
- Access spaces directly from VS Code and other IDEs
- Use space context while coding

## Key Benefits

| Benefit | Description |
|---------|-------------|
| **Better Context** | More relevant and accurate Copilot responses |
| **Faster Onboarding** | New team members get organized project knowledge |
| **Consistency** | Ensure Copilot follows internal standards |
| **Knowledge Sharing** | Reduce repetitive Q&A across the team |
| **Always Updated** | Auto-sync keeps context current |

## Space Visibility Options

| Type | Access |
|------|--------|
| **Private** | Only you can view and use |
| **Shared** | Specific users or teams |
| **Organization** | All organization members |
| **Public** | Anyone on GitHub (if enabled) |

## Tips

- **Start with the web dashboard**: It's the most comprehensive view of all your spaces
- **Starred spaces**: Mark frequently used spaces with a star for quick access
- **Organization spaces**: Check with your team admin about shared organizational spaces
- **IDE integration**: For the best experience, configure the GitHub MCP server in your IDE
- **Keep spaces focused**: Create separate spaces for different projects or contexts
- **Regular updates**: Review and update space content periodically to keep it relevant

## Quick Reference

See [references/quick-reference.md](references/quick-reference.md) for:
- Quick access links
- Common commands
- Troubleshooting tips
- Additional resources

## Related Documentation

- [About GitHub Copilot Spaces](https://docs.github.com/en/copilot/concepts/context/spaces)
- [Using GitHub Copilot Spaces](https://docs.github.com/en/copilot/how-tos/provide-context/use-copilot-spaces)
- [GitHub Copilot Spaces Blog](https://github.blog/ai-and-ml/github-copilot/github-copilot-spaces-bring-the-right-context-to-every-suggestion/)

## Notes

- Copilot Spaces require a GitHub Copilot subscription
- Space access depends on your GitHub organization's permissions
- Not all IDE integrations support Spaces yet (check GitHub's documentation for the latest)
- The web dashboard (github.com/copilot/spaces) allows full space management through the UI
- MCP server tools for programmatic space access work in desktop environments (VS Code, Claude Desktop, CLI) but not through the GitHub.com website
