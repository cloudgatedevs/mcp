# Cloudgate MCP

Build and manage **Cloudgate** workflow-APIs — controllers, actions, workflow graphs,
and databases — from any MCP-compatible AI client, including **Claude**, **OpenAI Codex**,
and others.

Cloudgate runs a hosted, remote MCP server at `api.cloudgate.dev`. It speaks the standard
Model Context Protocol over streamable HTTP and authenticates each user with OAuth, so any
client that supports remote MCP servers can connect. This repo also packages the server as a
ready-to-install **Claude plugin** for convenience.

- **MCP server URL:** `https://api.cloudgate.dev/mcp/workflow`
- **Registry:** listed in the [official MCP Registry](https://registry.modelcontextprotocol.io) as `dev.cloudgate/workflow`

## Connect

### Claude (Cowork / Desktop / Web)

1. Open **Customize → Plugins**.
2. In **Personal plugins**, click **+ → Add marketplace**.
3. Choose **Add from a repository** and enter this repo: `https://github.com/cloudgatedevs/mcp`
4. Install **cloudgate-builder** from the marketplace.
5. Start a chat and ask to list your Cloudgate controllers — you'll be sent to
   `hub.cloudgate.dev` to sign in, then the tools become available.

### Claude Code

```bash
claude plugin marketplace add cloudgatedevs/mcp
claude plugin install cloudgate-builder@cloudgate-app-templates
```

### OpenAI Codex

Add the remote server to `~/.codex/config.toml` (OAuth remote servers use Codex's RMCP client):

```toml
experimental_use_rmcp_client = true

[mcp_servers.cloudgate]
url = "https://api.cloudgate.dev/mcp/workflow"
```

Then authenticate:

```bash
codex mcp login cloudgate
```

### Other MCP clients

Any client that supports **remote (streamable HTTP) MCP servers** can connect directly to
`https://api.cloudgate.dev/mcp/workflow` and complete the OAuth sign-in.

For clients that only support **stdio** servers, bridge to the remote endpoint with
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "cloudgate": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.cloudgate.dev/mcp/workflow"]
    }
  }
}
```

## Authentication

The Cloudgate MCP server uses OAuth 2.1 (authorization code + PKCE). Each user signs in as
themselves; every MCP call is scoped to that user's tenant and account. No tokens or secrets
are stored in this repository.

## What's in this repo

The hosted server works on its own with any MCP client. This repo additionally ships the
**cloudgate-builder** Claude plugin, which bundles the connection config plus a
`cloudgate-build` skill — a playbook that guides the model through creating controllers,
actions, workflow graphs, and databases correctly.

```
.claude-plugin/marketplace.json     # Claude marketplace manifest (lists the plugins)
plugins/cloudgate-builder/          # the Claude plugin
  .claude-plugin/plugin.json        # plugin manifest
  .mcp.json                         # MCP server connection (native remote HTTP)
  README.md                         # plugin readme
  skills/cloudgate-build/SKILL.md   # the cloudgate-build skill
server.json                         # MCP Registry metadata (dev.cloudgate/workflow)
```

## License

© Cloudgate Dev LLC.
