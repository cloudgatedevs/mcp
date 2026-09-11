# Cloudgate Builder

Connects Codex or Claude to `https://api.cloudgate.dev/mcp/workflow` using native
streamable HTTP and OAuth, and includes the `cloudgate-build` skill.

## Requirements and setup

Use a desktop MCP client supporting native HTTP and OAuth, and a Cloudgate account.
Install or update the plugin from its marketplace, start a new conversation, and ask
"List my Cloudgate projects." Complete the client's OAuth sign-in when prompted.
No Node.js installation, static callback port, or packaged bearer token is required.

## Updating from the bridge version

Refresh the marketplace and reinstall/update the plugin. Start a new conversation so
its MCP configuration is reloaded. The installed `.mcp.json` should contain `type: http`
and the Cloudgate URL, with no `npx` command. Authorize through the client's connection
settings if requested; old `mcp-remote` credentials are not the native client's login.

## Troubleshooting

Both discovery documents must advertise the exact same issuer, including its trailing
slash: `https://api.cloudgate.dev/`. If tools do not load, inspect the client's MCP
startup error. Do not put account tokens in this repository or disable issuer checks.
