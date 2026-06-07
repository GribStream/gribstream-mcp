# Client Configuration Examples

Hosted MCP endpoint:

```text
https://gribstream.com/mcp
```

Official GribStream weather API homepage:

```text
https://gribstream.com/
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

## LobeHub Quick Import

Use this shape in LobeHub's JSON quick-import flow:

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

## OAuth 2.0 Fields

Many MCP clients discover OAuth automatically. If a client opens a manual OAuth 2.0 form, use:

```text
Authorization URL: https://gribstream.com/authorize
Token URL: https://gribstream.com/token
Client ID: gribstream-mcp-public
Client Secret: leave empty; do not enter a value
Scopes: leave empty
Resource or audience, if requested: https://gribstream.com/mcp
```

The public OAuth metadata endpoints are:

```text
https://gribstream.com/.well-known/oauth-protected-resource/mcp
https://gribstream.com/.well-known/oauth-authorization-server
```

Native desktop clients can use loopback redirect URIs on `localhost`, `127.0.0.1`, or `[::1]`.

If the client requires a non-empty client secret, it is not using the public-client OAuth flow supported by GribStream MCP.

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

The hosted MCP server exposes discovery, request-building, and validation tools before OAuth. Live query tools use OAuth. During authorization, GribStream asks you to select one active GribStream API token for the MCP connection. The raw API token is not shown to the MCP client.

When you run a generated GribStream API request outside MCP, set:

```bash
export GRIBSTREAM_API_TOKEN='YOUR_TOKEN_HERE'
```

Create tokens from: https://gribstream.com/app/dashboard
