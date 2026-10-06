# Preparation validation — 6 October 2026

## Passed

- ClawHub CLI **0.23.3** recognizes the package as `bundle-plugin`, version
  `0.1.0`, with seven intended package files. Publish preview performed without
  uploading or authenticating to ClawHub.
- ClawHub's Plugin Inspector against released OpenClaw **2026.9.8** reports
  **pass**, zero breakages, warnings, deprecations, or issues. This is a static
  metadata/package check, not an authenticated MCP test.
- OpenClaw **2026.9.8**'s actual bundle loader recognizes the Claude-format marker,
  loads the skill and MCP capabilities, and classifies `graphify` as a supported
  HTTP MCP server with no unsupported servers or loader diagnostics.
- The enabled-bundle configuration loader retains the endpoint, explicit
  Streamable HTTP transport, and OAuth authentication setting.
- Hermes's actual `_parse_manifest` and `_build_server_config` accept the candidate
  and produce `{"url":"https://api.graphify.com/mcp","auth":"oauth"}`. Source
  reference: `3b4a8911741a09440e0a10f25996eb41cf0f061f`.
- Public Graphify discovery returns the correct `/mcp` resource and advertises
  native OAuth registration, PKCE S256, and refresh-token support.
- The npm package file preview includes both hidden configuration files, the
  skill, README, metadata, and the existing white-on-green Graphify PNG icon
  (400 × 400; below ClawHub's 512 KiB limit). No dependencies or install scripts.

## Not yet verified

- Installation and OAuth in a complete running OpenClaw or Hermes client.
- Authenticated repository discovery, query results, token refresh, or revocation
  in either client. Existing tests from other clients do not establish this.
- ClawHub owner/namespace availability, distribution-license acceptance, security
  review, or public listing.
- Acceptance of the Hermes catalog contribution or the release that will ship it.

Local checks used isolated temporary dependencies and did not change the user's
OpenClaw/Hermes configuration or log into Graphify. No production change was made.
