# Graphify MCP for Hermes Agent

Connect Nous Research's Hermes Agent to Graphify's hosted code knowledge graph
using Hermes's native remote MCP and browser OAuth support.

## Connect now

1. Sign in to [Graphify](https://app.graphify.com), select a workspace, and wait
   for a populated repository to finish indexing.
2. Merge [config.example.yaml](config.example.yaml) into your active Hermes
   profile's `config.yaml` (normally `~/.hermes/config.yaml`). Preserve existing
   configuration and other MCP servers.
3. Run `hermes mcp login graphify` and complete browser authorization.
4. Run `hermes mcp configure graphify` to choose the exposed tools, then restart
   the Hermes session.
5. Ask it to list Graphify workspaces and repositories, select a repository, and
   look up a known function. Confirm returned file paths against the index.

Use a current Hermes release with native MCP OAuth support. This configuration
does not require an API key, a Python wrapper, or a local Graphify server.

## Official catalog contribution

The candidate is [optional-mcps/graphify/manifest.yaml](optional-mcps/graphify/manifest.yaml).
Copy that file to the same path in `NousResearch/hermes-agent` and submit a PR.
Nous Research reviews catalog additions; this candidate is not an approval or
an existing listing. The catalog entry can be installed by name only after
acceptance and availability in the user's Hermes distribution:

```sh
hermes mcp catalog
hermes mcp install graphify
```

The existing Graphify-named optional skill proposal concerned the local
`graphifyy` package, not this hosted OAuth MCP endpoint. This contribution adds
only an MCP catalog manifest and does not add a memory-provider plugin or
third-party service implementation to Hermes core.

## Data and behavior

Tool requests go to `https://api.graphify.com/mcp`. Hermes manages OAuth
credentials. Account permissions and the active workspace limit access.
Graphify's tools include writes: query trails can be recorded, memory can be
persisted or modified, and workspace selection changes subsequent query scope.
Review tool descriptions and choose the tools you want enabled. Indexed graph
results can be incomplete or stale; linked tests are not executed by this
connector. Treat semantic symbol matches as suggestions, not exact matches.

[Graphify documentation](https://graphify.com/mcp) ·
[Hermes MCP documentation](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/) ·
[Privacy](https://graphify.com/privacy) · [Terms](https://graphify.com/terms) ·
support@graphify.com
