# Client Configuration Examples

Hosted MCP endpoint:

```text
https://gribstream.com/mcp
```

## Generic Remote MCP

Use this shape for clients that accept remote MCP server URLs:

```json
{
  "mcpServers": {
    "gribstream": {
      "url": "https://gribstream.com/mcp"
    }
  }
}
```

Some clients use `type` or `transport` fields. If required, set the transport to Streamable HTTP:

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

## Usage Prompt

After connecting, try:

```text
Use GribStream MCP to build a GFS 2m temperature timeseries request for Houston for tomorrow at 12:00 UTC. Validate the request and return a runnable curl command.
```

Or:

```text
Use GribStream MCP to find the best dataset for short-range precipitation forecasts in the continental United States. Explain the tradeoffs and build a validated example request.
```

## API Token Note

The hosted MCP server does not require authentication. It builds and validates requests.

When you run the generated GribStream API request outside MCP, set:

```bash
export GRIBSTREAM_API_TOKEN='YOUR_TOKEN_HERE'
```

Create tokens from: https://gribstream.com/app/dashboard
