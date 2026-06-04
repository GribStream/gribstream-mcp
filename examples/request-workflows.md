# Request Workflow Examples

These examples show the kinds of tasks the GribStream MCP server is designed to help AI tools complete.

The MCP server should discover exact selectors and validate request bodies before running live queries or returning final `curl` commands.

## GFS 2m Temperature Timeseries

User request:

```text
Build a GribStream request for GFS 2m temperature in Houston for a single valid time.
```

Expected MCP workflow:

1. Use `gribstream_get_dataset` for `gfs`.
2. Use `gribstream_get_parameter` for `TMP`.
3. Confirm selector level `2 m above ground`.
4. Use `gribstream_build_timeseries_request`.
5. Use `gribstream_query_timeseries` to run the authenticated query, or return runnable `curl` with a `GRIBSTREAM_API_TOKEN` placeholder.

Representative request body:

```json
{
  "timesList": ["2026-05-04T12:00:00Z"],
  "coordinates": [
    {
      "lat": 29.7604,
      "lon": -95.3698,
      "name": "Houston"
    }
  ],
  "variables": [
    {
      "name": "TMP",
      "level": "2 m above ground",
      "alias": "temp_k"
    }
  ]
}
```

## ECMWF IFS 10m Wind Speed Expression

User request:

```text
Build an IFS wind-speed request from 10m U and V wind components for Montevideo.
```

Expected MCP workflow:

1. Use `gribstream_get_dataset` for `ifsoper`.
2. Use `gribstream_get_parameter` for `10u` and `10v`.
3. Use aliases such as `u10` and `v10`.
4. Use `gribstream_get_expression_reference`.
5. Build expression `func.Hypot(u10, v10)`.
6. Validate before running the authenticated query or returning final `curl`.

Representative expression fragment:

```json
{
  "variables": [
    {"name": "10u", "level": "sfc", "alias": "u10", "hidden": true},
    {"name": "10v", "level": "sfc", "alias": "v10", "hidden": true}
  ],
  "expressions": [
    {"expression": "func.Hypot(u10, v10)", "alias": "wind_speed_ms"}
  ]
}
```

## Compare Forecast Runs

User request:

```text
Compare how GFS forecasts changed across recent runs for one valid time.
```

Expected MCP workflow:

1. Use `/runs`, not `/timeseries`.
2. Treat `timesList`, `forecastedFrom`, and `forecastedUntil` as UTC model run times.
3. Use `gribstream_build_runs_request`.
4. Explain that `/runs` is for model-run history and forecast aging workflows.

## Choose a Dataset

User request:

```text
What GribStream dataset should I use for short-range US precipitation forecasts?
```

Expected MCP workflow:

1. Use `gribstream_list_datasets` with relevant filters or tags.
2. Compare GFS, HRRR, NBM, and any regional datasets that fit the domain.
3. Consider region, forecast horizon, cadence, archive coverage, and parameter availability.
4. Recommend a dataset and build a small validated request if the user wants one.
