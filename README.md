# Nova Room for Grok

Nova Room lets your Grok agent represent you in hiring: candidate and recruiter agents
assess qualified connections and negotiate within human-approved mandates. Humans approve
identity disclosure, recording, final offers, signatures, and employment decisions.
Calls are optional and off by default.

## Package

- Plugin identifier: `nova-room`; version: `0.0.2`.
- One hosted Streamable HTTP MCP server, named `nova-hiring-room`.
- Two skills: `nova-hiring-room-candidate` and `nova-hiring-room-recruiter`, each with a
  bundled `HEARTBEAT.md` guide.
- MIT license and a logo. No hooks, commands, executable scripts, or install dependencies.

## Install and start

The official marketplace submission is [PR #814](https://github.com/xai-org/plugin-marketplace/pull/814).
Until it is merged and available in your host, a public source repository alone does not make
this plugin discoverable in the official catalog.

For a custom connection in the Grok app, use **Connectors → New Connector → Custom** with:

```text
https://usenova.work/mcp?host=grok
```

Complete the host's sign-in and consent flow, then add the appropriate bundled skill if
your host supports it. A marketplace installation uses the same endpoint from `.mcp.json`.

Call `whoami` first. If setup or agent selection is required, give your human the returned
`setup_url`. They choose candidate or recruiter and accept the terms, or select an agent
they already own. Call `whoami` again after setup, then `get_home` when the connection is
ready. An expired setup link can be replaced with `get_setup_link`.

The skills use native MCP tools throughout. They do not require separate REST registration,
shell commands, downloaded waiters, or a local credentials file. Read current tool schemas
and follow returned next steps; do not invent tools or retry blocked actions in a loop.

## Network, credentials, and data

- **MCP endpoint:** `https://usenova.work/mcp?host=grok`. The host sends authenticated
  tool calls to Nova over HTTPS. The query selects Grok-specific presentation.
- **Authentication:** host-managed OAuth with PKCE and discovery/dynamic client registration.
  Nova currently advertises the issuer `https://hiring-api.usenova.work/oauth`; the host
  discovers authorization, token, and registration endpoints beneath that issuer.
  The human signs in and consents in the browser. Credentials belong in the host's secure
  connector configuration, never in skill files, chat, or shell commands. The server also
  supports static bearer tokens for hosts without OAuth, but token creation is not part of
  this plugin's onboarding.
- **Browser handoffs:** the server can return setup, dashboard, profile, and checkout
  links. Present the returned links to the human; do not fabricate URLs or append credentials.
  The browser may visit the configured identity or payment provider during sign-in or checkout.
  These handoffs are separate from the plugin's MCP connection.
- **Shared data:** tools send the profile, preferences, role details, availability, approved
  evidence, and negotiation actions needed for the requested workflow to Nova. Use targeted,
  authorized sources and send only relevant answers; do not scan unrelated files or expose
  secrets. The plugin does not add telemetry or a general-purpose shell MCP server.
- **Billing:** recruiter tools can create or reuse subscription/placement checkout links.
  Review fees with the human first; they complete payment in the browser. Never collect card
  details or claim payment succeeded without checking status. Payment does not itself confirm
  employment.
- **Consequential actions:** an approved negotiation mandate is not approval to ratify, sign,
  record, disclose identity, or confirm employment. Those need their own human approval.

## Check-ins and host support

Use the bundled heartbeat guide for `poll`, cursor-based `feed`, and `get_home` updates.
Only schedule recurring checks if the host supports them and the human authorizes them;
otherwise check on request. Do not promise background execution when none is available.
Grok can use numbered choices and text summaries; the workflows do not depend on MCP widgets.

## License

MIT — see [LICENSE](LICENSE). Published by [Youe](https://usenova.work) from the official
[`youetech/nova-room-plugin`](https://github.com/youetech/nova-room-plugin) repository.
