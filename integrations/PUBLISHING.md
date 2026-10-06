# OpenClaw and Hermes publishing handoff

Prepared on 6 October 2026 and updated after the first publication pass. These
packages connect to the existing hosted Graphify MCP service. No backend, OAuth,
or main Graphify OSS change is required by these manifests.

## Current status

- OpenClaw package published on ClawHub as
  `@graphify-labs/graphify-mcp`.
- Latest OpenClaw package version: `0.1.1`.
- Latest ClawHub release id: `rd7eyg2841f218ze3rcprm0by98fswzx`.
- Source commit for `0.1.1`:
  `9a091f1687d33d9b8c688fffcf3486cc1e362398`.
- ClawHub package inspection reports `scanStatus: clean`, `isOfficial: false`,
  and verification tier `source-linked`.
- Hermes catalog PR:
  <https://github.com/NousResearch/hermes-agent/pull/133973>.

## Remaining external decisions

1. Decide whether to request ClawHub official or verified treatment from the
   OpenClaw maintainers. The package currently remains a community package.
2. Wait for Nous Research review and acceptance of the Hermes catalog PR.
3. Record a post-publish OpenClaw install smoke test from the public package if
   the team wants evidence attached to a badge or trust review request. Package
   validation and public OAuth metadata checks are not proof of successful
   sign-in.

## ClawHub official or verified status

Do not add `official`, `verified`, `trusted`, `approved`, or similar topics to
the package. ClawHub rejects those as reserved metadata. The current public
trust state is source-linked, community, and cleanly scanned.

For an official or verified badge, prepare a maintainer-facing request with:

- ClawHub listing URL and package identity.
- Graphify Labs owner identity and proof that `graphify-labs` is controlled by
  the company.
- Source repo, source commit, and `integrations/openclaw` source path.
- Clean validation and scan results.
- OAuth, credential-handling, network-egress, and side-effect summary.
- Authenticated smoke-test evidence from OpenClaw and Hermes.
- Support and incident contact.

OpenClaw docs and ClawHub repository notes indicate that official or badge state
is admin or maintainer controlled, not something a publisher can self-assert.

## Smoke test before either submission

Use a test Graphify account with a populated, indexed repository. Do not put its
credentials in this repo, a marketplace description, a PR, or an issue.

For each client, follow its README, then record:

- Client version, successful browser authorization, and Graphify tool discovery.
- `list_workspaces` and `list_repositories` return the expected authorized data.
- A known function lookup returns the expected source path and definition.
- A callers or linked-tests lookup returns explainable results from that index.
- A new client session reconnects successfully; separately exercise token refresh
  when possible before asserting refresh compatibility.
- Live tool descriptions accurately disclose query-trail, memory, and workspace
  side effects. Do not run a memory write solely to complete this read-focused test.

Do not mark a check passed if the model cannot call the tools, the repository has
not finished indexing, or only `tools/list` succeeded.

## OpenClaw: publish or update the bundle on ClawHub

Use the prepared `integrations/openclaw` directory, not the repository root.
It contains a Graphify icon, MCP configuration, a usage skill, and both the
ClawHub catalog metadata and a compatible Claude-format bundle marker.
Do not add `openclaw.extensions` or a JavaScript entrypoint: this is a bundle.

For future updates, merge the integration files into the public Graphify MCP repo
and check out the merged commit with a clean working tree. With Node.js 22 or
newer for ClawHub:

```sh
npx --yes clawhub@0.23.3 login
npx --yes clawhub@0.23.3 package validate ./integrations/openclaw \
  --openclaw-version 2026.9.8

release_commit=$(git rev-parse HEAD)
npx --yes clawhub@0.23.3 package publish ./integrations/openclaw \
  --owner graphify-labs \
  --source-repo Graphify-Labs/graphify-mcp \
  --source-commit "$release_commit" \
  --source-path integrations/openclaw \
  --host-targets openclaw \
  --topics mcp,code-search,knowledge-graph \
  --changelog "Describe the package change here." \
  --dry-run
```

Review the preview. Then repeat the publish command with `--wait` in place of
`--dry-run`. Publishing is an external release; security checks can hold or reject
it. A successful upload is not the same as a public listing.

Install using the exact published package identity:

```sh
openclaw plugins install clawhub:@graphify-labs/graphify-mcp
```

Follow the bundle README to complete Graphify OAuth. Verify discovery and a real
repository query from the published package. Do not treat the browser OAuth
metadata check or `tools/list` alone as sufficient proof.

## Hermes: request official MCP catalog inclusion

1. Confirm the manifest's MIT contribution terms are acceptable and finish the
   Hermes smoke test. This licenses the submitted catalog text, not the backend.
2. In a current fork of `NousResearch/hermes-agent`, copy only
   `integrations/hermes/optional-mcps/graphify/manifest.yaml` from this repository
   to `optional-mcps/graphify/manifest.yaml` in the fork.
3. Follow Hermes's current contribution instructions and validate the manifest
   with its catalog parser. Open a PR titled **Add Graphify hosted MCP to the MCP
   catalog**. A prepared description is in `hermes/CATALOG-PR.md`; fill in the
   actual smoke-test results before submitting it.
4. After Nous Research accepts the entry and it reaches the user's installed
   distribution, verify `hermes mcp catalog` shows Graphify and test
   `hermes mcp install graphify`.

This is the official MCP catalog route. It is not an automatic import from the
official Model Context Protocol Registry, and it does not require a separate
memory-provider or runtime plugin. Direct configuration works independently of
catalog review. Acceptance and release timing are controlled by Nous Research.

## Sources

- [OpenClaw MCP](https://docs.openclaw.ai/tools/mcp)
- [OpenClaw bundle support](https://docs.openclaw.ai/plugins/bundles)
- [ClawHub publishing](https://docs.openclaw.ai/clawhub/publishing)
- [Hermes MCP](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/)
- [Hermes contribution terms](https://github.com/NousResearch/hermes-agent/blob/main/CONTRIBUTING.md)
- [Hermes MCP catalog implementation](https://github.com/NousResearch/hermes-agent/blob/main/hermes_cli/mcp_catalog.py)
