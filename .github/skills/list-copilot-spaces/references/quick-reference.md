# Copilot Spaces Quick Reference

## Quick Access

### Primary Dashboard
🔗 **[https://github.com/copilot/spaces](https://github.com/copilot/spaces)**

This is your main hub for:
- Viewing all available spaces
- Creating new spaces
- Managing existing spaces
- Discovering organization spaces

### Direct Actions

| Action | Method |
|--------|--------|
| **List all spaces** | Visit [github.com/copilot/spaces](https://github.com/copilot/spaces) |
| **Search spaces** | Use search bar on spaces dashboard |
| **Filter by type** | Use tabs: "Your spaces", "Organization", "Starred" |
| **Access in IDE** | Open Copilot panel → Select space dropdown |

## Common Scenarios

### Scenario 1: First Time User
**"I want to see what Copilot Spaces are"**

1. Go to https://github.com/copilot/spaces
2. Browse the interface to see available spaces
3. Click on any space to see its contents and context
4. Review the documentation links in the space

### Scenario 2: Finding Project Space
**"I need to find the space for my team's project"**

1. Visit https://github.com/copilot/spaces
2. Click the "Organization" tab
3. Use search to filter by project name
4. Star the space for quick access

### Scenario 3: Using Spaces in IDE
**"How do I access spaces while coding?"**

1. Ensure GitHub MCP server is configured in your IDE
2. Open the Copilot panel
3. Look for the "Space" selector dropdown
4. Select your desired space
5. Copilot will now use that space's context

## Space Types

```
Personal Spaces
└─ Created by you
└─ Private by default
└─ Full control over content

Organization Spaces
└─ Shared across org/team
└─ Managed by admins
└─ Centralized knowledge

Starred Spaces
└─ Quick access bookmarks
└─ Any space you've starred
└─ Personal favorites list
```

## Key Features

### What You Can Add to a Space
- ✅ GitHub repositories (entire repos or specific files)
- ✅ Issues and pull requests
- ✅ Documentation files (Markdown, text)
- ✅ Design documents and specs
- ✅ Custom instructions (coding style, frameworks)
- ✅ External links (within limits)

### What Spaces Provide
- 🎯 **Context-aware responses**: Copilot references space content
- 📝 **Custom guidelines**: Enforce project-specific standards
- 🔄 **Auto-sync**: GitHub content stays updated
- 👥 **Team collaboration**: Share knowledge across the team
- 🔒 **Access control**: Manage who can view/edit

## Troubleshooting

### Can't See Any Spaces
**Problem**: Dashboard is empty

**Solutions**:
- ✓ Verify you have GitHub Copilot access
- ✓ Check if you're signed in to the correct account
- ✓ Ask your organization admin about available spaces
- ✓ Create your first personal space

### Space Not Available in IDE
**Problem**: Can't select space in IDE

**Solutions**:
- ✓ Confirm GitHub MCP server is installed and configured
- ✓ Restart your IDE after MCP configuration
- ✓ Check IDE supports Spaces feature (see GitHub docs)
- ✓ Verify authentication with GitHub account

### Space Context Not Working
**Problem**: Copilot doesn't seem to use space context

**Solutions**:
- ✓ Ensure space is selected in IDE
- ✓ Verify space contains relevant content
- ✓ Check that content is synced (for GitHub resources)
- ✓ Try explicitly mentioning files from the space in your prompt

## Best Practices

### Creating Effective Spaces
1. **Focus on one project or domain** - Don't mix unrelated contexts
2. **Include comprehensive docs** - README, architecture, API docs
3. **Add custom instructions** - Code style, naming conventions, frameworks
4. **Keep it updated** - Review and refresh content periodically
5. **Use descriptive names** - Make it easy to identify the space's purpose

### Managing Multiple Spaces
- ⭐ Star your most-used spaces
- 🏷️ Use clear, descriptive names
- 📁 Organize by project or team
- 🔄 Regular audits to remove outdated spaces
- 📋 Document space purpose and contents

### Team Collaboration
- 📢 Announce new organization spaces to the team
- 📝 Add clear descriptions to each space
- 👥 Assign space maintainers
- 🔍 Review access permissions regularly
- 💬 Collect feedback on space usefulness

## Environment Compatibility

### ✅ Where Spaces Work
- **GitHub Web** (github.com/copilot/spaces) - Full management
- **VS Code** - Full integration with GitHub MCP server
- **Claude Desktop** - With GitHub MCP server configured
- **GitHub CLI** - With MCP integration
- **IDEs with GitHub Copilot** - Check specific IDE support

### ⚠️ Limited Support
- **GitHub.com website** - Only the dedicated Spaces dashboard at github.com/copilot/spaces is fully supported. General GitHub browsing doesn't provide space context.

### ❌ Where Spaces Don't Work
- **Legacy Copilot versions** - Update to latest version for full support
- **Unsupported IDEs** - Check GitHub documentation for supported editors

## Useful Links

| Resource | URL |
|----------|-----|
| **Spaces Dashboard** | https://github.com/copilot/spaces |
| **Official Docs** | https://docs.github.com/en/copilot/concepts/context/spaces |
| **How-to Guide** | https://docs.github.com/en/copilot/how-tos/provide-context/use-copilot-spaces |
| **Blog Announcement** | https://github.blog/ai-and-ml/github-copilot/github-copilot-spaces-bring-the-right-context-to-every-suggestion/ |
| **GitHub Copilot Docs** | https://docs.github.com/en/copilot |

## FAQ

**Q: Do I need a special license for Spaces?**
A: Spaces are included with GitHub Copilot subscription.

**Q: Can I share spaces with external collaborators?**
A: Yes, you can share with anyone who has GitHub Copilot access and appropriate repository permissions.

**Q: How many spaces can I create?**
A: Check your organization's limits. Typically, there are generous limits for both personal and organization spaces.

**Q: Are spaces private by default?**
A: Yes, personal spaces are private. You must explicitly share them.

**Q: Can I use spaces for private repositories?**
A: Yes, spaces can include private repositories if you have access to them.

**Q: How often does space content sync?**
A: GitHub resources (repos, issues, PRs) sync automatically. Manual content may need updating.

## Getting Help

If you need assistance:
1. Check the [official documentation](https://docs.github.com/en/copilot/concepts/context/spaces)
2. Visit [GitHub Community](https://github.community)
3. Contact your organization's GitHub admin
4. Open a support ticket with GitHub Support

---

*This quick reference is part of the list-copilot-spaces skill for GitHub Copilot agents.*
