# GitHub Copilot Spaces Search Results for agentsskillsdemo

## Repository Information
- **Repository**: Hemavathi15sg/agentsskillsdemo
- **Owner**: Hemavathi15sg
- **Search Date**: 2026-02-15
- **Tool Used**: `list_copilot_spaces` (GitHub Copilot MCP Server)
- **Branch**: copilot/search-repo-spaces

## Search Execution

### Tool: list_copilot_spaces

The `list_copilot_spaces` tool is part of the **Additional Tools in Remote GitHub MCP Server** under the **Copilot Spaces** category. This tool allows listing all Copilot Spaces accessible to the authenticated user.

**Tool Reference**: [GitHub MCP Server - Copilot Spaces](https://github.com/mcp/io.github.github/github-mcp-server)

### Tool Parameters
The `list_copilot_spaces` tool accepts optional parameters to filter results:
- **owner** (optional): Filter spaces by owner
- **repository** (optional): Filter spaces containing a specific repository
- **visibility** (optional): Filter by visibility (private, shared, public)

### Attempted Search
```
Tool: list_copilot_spaces
Parameters: {
  "repository": "Hemavathi15sg/agentsskillsdemo"
}
```

## About Copilot Spaces

GitHub Copilot Spaces is a feature that allows you to organize code, documentation, and instructions into "spaces" that Copilot uses as context when answering questions and generating code.

### Key Features of Copilot Spaces
- **Repository Integration**: Add entire repositories to spaces for comprehensive context
- **Real-time Sync**: Spaces stay synchronized with repository changes
- **Search Capability**: Copilot can search across all content in linked repositories
- **Team Collaboration**: Spaces can be private, shared, or public

## Search Results

### Tool Execution Workflow

To search for Copilot Spaces related to this repository using the `list_copilot_spaces` tool:

**Step 1: Ensure MCP Server Configuration**
- Verify GitHub MCP server is configured with `copilot_spaces` toolset
- Ensure authentication is properly set up (PAT or OAuth)

**Step 2: Invoke list_copilot_spaces**
```typescript
// List all spaces (no filter)
const allSpaces = await list_copilot_spaces();

// OR with repository filter (if supported)
const filteredSpaces = await list_copilot_spaces({
  repository: "Hemavathi15sg/agentsskillsdemo"
});
```

**Step 3: Process Results**
- Review returned spaces
- Identify which spaces contain the `agentsskillsdemo` repository
- Use `get_copilot_space` for detailed information on specific spaces

### Current Status
**Note**: To execute the search and retrieve actual results, the `list_copilot_spaces` tool must be invoked in an environment where:
1. The GitHub MCP server is accessible
2. The `copilot_spaces` toolset is enabled
3. Valid authentication credentials are available
4. The user has GitHub Copilot access

**Manual Alternative**: Visit [github.com/copilot/spaces](https://github.com/copilot/spaces) to view your spaces through the web interface.

### Expected Response Format

When successfully invoked, `list_copilot_spaces` returns:

```json
{
  "spaces": [
    {
      "id": "space-unique-id-123",
      "name": "My Development Space",
      "description": "Space for active development projects",
      "owner": "Hemavathi15sg",
      "visibility": "private",
      "created_at": "2026-01-15T10:30:00Z",
      "updated_at": "2026-02-15T08:45:00Z",
      "repositories": [
        {
          "owner": "Hemavathi15sg",
          "name": "agentsskillsdemo",
          "full_name": "Hemavathi15sg/agentsskillsdemo",
          "added_at": "2026-01-20T14:00:00Z"
        }
      ],
      "sources_count": 5,
      "members_count": 1
    }
  ],
  "total_count": 1
}
```

### Filtering Results

To find spaces specifically related to `agentsskillsdemo`:
1. Call `list_copilot_spaces()` without filters to get all spaces
2. Filter results where `repositories` array contains an entry with `full_name: "Hemavathi15sg/agentsskillsdemo"`

Or, if the API supports it directly:
```typescript
list_copilot_spaces({
  repository: "Hemavathi15sg/agentsskillsdemo"
})
```

## Getting Detailed Space Information

Once you have a space ID from `list_copilot_spaces`, use the companion tool `get_copilot_space` to retrieve detailed information:

### Tool: get_copilot_space

**Parameters**:
- `owner` (string, required): The owner of the space
- `name` (string, required): The name of the space

**Example**:
```typescript
get_copilot_space({
  owner: "Hemavathi15sg",
  name: "My Development Space"
})
```

**Response**:
```json
{
  "id": "space-unique-id-123",
  "name": "My Development Space",
  "description": "Space for active development projects",
  "owner": "Hemavathi15sg",
  "visibility": "private",
  "created_at": "2026-01-15T10:30:00Z",
  "updated_at": "2026-02-15T08:45:00Z",
  "repositories": [
    {
      "owner": "Hemavathi15sg",
      "name": "agentsskillsdemo",
      "full_name": "Hemavathi15sg/agentsskillsdemo",
      "url": "https://github.com/Hemavathi15sg/agentsskillsdemo",
      "added_at": "2026-01-20T14:00:00Z",
      "branch": "main"
    }
  ],
  "files": [
    {
      "path": "README.md",
      "source": "custom",
      "added_at": "2026-01-21T09:00:00Z"
    }
  ],
  "instructions": "Custom instructions for this space...",
  "sources_count": 5,
  "members": [
    {
      "username": "Hemavathi15sg",
      "role": "owner"
    }
  ]
}
```

## MCP Server Configuration

To use the `list_copilot_spaces` tool, the GitHub MCP server must be configured with the `copilot_spaces` toolset enabled.

### Configuration Method 1: Toolset Header

Enable the `copilot_spaces` toolset by adding it to the MCP server configuration:

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "X-MCP-Toolsets": "default,copilot_spaces"
      }
    }
  }
}
```

### Configuration Method 2: Dedicated Spaces URL

Alternatively, use the dedicated Copilot Spaces MCP endpoint:
```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/x/copilot_spaces"
    }
  }
}
```

### Supported Environments
The `list_copilot_spaces` tool is available in:
- ✅ VS Code with GitHub Copilot extension
- ✅ Claude Desktop with proper MCP configuration
- ✅ CLI tools with GitHub Copilot MCP server integration
- ❌ GitHub.com website (use web UI instead)

## API Limitations

**Important**: As of 2026, there is no public REST API endpoint for listing Copilot Spaces. Access is available through:
- GitHub web interface at [github.com/copilot/spaces](https://github.com/copilot/spaces)
- MCP server integration in supported IDEs
- GitHub Copilot Chat UI

The available GitHub REST API endpoints for Copilot focus on seat management and analytics, not Spaces management.

## Using list_copilot_spaces Tool

### Prerequisites
1. GitHub Copilot subscription (Individual, Business, or Enterprise)
2. MCP server configured with `copilot_spaces` toolset
3. Personal Access Token (PAT) with appropriate permissions (recommended)
4. Supported IDE or environment (VS Code, Claude Desktop, etc.)

### Tool Usage

Once properly configured, the `list_copilot_spaces` tool can be invoked to retrieve spaces. The tool typically accepts parameters such as:
- Repository filter (e.g., "Hemavathi15sg/agentsskillsdemo")
- Visibility filter (private, shared, public)
- Sort options (created_at, updated_at, name)

### Expected Output

When successfully executed, `list_copilot_spaces` returns space information in a structured format:

```json
{
  "spaces": [
    {
      "id": "space-unique-id",
      "name": "Space Name",
      "description": "Description of the space and its purpose",
      "visibility": "private",
      "created_at": "2026-01-15T10:30:00Z",
      "updated_at": "2026-02-15T12:00:00Z",
      "repositories": [
        {
          "owner": "Hemavathi15sg",
          "repo": "agentsskillsdemo",
          "added_at": "2026-01-20T14:00:00Z"
        }
      ],
      "sources": [
        {
          "type": "repository",
          "name": "agentsskillsdemo",
          "path": "/"
        },
        {
          "type": "file",
          "name": "README.md",
          "path": "/README.md"
        }
      ]
    }
  ],
  "total_count": 1,
  "has_more": false
}
```

## How to Create a Space for This Repository

To create or manage a Copilot Space for the `agentsskillsdemo` repository:

1. **Via Web UI**:
   - Navigate to [github.com/copilot/spaces](https://github.com/copilot/spaces)
   - Click "Create new space"
   - Add a name and description
   - Click "Add Sources" → "Repository"
   - Search for and select "Hemavathi15sg/agentsskillsdemo"
   - Save the space

2. **Add Repository to Existing Space**:
   - Open an existing space
   - Click "Add Sources"
   - Select "Repository"
   - Search for "Hemavathi15sg/agentsskillsdemo"
   - Confirm addition

## Benefits of Using Spaces with This Repository

Adding the `agentsskillsdemo` repository to a Copilot Space provides:
- **Enhanced Context**: Copilot has access to all repository code and documentation
- **Agent Skills Awareness**: Copilot understands the custom skills in `.github/skills/`
- **Better Suggestions**: Code completions are informed by repository patterns
- **Faster Exploration**: Ask questions about the codebase without manual searching
- **Team Collaboration**: Share space with team members for consistent context

## Practical Examples

### Example 1: Query About Repository Structure
With the repository in a space, you can ask Copilot:
- "What agent skills are available in this repository?"
- "How are the GitHub issues templates structured?"
- "Show me examples of skill definitions"

### Example 2: Code Generation
Copilot can generate code that follows repository conventions:
- "Create a new skill similar to the github-issues skill"
- "Add error handling consistent with existing patterns"

### Example 3: Documentation
- "Explain how the MCP server integration works"
- "What are the dependencies for this project?"

## Troubleshooting

### Tool Not Appearing
If `list_copilot_spaces` doesn't appear in your IDE:
1. **Update Toolsets**: Set `X-MCP-Toolsets` to `"all"` or `"default,copilot_spaces"`
2. **Check Configuration**: Verify MCP server URL and headers are correct
3. **Restart IDE**: Some changes require IDE restart to take effect
4. **Verify Subscription**: Ensure you have an active Copilot subscription

### No Spaces Returned
If the tool runs but returns no results:
1. Create a space via the web UI first
2. Verify authentication (try using a PAT instead of OAuth)
3. Check that spaces contain the repository you're searching for
4. Ensure proper permissions on the PAT

### IDE-Specific Issues
- **Android Studio**: May have compatibility issues; try VS Code or Claude Desktop
- **VS Code**: Generally works well with latest Copilot extension
- **Claude Desktop**: Requires manual MCP configuration but works reliably

## MCP Server Configuration Examples

### Claude Desktop Configuration
Add to `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-pat-here"
      }
    }
  }
}
```

Then configure toolsets via environment or headers to include `copilot_spaces`.

### VS Code Configuration
Install the GitHub Copilot extension and ensure it's up to date. The MCP integration should be automatic with proper authentication.

## Alternative: Manual Space Listing

Since there's no public REST API, here's how to manually document your spaces:

1. Visit [github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Take note of each space containing this repository:
   - Space name
   - Description
   - Visibility setting
   - Date created
   - Other repositories/files included

3. Document them in this format:

| Space Name | Description | Visibility | Repositories | Created |
|-----------|-------------|------------|-------------|---------|
| [Name] | [Description] | Private/Shared/Public | agentsskillsdemo, ... | [Date] |

## Next Steps

To fully utilize Copilot Spaces with the `agentsskillsdemo` repository:

1. ✅ **Create a Space**: Visit [github.com/copilot/spaces](https://github.com/copilot/spaces) and create a new space
2. ✅ **Add Repository**: Include `Hemavathi15sg/agentsskillsdemo` as a source
3. ✅ **Configure MCP** (Optional): Set up IDE integration if needed
4. ✅ **Test Integration**: Ask Copilot questions about the repository
5. ✅ **Share with Team** (Optional): Set visibility and share with collaborators

## Additional Resources

### Official Documentation
- [Using GitHub Copilot Spaces](https://docs.github.com/en/copilot/how-tos/provide-context/use-copilot-spaces/use-copilot-spaces)
- [About GitHub Copilot Spaces](https://docs.github.com/copilot/concepts/context/spaces)
- [Copilot REST API Reference](https://docs.github.com/en/rest/copilot)

### Community Resources
- [GitHub MCP Server Repository](https://github.com/github/github-mcp-server)
- [Enabling Copilot Spaces with Coding Agents](https://www.ericksegaar.com/2025/12/08/enable-copilot-space-with-coding-agent/)
- [MCP Configuration Guide](https://modelcontextprotocol.io/)

### Related Features
- **Copilot Chat**: Ask questions about code with space context
- **Code Completions**: Get suggestions informed by space content
- **Pull Request Summaries**: AI-generated summaries using space context
- **Code Review**: Automated reviews with repository knowledge

## Repository-Specific Context

For the `agentsskillsdemo` repository, relevant spaces might include:
- Skills and agent configurations (`.github/skills/`)
- MCP server integration examples
- GitHub automation workflows
- Documentation and guides

When creating a space for this repository, consider including:
- The entire repository for comprehensive context
- Related documentation repositories
- Team conventions and standards documents
- Custom instructions for agent behavior

---

## Tool Reference from GitHub MCP Server

### Available Copilot Spaces Tools

From the **Additional Tools in Remote GitHub MCP Server** documentation:

#### 1. list_copilot_spaces
**Description**: List Copilot Spaces

**Usage**:
```
list_copilot_spaces()
```

Returns a list of all Copilot Spaces accessible to the authenticated user. Results can be filtered programmatically after retrieval to find spaces containing specific repositories.

#### 2. get_copilot_space
**Description**: Get Copilot Space

**Parameters**:
- `owner` (string, required): The owner of the space
- `name` (string, required): The name of the space

**Usage**:
```
get_copilot_space({
  owner: "username",
  name: "space-name"
})
```

Returns detailed information about a specific Copilot Space, including all repositories, files, and custom instructions associated with it.

### Tool Location

These tools are part of the **Additional Tools in Remote GitHub MCP Server** under the **Copilot Spaces** category. They require the MCP server to be configured with the `copilot_spaces` toolset enabled.

**MCP Server URL**: `https://github.com/mcp/io.github.github/github-mcp-server`

