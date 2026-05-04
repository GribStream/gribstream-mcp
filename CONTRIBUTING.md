# Contributing

Thanks for improving GribStream MCP documentation and examples.

## Scope

Good contributions include:

- Clearer setup instructions for MCP clients.
- Better examples for weather forecast workflows.
- Corrections to public metadata in `server.json`.
- Fixes for broken links, typos, or unclear wording.

Please keep examples focused on the hosted endpoint:

```text
https://gribstream.com/mcp
```

## Checks

```bash
python -m json.tool server.json >/dev/null
```
