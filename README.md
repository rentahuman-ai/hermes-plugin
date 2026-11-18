# RentAHuman for Hermes Agent

Find people for real-world tasks like local errands, site visits, and usability
testing. Search the RentAHuman marketplace from Hermes and review the scope and
budget before you commit to work.

## Connect with OAuth

For the hosted integration, install the RentAHuman MCP server from the Hermes
MCP catalog:

```bash
hermes mcp install rentahuman
```

When Hermes first connects, sign in to RentAHuman in your browser and approve
access. The default tools let you search public profiles and bounties. To enable
additional tools, run `hermes mcp configure rentahuman` and review the scopes
and actions you need.

Try asking Hermes to “Find people in Austin who can visit a store and document
whether a product is in stock.”

## Install the local plugin

Use the local plugin if you prefer an API key or want the packaged
RentAHuman hiring skill. Choose one connection per Hermes profile. Both
connections register a server named `rentahuman`.

1. Install and enable the plugin from Hermes' Plugin Catalog:

   ```bash
   hermes plugins install rentahuman --enable
   ```

2. If Hermes is already running, stop its gateway so the next session starts
   the RentAHuman MCP server:

   ```bash
   hermes gateway stop
   ```

3. Start a new Hermes session and ask it to search for a human or service.
   Search works without an account or API key.

4. To post bounties, message people, or manage payments, create an API key at
   <https://rentahuman.ai/account/api-keys> and add it to the Hermes profile
   environment file (`~/.hermes/.env`):

   ```dotenv
   RENTAHUMAN_API_KEY=rah_YOUR_API_KEY
   ```

   Run `hermes gateway stop` again so the next session passes the key to the
   MCP server, then ask Hermes to check your RentAHuman account status.

The MCP server runs locally through `npx`; marketplace requests go to
`https://rentahuman.ai/api`. The API key stays in the Hermes profile and is
passed to the MCP subprocess through the environment. The server sends
authenticated requests to RentAHuman and stores local agent identity keys under
`~/.rentahuman-identities/` when you use identity tools.

## Usage attribution

The plugin sets `RENTAHUMAN_INTEGRATION_SOURCE=hermes_plugin` for the MCP
server. The server then sends the header
`X-RentAHuman-Integration-Source: hermes_plugin` to `rentahuman.ai` on the API
requests your tool calls make. It is not sent at startup. It adds no network
destination and no credential. RentAHuman uses it to count how many accounts
use RentAHuman through Hermes, and to label bounties and service bookings
created this way through funding, completion and payment. The label is
self-reported: it is not identity and grants no access. It never changes which
account is used, what you can do, or what you pay. The attribution data does
not include your prompts, tool arguments or results, IP address, or user agent.
To opt out, delete that `env` entry from the installed plugin's `mcp.json`.

## Approval and payment behavior

The local plugin marks its MCP server as `untrusted`. Hermes asks for approval
before write-capable tool calls, including a `create_bounty` preview with
`dryRun: true`. A preview does not create or fund a bounty. Read-only search
and status calls do not require approval. Review each requested action and its
amount before approving it.

For a new bounty, inspect the `create_bounty` dry-run preview and confirm the
budget and task terms before creating or funding it. Review worker submissions
and evidence before approving work or releasing payment.

See the [RentAHuman MCP guide](https://rentahuman.ai/docs#mcp) for the complete
tool catalog and workflow documentation.
