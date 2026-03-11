# mcp-redhat-support

An [MCP](https://modelcontextprotocol.io/) server for the Red Hat Support Case Management API. Lets AI assistants list, read, comment on, and manage Red Hat support cases.

## Tools

| Tool | Description |
|------|-------------|
| `listCases` | List support cases with filtering by status, severity, and search string |
| `getCase` | Get full details of a specific case |
| `getCaseComments` | Get all comments on a case |
| `getCaseComment` | Get a single comment by comment ID |
| `addCaseComment` | Add a comment to a case |
| `getExternalTrackerUpdates` | List external tracker updates (linked Jira/Bugzilla) on a case |
| `addNotifiedUsers` | Add notified users to a case |
| `removeNotifiedUser` | Remove a notified user from a case |
| `getCaseAttachments` | List attachments on a case |
| `downloadAttachment` | Download a case attachment to a local file |
| `uploadAttachment` | Upload a local file as an attachment to a case |
| `deleteAttachment` | Delete an attachment from a case |
| `createCase` | Open a new support case |
| `updateCase` | Update case fields (severity, status, consultant engaged, contact, etc.) |
| `closeCase` | Close a case with an optional resolution comment |
| `getBusinessHours` | Get Red Hat support business hours for a given timezone |

## Prerequisites

- Node.js 18+
- A Red Hat offline API token ([generate one here](https://access.redhat.com/management/api))

## Configuration

Set your Red Hat offline API token in your shell profile:

```bash
export REDHAT_TOKEN="your-offline-token-here"
```

In enterprise environments, provide `REDHAT_TOKEN` through your secrets management solution (e.g. HashiCorp Vault, Kubernetes secrets, or your CI platform's secret variables) rather than shell profiles.

### Gemini CLI

Add to `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "redhat-support": {
      "command": "npx",
      "args": ["-y", "mcp-redhat-support"],
      "env": {
        "REDHAT_TOKEN": "$REDHAT_TOKEN"
      }
    }
  }
}
```

### watsonx Orchestrate

```bash
# Add a connection for the Red Hat API token
orchestrate connections add --app-id "redhat-support"
orchestrate connections configure --app-id redhat-support --env draft --kind key_value --type team --url "https://access.redhat.com"
orchestrate connections set-credentials --app-id "redhat-support" --env draft -e REDHAT_TOKEN=your-offline-token-here

# Import the MCP toolkit
orchestrate toolkits import --kind mcp \
  --name redhat-support \
  --description "Red Hat Support Case Management" \
  --command "npx -y mcp-redhat-support" \
  --tools "*" \
  --app-id redhat-support
```

### Claude Code

Add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "redhat-support": {
      "command": "npx",
      "args": ["-y", "mcp-redhat-support"],
      "env": {
        "REDHAT_TOKEN": "${REDHAT_TOKEN}"
      }
    }
  }
}
```

### VS Code / Cursor

Add to `.vscode/mcp.json` in your workspace:

```json
{
  "mcpServers": {
    "redhat-support": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "mcp-redhat-support"],
      "env": {
        "REDHAT_TOKEN": "${REDHAT_TOKEN}"
      }
    }
  }
}
```

## Authentication

The server exchanges your Red Hat offline API token for a short-lived bearer token via Red Hat SSO. Tokens are cached and refreshed automatically.

## Related MCP Servers

- [mcp-redhat-account](https://github.com/shonstephens/mcp-redhat-account) - Account management
- [mcp-redhat-subscription](https://github.com/shonstephens/mcp-redhat-subscription) - Subscription management
- [mcp-redhat-knowledge](https://github.com/shonstephens/mcp-redhat-knowledge) - Knowledge Base search
- [mcp-redhat-manpage](https://github.com/shonstephens/mcp-redhat-manpage) - RHEL man pages

## License

MIT
