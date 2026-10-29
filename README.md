# RentAHuman for Hermes Agent

Connect Hermes to RentAHuman's MCP server to find people and manage real-world
work. The package uses the published `rentahuman-mcp` server and pins its npm
version so updates happen through a reviewed catalog release.

## Setup

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
passed to the MCP subprocess through the environment.

Use either this plugin or the `hermes mcp install rentahuman` remote server in
a profile, not both. Both register a server named `rentahuman`, and Hermes
skips this plugin's server when the other is configured.

## Approval and payment behavior

The plugin marks the MCP server as `untrusted`. Hermes asks for approval before
each tool call that can change state, send a message, accept a worker, or move
money, including a `create_bounty` preview with `dryRun: true`. Read-only search
and status calls do not require approval. Review each requested action and its
amount before approving it.

For a new bounty, inspect the `create_bounty` dry-run preview and confirm the
budget and task terms before creating or funding it. Review worker submissions
and evidence before approving work or releasing payment.

See the [RentAHuman MCP guide](https://rentahuman.ai/docs#mcp) for the complete
tool catalog and workflow documentation.
