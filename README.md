# Graphify MCP

Connect your AI assistant to Graphify's hosted code knowledge graph. Search indexed repositories, follow code relationships, assess static change impact, find linked tests, and retain repository knowledge through the Model Context Protocol (MCP).

This repository contains public connection documentation for the **Graphify hosted MCP service**. The hosted server implementation is maintained privately. This repository does not contain or distribute that implementation.

- **Endpoint:** `https://api.graphify.com/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** Graphify OAuth through your browser
- **Official MCP Registry identity:** `io.github.Graphify-Labs/graphify`
- **Documentation:** [graphify.com/mcp](https://graphify.com/mcp)

## What you can do

| Workflow | Example request |
| --- | --- |
| Discover repositories | “List my Graphify workspaces and repositories.” |
| Find implementations | “Find the implementation of this function in my selected repository.” |
| Trace relationships | “Show the callers and dependencies of this symbol.” |
| Assess changes | “What could be affected by changing this function, and which tests are linked to it?” |
| Retain context | “Remember this repository decision, then recall it when we return to this code.” |

Results depend on your account permissions, selected workspace, indexed repositories, and available index data. Static impact analysis and linked-test results help guide investigation; they do not execute tests or prove runtime correctness. Memory operations can persist or modify repository knowledge; review write requests before approving them in your client.

## Before connecting

1. Sign in or create an account at [app.graphify.com](https://app.graphify.com).
2. Select your workspace and connect a repository you are authorized to use.
3. Wait for repository indexing to complete.
4. Use an MCP-capable client with support for Streamable HTTP and OAuth. Client/model access and Graphify service plans apply separately.

## Connect in VS Code / GitHub Copilot

You can connect directly even if Graphify is not yet visible in the MCP gallery.

1. Open the Command Palette and run **MCP: Add Server**.
2. Choose **HTTP**, enter `https://api.graphify.com/mcp`, and name the server `graphify`.
3. Choose the desired configuration scope and start the server.
4. Complete the Graphify browser sign-in and authorize the connection.
5. Confirm Graphify tools are available. Open an agent chat with MCP tools enabled and ask it to list your Graphify repositories.

Alternatively, use this VS Code configuration in `.vscode/mcp.json`:

```json
{
  "servers": {
    "graphify": {
      "type": "http",
      "url": "https://api.graphify.com/mcp"
    }
  }
}
```

No password or access token belongs in this file. Let the client complete OAuth and manage its credentials. See [VS Code's MCP instructions](https://code.visualstudio.com/docs/agent-customization/mcp-servers) for current UI and configuration options.

## Connect in other MCP clients

Client-specific packages and setup instructions:

- [OpenClaw / ClawHub](https://clawhub.ai/graphify-labs/plugins/graphify-mcp) — published MCP bundle and usage skill; see the [local bundle notes](integrations/openclaw/README.md).
- [Nous Research Hermes Agent](integrations/hermes/README.md) — native OAuth configuration and an official-catalog contribution candidate.

These are connection packages for the hosted service. Their inclusion here does
not mean every platform has approved or shipped a catalog listing.

For clients with a remote MCP setup form, enter `https://api.graphify.com/mcp` as the server URL and complete browser OAuth. Configuration formats differ by client.

Clients that accept an `mcpServers` configuration may use:

```json
{
  "mcpServers": {
    "graphify": {
      "url": "https://api.graphify.com/mcp"
    }
  }
}
```

For a STDIO-only client or one without compatible remote OAuth support, a bridge configuration is available:

```json
{
  "mcpServers": {
    "graphify": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote@0.14.3",
        "https://api.graphify.com/mcp",
        "--transport",
        "http-only"
      ]
    }
  }
}
```

The bridge requires Node.js and npm on the client's PATH. Enabling it downloads and runs the pinned third-party `mcp-remote` package, which handles browser OAuth and stores credentials locally. Prefer your client's native remote connection when it supports Graphify's OAuth flow.

## Verify the connection

1. Confirm the MCP client reports the server as connected and exposes Graphify tools.
2. Ask it to list your workspaces and repositories.
3. Select a populated, indexed repository and request a known function.
4. Ask for that function's callers or linked tests and check the returned evidence.

Tool discovery alone does not verify repository access or the accuracy of a particular query result.

## Troubleshooting

- **Server missing from a gallery:** add the endpoint directly. Official MCP Registry publication and downstream gallery inclusion are separate steps.
- **Sign-in fails:** check that your client supports Graphify OAuth, retry its sign-in flow, or use the bridge configuration where supported.
- **No repositories or empty results:** confirm workspace access and indexing completion in Graphify.
- **Node.js or npx not found:** ensure Node.js and npm are available on the MCP client's PATH when using the bridge.
- **Tools connect but chat cannot run them:** check model access and tool enablement in your AI client.

## Support and policies

- [MCP documentation](https://graphify.com/mcp)
- [Contact support](https://graphify.com/contact) · support@graphify.com
- [Privacy policy](https://graphify.com/privacy)
- [Terms of service](https://graphify.com/terms)

Do not post credentials, tokens, private repository contents, or sensitive logs in public issues. Use the support channel for account-specific assistance.
