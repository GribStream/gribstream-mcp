# Using GribStream MCP

Connect your MCP client to:

```text
https://gribstream.com/mcp
```

The server uses remote MCP over Streamable HTTP.

## Authentication

The hosted MCP endpoint allows read-only discovery, selector lookup, request building, and validation before OAuth. Live `/timeseries` and `/runs` query tools require OAuth. Tool descriptions start with either `NO AUTH / PUBLIC / READ-ONLY` or `AUTH REQUIRED / DATA QUERY`.

Most clients should discover the OAuth flow automatically from the `WWW-Authenticate` challenge and the public metadata endpoints. If your client asks for manual OAuth 2.0 configuration, use:

```text
Authorization URL: https://gribstream.com/authorize
Token URL: https://gribstream.com/token
Client ID: gribstream-mcp-public
Client Secret: leave empty; do not enter a value
Scopes: leave empty
Resource or audience, if requested: https://gribstream.com/mcp
```

The OAuth flow asks you to sign in to GribStream and select an active API token for live MCP queries. The MCP client receives MCP OAuth tokens; it does not receive your raw GribStream API token.

If a setup form requires a non-empty client secret, it is asking for a confidential-client OAuth flow. GribStream MCP uses a public OAuth client with PKCE and `token_endpoint_auth_method` set to `none`.

If you run a generated API request outside MCP, set a GribStream API token:

```bash
export GRIBSTREAM_API_TOKEN='YOUR_TOKEN_HERE'
```

Create tokens from:

```text
https://gribstream.com/app/dashboard
```

## Common Workflow

A typical AI workflow is:

1. Choose a dataset with `gribstream_list_datasets` or `gribstream_get_dataset`.
2. Find exact variables with `gribstream_list_parameters`, `gribstream_get_parameter`, or `gribstream_resolve_shared_parameter`.
3. Use aliases for variables that feed expressions.
4. Build a `/timeseries` or `/runs` request.
5. Validate the request.
6. Run the query through authenticated MCP, or run the generated `curl` command with your API token.

## Saving Query Results

Authenticated live query tools return CSV and NDJSON as typed inline MCP resources. They also include metadata in `structuredContent`.

When saving a live query result, use `structuredContent.suggested_filename` instead of a generic name such as `timeseries.csv` or `runs.csv`. Suggested filenames include the dataset, endpoint, request hints, and a short result hash, so repeated queries or comparisons across models do not overwrite earlier files.

For comparison or plotting workflows:

1. Save each MCP result with its own `suggested_filename`.
2. Sort `/timeseries` rows by `forecasted_time`.
3. Sort `/runs` rows by `forecasted_at` and `forecasted_time`.
4. Compare only aligned timestamps or model-run timestamps.

## Endpoint Choice

Use `/timeseries` for values by valid time.

Use `/runs` when comparing how forecasts changed across model runs or when working with forecast-aging workflows.

## Example Prompts

```text
Use GribStream MCP to build a GFS 2m temperature timeseries request for Houston for tomorrow at 12:00 UTC. Validate the request and return a runnable curl command.
```

```text
Use GribStream MCP to find exact ECMWF IFS 10m wind selectors for Montevideo and build an expression for wind speed.
```

```text
Use GribStream MCP to compare recent GFS forecast runs for 2m temperature at Chicago for one valid time.
```

## References

- GribStream MCP page: https://gribstream.com/mcp
- GribStream AI docs: https://gribstream.com/ai
- OpenAPI spec: https://gribstream.com/docs/openapi.yaml
- GribStream dashboard: https://gribstream.com/app/dashboard
