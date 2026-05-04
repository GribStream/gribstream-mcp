# Security

## Reporting Security Issues

Please report security concerns privately through the GribStream support contact listed on:

```text
https://gribstream.com/faq
```

Do not open public issues for vulnerabilities, leaked credentials, or private-key material.

## Authentication

The hosted MCP connection is unauthenticated and read-only. It helps AI tools discover datasets, resolve selectors, validate request bodies, and build GribStream API requests.

Generated GribStream API requests may require a user API token when executed outside MCP.
