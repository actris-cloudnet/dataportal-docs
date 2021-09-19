[Docs home](https://docs.cloudnet.fmi.fi)

# Calibration API reference

This is a documentation for the HTTP API for configuring the calibration factors used in
Cloudnet data processing.

## Routes

### GET /api/calibration

This route takes the following URL parameters:

- `site`: Site id
- `instrument`: Instrument id
- `date`: Date of the calibration
- `showAll`: Boolean, show history of previous calibration factors. By default, only the most recent calibration factor is returned.

NOTE: For dates that do not have a calibration factor set, the previous calibration factor is returned.
Example: given that there is a calibration factor for date 2021-01-01, querying the factor for 2021-01-02 will return the calibration factor of 2021-01-01.

### POST /calibration

The route takes the following parameters in a JSON object:

- `site`: Site id
- `instrument`: Instrument id
- `date`: Date for which the calibration is valid
- `calibrationFactor`: Calibration factor as a number

NOTE: Credentials are required for posting new calibration factors.
Contact [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi) if you have
any questions.

## Examples

### Fetching the most recent value

Example query:

`GET https://cloudnet.fmi.fi/api/calibration?site=lindenberg&instrument=chm15k&date=2021-01-01`

Response body:

```json
[
  {
    "createdAt": "2021-03-18T11:08:40.527Z",
    "calibrationFactor": 4e-12
  }
]
```

### Setting new value

```shell
curl -u username:password -H "Content-Type: application/json" -d '{"site": "schneefernerhaus", "date":"2012-01-01","instrument":"chm15k", "calibrationFactor":5e-9}' https://cloudnet.fmi.fi/calibration/
```