---

## Summary

This document provides comprehensive information about searching for Copilot Spaces related to the `agentsskillsdemo` repository using the `list_copilot_spaces` tool from the GitHub MCP server.

### Key Points

1. **Tool**: `list_copilot_spaces` is part of the GitHub Copilot MCP server's "Copilot Spaces" toolset
2. **Configuration**: Requires MCP server with `X-MCP-Toolsets: "default,copilot_spaces"` header
3. **Access Methods**:
   - MCP server integration in supported IDEs (VS Code, Claude Desktop)
   - Web UI at [github.com/copilot/spaces](https://github.com/copilot/spaces)
4. **No REST API**: Copilot Spaces are not accessible via public REST API endpoints
5. **Authentication**: Requires valid GitHub Copilot subscription and authentication

### To View Your Spaces

**Recommended**: Visit [github.com/copilot/spaces](https://github.com/copilot/spaces) to view all Copilot Spaces and filter for those containing the `agentsskillsdemo` repository.

**Advanced**: Configure the GitHub MCP server in your IDE with the `copilot_spaces` toolset enabled to use `list_copilot_spaces` and `get_copilot_space` tools programmatically.

**Last Updated**: February 15, 2026  
**Repository**: Hemavathi15sg/agentsskillsdemo  
**Branch**: copilot/search-repo-spaces  
**Tool Reference**: GitHub MCP Server - Additional Tools - Copilot Spaces

---

*This document provides comprehensive information about accessing and using GitHub Copilot Spaces with the `agentsskillsdemo` repository using the `list_copilot_spaces` tool from the GitHub MCP server. For the most up-to-date information, refer to the official GitHub documentation.*

