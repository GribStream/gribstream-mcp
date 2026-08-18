# Changelog

All notable listing and metadata changes for GribStream MCP are tracked here.

## 1.1.4 - 2026-08-17

- Renamed the public listing to GribStream Weather MCP and clarified its NOAA and ECMWF forecast-data coverage.
- Documented signed URL delivery for complete CSV, JSON, NDJSON, and Parquet results.
- Clarified that the 10 MB `max_bytes` limit applies only to inline MCP results, not URL-delivered artifacts.
- Added dense-grid guidance: use one grid request, prefer Parquet when a compatible reader is already available, and otherwise use URL-delivered CSV.
- Documented signed resource-link consumption, artifact metadata, and result sorting requirements for plotting and analysis.

## 1.1.3 - 2026-06-07

- Added LobeHub-friendly Skill List, Prompt List, and Resource List tables to the README.
- Added the LobeHub MCP badge for marketplace claim and validation workflows.
- Added a LobeHub JSON quick-import example using its remote HTTP configuration shape.
- Set the registry homepage URL to the main GribStream weather API landing page while keeping `https://gribstream.com/mcp` as the remote MCP endpoint.
- Added explicit `https://gribstream.com/` homepage references to machine-readable and setup docs.

## 1.1.2 - 2026-06-04

- Documented explicit tool auth labels: `NO AUTH / PUBLIC / READ-ONLY` and `AUTH REQUIRED / DATA QUERY`.
- Documented typed inline CSV/NDJSON resources and `structuredContent.suggested_filename` for saving multiple query results without overwriting earlier files.
- Updated registry metadata description for typed tabular result metadata.

## 1.1.1 - 2026-06-04

- Clarified that discovery, request-building, and validation tools are available before OAuth.
- Clarified that OAuth is required for live `/timeseries` and `/runs` query tools.

## 1.1.0 - 2026-06-04

- Documented OAuth setup for the hosted MCP endpoint, including manual OAuth 2.0 fields for clients that do not fully auto-discover metadata.
- Documented authenticated live `/timeseries` and `/runs` query tools.
- Updated registry metadata with current version, homepage, icon, and repository links.

## 1.0.0 - 2026-05-04

- Added public documentation for the hosted GribStream MCP endpoint.
- Added official MCP Registry metadata for the remote Streamable HTTP server.
- Added client setup and weather request workflow examples.
