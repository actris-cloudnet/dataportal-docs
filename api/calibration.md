[Docs home](https://docs.cloudnet.fmi.fi)

# Calibration API reference

This is a documentation for the HTTP API for configuring the calibration
factors used in Cloudnet data processing.

## Routes

### GET /api/calibration

This route takes the following URL parameters:

- `instrumentPid`: Instrument PID
- `date`: Date of the calibration

NOTE: For dates that do not have a calibration factor set, the previous
calibration factor is returned. For example, given that there is a calibration
factor for date 2021-01-01, querying the factor for 2021-01-02 will return the
calibration factor of 2021-01-01.

### POST /api/calibration

The route takes the following URL parameters:

- `instrumentPid`: Instrument id
- `date`: Date for which the calibration is valid

Request body must contain calibration data as JSON.

## Examples

### Fetching the most recent value

Example query:

`GET https://cloudnet.fmi.fi/api/calibration?instrumentPid=https://hdl.handle.net/21.12132/3.cdf99c536bd04146&date=2021-01-01`

Response body:

```json
{
  "createdAt": "2023-04-04T12:59:47.686Z",
  "updatedAt": "2023-04-04T12:59:47.686Z",
  "data": {
    "range_corrected": true,
    "calibration_factor": 4e-12
  }
}
```

### Setting new value

```shell
curl -u username:password \
     -H "Content-Type: application/json" \
     -d '{"range_corrected": true, "calibration_factor": 5e-9}' \
     https://cloudnet.fmi.fi/api/calibration?instrumentPid=https://hdl.handle.net/21.12132/3.cdf99c536bd04146&date=2021-01-01
```
