# Connect Divblox MCP

**Manual connection package 0.1.0.** No marketplace listing is required to use a supported manual connection.

## Before you begin

You need a Divblox account with access to a project and a client that supports remote MCP over Streamable HTTP with OAuth. Your client or workspace administrator may need to enable custom MCP connections.

- Name: **Divblox MCP**
- Server URL: `https://mcp.divblox.app/mcp`
- Authentication: OAuth, using the client profile below; no client secret.

Divblox hosts the server. You do not install or run a local Divblox server. Browser sign-in and project consent use `https://mcp.divblox.app`. Existing connections created before the production-domain migration must be connected again. Enter your Divblox password only in that sign-in form, never in chat or configuration files.

## Choose your client

### ChatGPT

1. Enable Developer mode in Settings → Security and login, if available to your account/workspace.
2. In Plugins, create a custom connection named **Divblox MCP** using the server URL above.
3. Choose OAuth. Where advanced settings request a client ID, use `chatgpt-production`; leave the client secret empty.
4. Complete Divblox sign-in and project consent, then select Divblox MCP in a chat.

The registered callback is `https://chatgpt.com/connector_platform_oauth_redirect`. If ChatGPT displays a different callback, contact support before retrying; it must match the registered configuration. Workspace policy or plan access may prevent custom connections.

[Official connection guide](https://developers.openai.com/plugins/deploy/connect-chatgpt)

### Claude

In Claude’s connector settings, add a custom remote connector named **Divblox MCP** with the server URL. Choose “Use your own OAuth client” (or advanced OAuth settings), use client ID `claude-desktop-production`, and leave the secret empty. Connect and complete the browser consent. The registered callback is `https://claude.ai/api/mcp/auth_callback`.

Availability depends on your Claude account and workspace configuration. The packaged route is a custom remote connector, not a claim of directory approval.

### Claude Code

Merge [the Claude Code profile](clients/claude-code.json) into your workspace `.mcp.json`, preserving existing entries. In Claude Code, run `/mcp`, choose `divblox-mcp`, and authenticate. The profile fixes the callback port to 8841 and uses public client `claude-code-production`.

Use a version that sends `http://localhost:8841/callback`. The vendor documents a callback-host regression in version 2.1.229, corrected in 2.1.231. Do not add a secret to work around a redirect error.

[Official MCP guide](https://code.claude.com/docs/en/mcp)

### Codex

Merge [the Codex profile](clients/codex.toml) into your Codex `config.toml`, preserving other settings and servers. Run:

```sh
codex mcp login divblox-mcp
```

Complete the browser consent. The profile uses `codex-production` and callback port 8842. Reload the client if tools do not appear.

[Official MCP guide](https://learn.chatgpt.com/docs/extend/mcp?surface=cli)

### Cursor desktop

Merge [the Cursor profile](clients/cursor.json) into `.cursor/mcp.json` in your workspace. Open that workspace in Cursor. In Customize → MCPs (or MCP settings), open **Divblox MCP**, enable its workspace source, and choose Authenticate. Complete the Divblox browser sign-in and consent. The profile uses `cursor-production` and the desktop callback `http://localhost:8787/callback`.

This covers Cursor’s desktop IDE and desktop Agents interface using “This Mac”. Cursor web/cloud Agents use a different callback and are not covered by this registration.

[Official MCP guide](https://cursor.com/docs/mcp)

### VS Code

Merge [the VS Code profile](clients/vscode.json) into `.vscode/mcp.json`, preserving other servers. Start **Divblox MCP** using its configuration controls or MCP server commands, then complete OAuth. Use a VS Code version supporting `oauth.clientId`.

The existing registration includes `http://127.0.0.1:33418/` and `https://vscode.dev/redirect`. If your client chooses another callback, contact support; do not change ports or disable authentication checks blindly. The production flow was verified with VS Code 1.138.0.

[Official MCP guide](https://code.visualstudio.com/docs/agent-customization/mcp-servers)

### Gemini CLI — limited validation

From your extracted package, link the included extension:

```sh
gemini extensions link ./gemini/divblox-mcp
```

Start Gemini CLI, run `/mcp auth`, and choose **Divblox MCP**. Its profile uses `gemini-production` and callback port 8873. Gemini’s own model sign-in is separate from Divblox sign-in. Gemini write reliability is not certified; consumer Gemini is not covered by this package.

[Official MCP guide](https://geminicli.com/docs/tools/mcp-server/)

### Other MCP clients

Choose a remote Streamable HTTP server, enter the server URL, and enable OAuth. Divblox currently uses predefined public OAuth clients, not open dynamic registration. Contact **support@divblox.com** with your client name/version and exact callback URI if it has no supported profile. Do not reuse another client’s ID or assume a generic URL-only connection will work.

## Verified scope

Production sign-in and project reads were checked on 1 October 2026 in ChatGPT web, Claude web, Claude Desktop 2.16120.0, Claude Code 2.1.286, Codex, Cursor desktop 3.21.16, VS Code 1.138.0 and Gemini CLI 0.60.0. Read checks use consent-limited projects; they do not certify every tool, model or operating system. Gemini write reliability remains uncertified.

## Choose projects and try a first request

Sign in, select your projects, and review each project’s permissions before continuing. Selecting a project preselects available permissions; deselect any you do not want to grant. Divblox’s own access rules still apply.

Start with:

> Which Divblox projects can you access?

Then try:

> Get started in my new project on Divblox.

Name the project you want to use. Your assistant can help refine a requirement, prepare and estimate tickets, plan tasks, publish work to delivery, and track progress. Confirm the intended project and changes as you work.

If a project is missing, ask the assistant to discover projects and add it to the connection. Use the returned sign-in link to review access; you do not need to disconnect existing projects.

## Troubleshooting and removal

- **Forbidden before sign-in:** contact support with your client/version and error time. Network reachability must be enabled for your client and browser; reconnecting does not fix an ingress block.
- **Invalid client or callback:** use the matching profile. Send support the client/version and callback URI, without authorization codes or state parameters.
- **Missing project:** refresh discovery and review project consent. Confirm the same Divblox account has an active membership.
- **Missing tools or stale connection status:** refresh the client’s tool catalog or reload it. In Claude Desktop, View → Reload refreshed a connection completed in the browser. Avoid unnecessary reconnection.
- **Unknown write outcome:** inspect whether the record changed before retrying.

To stop access, disconnect through the client or Divblox connection portal, then remove only the Divblox configuration entry. Removing local files alone may leave a remote grant active. Never send support passwords, tokens or full sign-in URLs.
