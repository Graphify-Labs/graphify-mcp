# OpenClaw and Hermes publishing handoff

Prepared on 6 October 2026. These packages connect to the existing hosted
Graphify MCP service. No backend, OAuth, or main Graphify OSS change is required
by these manifests. Neither listing has been submitted by this preparation.

## Remaining decisions

1. Confirm a Graphify-owned ClawHub publisher handle and grant the person
   publishing access to that owner. `@graphify-labs/graphify-mcp` is provisional;
   the package scope must match the actual owner handle. A founder's personal
   account is not inherently required when your account has the needed access.
2. Approve a license for these small public manifests, documentation, skill, and
   icon. The OpenClaw package currently says `UNLICENSED`; it is not licensed as
   open source. A permissive license for the integration files would not expose
   or relicense the private hosted backend. The Hermes upstream contribution is
   governed by that project's MIT contribution terms. Keep Graphify trademark
   and logo rights distinct if adopting an open-source license.
3. Complete the two authenticated client smoke tests below. Package validation
   and public OAuth metadata checks are not proof of successful sign-in.

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

## OpenClaw: publish the bundle on ClawHub

Use the prepared `integrations/openclaw` directory, not the repository root.
It contains a Graphify icon, MCP configuration, a usage skill, and both the
ClawHub catalog metadata and a compatible Claude-format bundle marker.
Do not add `openclaw.extensions` or a JavaScript entrypoint: this is a bundle.

After review, merge the integration files into the public Graphify MCP repo.
Check out the merged commit with a clean working tree. Confirm the publisher
scope and license first. With Node.js 22 or newer for ClawHub:

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
  --changelog "Initial Graphify hosted MCP bundle with browser OAuth setup and code investigation guidance." \
  --dry-run
```

Replace `graphify-labs` and the package scope together if the owner handle differs.
Review the preview. Then repeat the publish command with `--wait` in place of
`--dry-run`. Publishing is an external release; security checks can hold or reject
it. A successful upload is not the same as a public listing.

Once public, install using the exact published package identity:

```sh
openclaw plugins install clawhub:@graphify-labs/graphify-mcp
```

Follow the bundle README to complete Graphify OAuth. Verify discovery and a real
repository query from the published package, then add its live listing URL to
the root README. Do not advertise a live install until publication is confirmed.

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
