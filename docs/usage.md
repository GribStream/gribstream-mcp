# Using GribStream MCP

Connect your MCP client to:

```text
https://gribstream.com/mcp
```

The server uses remote MCP over Streamable HTTP.

## Authentication

The MCP connection does not require authentication.

The server builds and validates GribStream API requests. When you run a generated API request outside MCP, set a GribStream API token:

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
6. Run the generated `curl` command with your API token.

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
