# Add Graphify hosted MCP to the MCP catalog

Graphify provides a hosted code knowledge graph for indexed repositories. This
adds one `optional-mcps/graphify/manifest.yaml` entry so users can discover and
configure it through Hermes's MCP catalog.

The server uses Streamable HTTP at `https://api.graphify.com/mcp` and native
OAuth with dynamic client registration. Hermes handles authentication; there is
no wrapper, bootstrap command, new dependency, core change, or server source in
this contribution.

A Graphify account, workspace access, and an indexed repository are required.
The live tool catalog includes query-trail and memory writes and workspace
selection. The manifest lets users choose their tools and discloses those side
effects in the post-install instructions.

Documentation: https://graphify.com/mcp
Public connection repository: https://github.com/Graphify-Labs/graphify-mcp
Official MCP Registry name: `io.github.Graphify-Labs/graphify`

## Validation

- Parsed using Hermes's `_parse_manifest` and built the expected native OAuth
  configuration using `_build_server_config` at commit
  `3b4a8911741a09440e0a10f25996eb41cf0f061f`.
- Public discovery metadata identifies resource `https://api.graphify.com/mcp`
  and advertises authorization code, PKCE S256, public-client registration,
  refresh tokens, and the `graphify:query` scope.
- **Before submitting this PR:** replace this line with actual Hermes version,
  browser-login, repository-list, and known-function lookup results. Those
  authenticated checks were not performed during package preparation.

The prior Graphify-named optional skill proposal (#47576, closed) targeted
`graphifyy` locally; this entry targets Graphify's hosted OAuth MCP service.
