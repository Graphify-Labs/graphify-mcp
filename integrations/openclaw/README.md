# Graphify MCP for OpenClaw

Investigate your indexed repositories with Graphify's hosted code knowledge
graph: find implementations, follow callers and dependencies, assess static
change impact, identify linked tests, and retain repository knowledge.

This is a declarative MCP bundle with a usage skill. It contains no server
implementation, runtime JavaScript, installer hooks, or bundled credentials.
ClawHub publication is pending; the package name is provisional until Graphify's
publisher handle is confirmed.

## Requirements

- A Graphify account with access to a workspace and a populated, indexed repository.
- OpenClaw with remote MCP, OAuth, and Claude-format bundle support. The preparation
  target is OpenClaw 2026.9.8; older releases may differ.
- An enabled model and tool access in OpenClaw. Graphify service access and model
  access are separate.

## Install from this repository

From the repository root:

```sh
openclaw plugins install ./integrations/openclaw
openclaw plugins inspect graphify-mcp
```

Complete Graphify OAuth using OpenClaw's native MCP configuration. This explicit
definition also makes the server available to OpenClaw's MCP management commands:

```sh
openclaw mcp set graphify '{"url":"https://api.graphify.com/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login graphify
openclaw mcp doctor graphify --probe
```

If a server named `graphify` already exists, inspect it first with
`openclaw mcp show graphify` and retain any intentional account-specific settings.
Sign in and authorize Graphify in the browser. Start a new agent session after
configuration; follow any restart instruction printed by your OpenClaw version.
Do not put passwords or tokens in configuration files.

The equivalent configuration, merged into your existing OpenClaw configuration:

```json
{
  "mcp": {
    "servers": {
      "graphify": {
        "url": "https://api.graphify.com/mcp",
        "transport": "streamable-http",
        "auth": "oauth"
      }
    }
  }
}
```

Use one connection named `graphify`; do not create a second differently named
connection to the same service unless you intend to expose duplicate tools.

## Try it

1. “List my Graphify workspaces and repositories.”
2. “In my selected repository, find the function that handles authentication.”
3. “Show its callers and the linked tests, with file paths as evidence.”

Select a workspace and repository from the returned results. A connection test
should include an actual repository lookup, not only tool discovery.

## Behavior and limitations

Requests go to `https://api.graphify.com/mcp`. OAuth credentials are managed by
OpenClaw. Results are restricted by the signed-in account and depend on indexing.
Some queries can save query trails; memory tools can persist or modify knowledge,
and changing the workspace affects subsequent queries. Review live annotations
and client tool approvals. Static graph results do not execute code or tests.

If tools are hidden, check plugin enablement and your tool policy, including any
`bundle-mcp` denial. If sign-in fails, run `openclaw mcp doctor graphify --probe`
and retry login. Do not share tokens or private source in public issue reports.

## Support

[Graphify MCP documentation](https://graphify.com/mcp) ·
[Privacy policy](https://graphify.com/privacy) ·
[Terms of service](https://graphify.com/terms) · support@graphify.com

The package currently reserves its rights (`UNLICENSED`) pending Graphify's
distribution-license decision. It is not represented as OpenClaw-approved.
