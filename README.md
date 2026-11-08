# SendGrid MCP server

Inspect existing transactional templates and explicitly create a new dynamic template shell without sending email.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `SENDGRID_API_KEY`



Credential help: https://www.twilio.com/docs/sendgrid/api-reference/transactional-templates/create-a-transactional-template

## Provider setup

Use your own scoped SendGrid API key with template read/write permissions. Base URL is global US; EU subusers need a separately reviewed regional manifest. This connector manages template shells only. Mail Send is absent because its empty HTTP202 response is unsupported by the current JSON receipt runtime.

## Supported scope

- No Mail Send tool. Empty HTTP202 receipts require a future runtime enhancement; template creation returns documented201 JSON.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call sendgrid_list_templates '{}'
```

## Use with an MCP client

Launch `node /absolute/path/sendgrid-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| sendgrid_list_templates | Read | Read the account transactional templates. |
| sendgrid_create_template | Explicit approval required | Create an empty dynamic template shell. No version is activated and no message is sent. |

## Concrete workflow

Inspect existing transactional templates and explicitly create a new dynamic template shell without sending email.

1. sendgrid_list_templates
2. sendgrid_create_template

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-sendgrid-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/sendgrid/).

## Provenance and maintenance

- https://www.twilio.com/docs/sendgrid/api-reference/transactional-templates/create-a-transactional-template
- https://raw.githubusercontent.com/sendgrid/sendgrid-python/main/examples/templates/templates.py

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
