# Security

## Reporting Security Issues

Please report security concerns privately through the GribStream support contact listed on:

```text
https://gribstream.com/faq
```

Do not open public issues for vulnerabilities, leaked credentials, or private-key material.

## Authentication

The hosted MCP endpoint uses OAuth for MCP JSON-RPC requests. Public OAuth metadata is available so MCP clients can discover the authorization flow.

During authorization, GribStream asks the user to sign in and select an active GribStream API token for the MCP connection. The MCP client receives MCP OAuth access and refresh tokens, not the raw GribStream API token.

MCP queries are read-only and count against the selected API token quota. Users can disconnect MCP clients from the GribStream dashboard.
