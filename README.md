# GribStream Weather MCP

[![MCP Badge](https://lobehub.com/badge/mcp/gribstream-gribstream-mcp)](https://lobehub.com/mcp/gribstream-gribstream-mcp)

GribStream Weather MCP is a hosted Model Context Protocol server for AI access to weather forecast data from GribStream, including NOAA and ECMWF model output.

It helps AI agents discover forecast datasets such as NOAA GFS, HRRR, and NBM or ECMWF IFS, resolve exact forecast variables and levels, validate request bodies, build runnable GribStream Weather API requests, and execute live read-only weather queries after OAuth authorization.

Official GribStream weather API homepage:

```text
https://gribstream.com/
```

Hosted MCP endpoint:

```text
https://gribstream.com/mcp
```

## Use Cases

Use GribStream MCP when you want an AI tool to:

- List public GribStream weather datasets.
- Choose a NOAA, ECMWF, or other forecast model for a region, horizon, or weather task.
- Query GFS, HRRR, NBM, IFS, and other numerical weather prediction model data exposed by GribStream.
- Find exact variable selectors, levels, and aliases.
- Build a `/timeseries` request for values by valid time.
- Build a `/runs` request to compare forecasts across model runs.
- Validate a GribStream API request before running it.
- Execute live `/timeseries` or `/runs` queries after OAuth authorization.
- Retrieve dense forecast grids as downloadable files without embedding the complete result in the conversation.
- Generate a copy-pasteable `curl` command for the GribStream Weather API.

The hosted MCP server exposes discovery, selector lookup, request building, and validation tools before OAuth. Live `/timeseries` and `/runs` query tools require OAuth. Tool descriptions begin with either `NO AUTH / PUBLIC / READ-ONLY` or `AUTH REQUIRED / DATA QUERY` so AI clients can route metadata tasks and live data-query tasks correctly. When a client connects through OAuth, GribStream asks the user to sign in and select an active GribStream API token. MCP access tokens are scoped to that selected API token, and the raw API token is not shown to the MCP client.

## Connect

Use the hosted endpoint exactly as shown in any MCP client that supports remote Streamable HTTP:

```text
https://gribstream.com/mcp
```

Do not include trailing punctuation in the URL.

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

LobeHub quick import configuration:

```json
{
  "mcpServers": {
    "gribstream": {
      "type": "http",
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

## OAuth

OAuth is needed for live query tools. Most MCP clients should discover OAuth from the hosted endpoint automatically, either during connector setup or when the client requests authenticated tools. If a client asks for manual OAuth 2.0 fields, use:

```text
Authorization URL: https://gribstream.com/authorize
Token URL: https://gribstream.com/token
Client ID: gribstream-mcp-public
Client Secret: leave empty; do not enter a value
Scopes: leave empty
Resource or audience, if requested: https://gribstream.com/mcp
```

OAuth discovery endpoints:

```text
https://gribstream.com/.well-known/oauth-protected-resource/mcp
https://gribstream.com/.well-known/oauth-authorization-server
```

The OAuth client is public and uses PKCE with `token_endpoint_auth_method` set to `none`. Native desktop clients may use loopback redirect URIs such as `http://localhost:{port}/...`, `http://127.0.0.1:{port}/...`, or `http://[::1]:{port}/...`. A setup form that requires a non-empty client secret is asking for a confidential-client flow and is not compatible with this public MCP OAuth flow.

## Server Features

LobeHub and other MCP crawlers may call MCP tools "skills". The live GribStream MCP server exposes the following skills/tools, prompts, and resources.

### Skill List

| Name | Auth | Description |
| --- | --- | --- |
| `gribstream_list_datasets` | No auth, public, read-only | List public GribStream datasets and metadata. |
| `gribstream_get_dataset` | No auth, public, read-only | Get full public metadata for one dataset code. |
| `gribstream_list_parameters` | No auth, public, read-only | List parameter summaries for one dataset. |
| `gribstream_get_parameter` | No auth, public, read-only | Get detailed metadata and exact selector variations for one dataset parameter. |
| `gribstream_list_shared_parameters` | No auth, public, read-only | List normalized shared parameter presets. |
| `gribstream_resolve_shared_parameter` | No auth, public, read-only | Resolve one shared parameter preset to native variables and expressions. |
| `gribstream_get_expression_reference` | No auth, public, read-only | Get GribStream expression-language rules, examples, and math functions. |
| `gribstream_build_timeseries_request` | No auth, public, read-only | Build and validate a copy-pasteable authenticated `/timeseries` API request without sending it. |
| `gribstream_build_runs_request` | No auth, public, read-only | Build and validate a copy-pasteable authenticated `/runs` API request without sending it. |
| `gribstream_validate_request` | No auth, public, read-only | Validate request shape, selector tuples, and expression syntax without querying weather values. |
| `gribstream_query_timeseries` | OAuth required, data query | Execute an authenticated `/timeseries` request and return actual weather values. |
| `gribstream_query_runs` | OAuth required, data query | Execute an authenticated `/runs` request and return actual weather values across model runs. |

Public read-only skills/tools are visible before OAuth. OAuth live query skills/tools may be hidden from `tools/list` until the client completes OAuth or sends a valid `Authorization: Bearer` token.

### Prompt List

| Name | Description |
| --- | --- |
| `build_gribstream_api_request` | Turn a weather question into a validated GribStream `/timeseries` or `/runs` request and runnable curl. |
| `build_gfs_forecast_request` | Create a validated GFS request for common forecast variables at points or grids. |
| `build_historical_forecast_request` | Build a request for archived forecasts or forecasts selected by a model-run-time cutoff. |
| `create_grid_request` | Create a GribStream request over a latitude/longitude grid instead of point coordinates. |
| `compare_forecast_runs` | Build a `/runs` request to compare model forecasts across runs or lead times. |
| `choose_weather_dataset` | Choose a GribStream dataset for a region, horizon, weather signal, or forecast task. |
| `find_weather_variable` | Find the exact dataset parameter selectors for a weather signal. |

### Resource List

| URI | MIME Type | Description |
| --- | --- | --- |
| `gribstream://openapi` | `application/yaml` | Machine-readable GribStream OpenAPI specification. |
| `gribstream://skill/gribstream-query` | `text/markdown` | Vendor-neutral behavioral instructions for AI tools using GribStream. |

### Live Query Output

Authenticated live query tools support CSV, JSON, NDJSON, and Parquet results. Small CSV, JSON, and NDJSON results can be returned inline. Set `delivery=url` for dense grids or large results to receive a short-lived signed HTTPS `resource_link` to the complete file instead.

- URL delivery works with CSV, JSON, NDJSON, and Parquet. Parquet always uses URL delivery.
- Download the exact signed URL once without an API token. This consumes the MCP result; it is not a separate direct GribStream API request.
- The 10 MB `max_bytes` limit applies only to inline MCP results. URL artifacts use a separate server-configured size limit, so omit `max_bytes` when using `delivery=url`.
- For dense grids, prefer Parquet when a compatible reader is already installed; otherwise use CSV with `delivery=url`. Do not install a dependency or write a Parquet parser solely for one result.
- Use one grid request and only split after an actual request or artifact-size error. If splitting is necessary, split time first, variables second, and space last.
- `structuredContent` includes metadata such as `content_type`, `rows`, `bytes`, `columns`, `schema`, `delivery`, `cache_hit`, `signed_url_expires_at`, and `suggested_filename` when applicable.
- Inline tabular results also include a preview plus `request_hash`, `response_hash`, and `result_hash`. Use `suggested_filename` when saving multiple results so repeated requests or comparisons do not overwrite earlier files.
- Rows are streamed from parallel extraction and are not guaranteed to be sorted. Sort by `forecasted_time` for `/timeseries`, and by `forecasted_at` plus `forecasted_time` for `/runs`, before plotting or comparing time series.

## Links

- Official GribStream weather API homepage: `https://gribstream.com/`
- Endpoint: `https://gribstream.com/mcp`
- MCP page: `https://gribstream.com/mcp`
- Icon: `https://gribstream.com/gribstream_small.png`
- OpenAPI: `https://gribstream.com/docs/openapi.yaml`
- AI setup docs: `https://gribstream.com/ai`
- API dashboard: `https://gribstream.com/app/dashboard`

## Try It

After connecting GribStream MCP to an AI tool, ask:

```text
Use GribStream MCP to build a GFS 2m temperature timeseries request for Houston for tomorrow at 12:00 UTC. Validate the request and return a runnable curl command.
```

If your client has completed OAuth, you can also ask it to run the query:

```text
Use GribStream MCP to run a GFS 2m temperature timeseries query for Houston for tomorrow at 12:00 UTC and return CSV.
```

For a dense-grid visualization:

```text
Using GribStream, plot the latest available ICON-D2 2 metre temperature forecast over Germany at 0.025 degree resolution as a beautiful PNG.
```

Or:

```text
Use GribStream MCP to find the best dataset for short-range precipitation forecasts in the continental United States. Explain the tradeoffs and build a validated example request.
```

More examples:

- [Client configuration examples](examples/client-config.md)
- [Weather request workflows](examples/request-workflows.md)

For the GribStream API itself, see the [OpenAPI specification](https://gribstream.com/docs/openapi.yaml).

[![MCP Badge](https://lobehub.com/badge/mcp/gribstream-gribstream-mcp)](https://lobehub.com/mcp/gribstream-gribstream-mcp)
