# GribStream MCP

GribStream MCP is a hosted Model Context Protocol server for weather forecast data.

It helps AI agents discover GribStream datasets, resolve exact forecast variables and levels, validate request bodies, and build runnable GribStream Weather API requests for GFS, ECMWF IFS, NBM, HRRR, and other models.

Hosted MCP endpoint:

```text
https://gribstream.com/mcp
```

## Use Cases

Use GribStream MCP when you want an AI tool to:

- List public GribStream weather datasets.
- Choose a forecast model for a region, horizon, or weather task.
- Find exact variable selectors, levels, and aliases.
- Build a `/timeseries` request for values by valid time.
- Build a `/runs` request to compare forecasts across model runs.
- Validate a GribStream API request before running it.
- Generate a copy-pasteable `curl` command for the GribStream Weather API.

The hosted MCP server is read-only. It builds and validates requests, but it does not execute live `/timeseries` or `/runs` data queries. Generated API requests may require a GribStream API token when you run them outside MCP.

## Connect

Use the hosted endpoint in any MCP client that supports remote Streamable HTTP:

```text
https://gribstream.com/mcp
```

Generic configuration shape:

```json
{
  "mcpServers": {
    "gribstream": {
      "url": "https://gribstream.com/mcp"
    }
  }
}
```

Some clients require an explicit transport field:

```json
{
  "mcpServers": {
    "gribstream": {
      "type": "streamable-http",
      "url": "https://gribstream.com/mcp"
    }
  }
}
```

## Capabilities

Hosted tools:

- `gribstream_list_datasets`
- `gribstream_get_dataset`
- `gribstream_list_parameters`
- `gribstream_get_parameter`
- `gribstream_list_shared_parameters`
- `gribstream_resolve_shared_parameter`
- `gribstream_get_expression_reference`
- `gribstream_build_timeseries_request`
- `gribstream_build_runs_request`
- `gribstream_validate_request`

Hosted prompts:

- `build_gribstream_api_request`
- `build_gfs_forecast_request`
- `build_historical_forecast_request`
- `create_grid_request`
- `compare_forecast_runs`
- `choose_weather_dataset`
- `find_weather_variable`

Hosted resources:

- `gribstream://openapi`
- `gribstream://skill/gribstream-query`

## Links

- Endpoint: `https://gribstream.com/mcp`
- Homepage: `https://gribstream.com/mcp`
- Icon: `https://gribstream.com/gribstream_small.png`
- OpenAPI: `https://gribstream.com/docs/openapi.yaml`
- AI setup docs: `https://gribstream.com/ai`
- API dashboard: `https://gribstream.com/app/dashboard`

## Try It

After connecting GribStream MCP to an AI tool, ask:

```text
Use GribStream MCP to build a GFS 2m temperature timeseries request for Houston for tomorrow at 12:00 UTC. Validate the request and return a runnable curl command.
```

Or:

```text
Use GribStream MCP to find the best dataset for short-range precipitation forecasts in the continental United States. Explain the tradeoffs and build a validated example request.
```

More examples:

- [Client configuration examples](examples/client-config.md)
- [Weather request workflows](examples/request-workflows.md)

For the GribStream API itself, see the [OpenAPI specification](https://gribstream.com/docs/openapi.yaml).
