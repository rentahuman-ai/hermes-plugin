# RentAHuman for Hermes Agent

Connect Hermes to RentAHuman's MCP server to find people and manage real-world
work. The package uses the published `rentahuman-mcp` server and pins its npm
version so updates happen through a reviewed catalog release.

## Setup

1. Create an API key at <https://rentahuman.ai/account/api-keys>.
2. Add it to the Hermes profile environment file (`~/.hermes/.env`):

   ```dotenv
   RENTAHUMAN_API_KEY=rah_YOUR_API_KEY
   ```

3. Install and enable the RentAHuman plugin from Hermes' Plugin Catalog.
4. Start a new Hermes session. Ask it to search for a human or service to
   verify the connection.

The MCP server runs locally through `npx`; marketplace requests go to
`https://rentahuman.ai/api`. The API key stays in the Hermes profile and is
passed to the MCP subprocess through the environment.

## Approval and payment behavior

The plugin marks the MCP server as `untrusted`. Hermes asks for approval before
each tool call that changes state, sends a message, accepts a worker, or moves
money. Read-only search and status calls do not require approval. Review each
requested action and its amount before approving it.

For a new bounty, inspect the `create_bounty` dry-run preview and confirm the
budget and task terms before creating or funding it. Review worker submissions
and evidence before approving work or releasing payment.

See the [RentAHuman MCP guide](https://rentahuman.ai/docs#mcp) for the complete
tool catalog and workflow documentation.
