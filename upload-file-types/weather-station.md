[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Weather station

Meteorological data from an automated weather station (temperature, humidity, etc.).

Instrument ID: `weather-station`

## Recommended files

| File                 | Format  | Description                 |
| -------------------- | ------- | --------------------------- |
| `*.txt`, `*.nc` etc. | unknown | No strict format specified. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "weather-station"
instrument_pid = "https://hdl.handle.net/21.12132/3.80082867c5744f11"
path_fmt       = "/data/aws/%Y%m%d/%Y%m%d.txt"
```

## Calibration

| Field Name    | Type  | Unit | Description                                                                                          |
| ------------- | ----- | ---- | ---------------------------------------------------------------------------------------------------- |
| `time_offset` | `int` | min  | Time offset compared to UTC (positive values mean ahead of UTC, e.g. 120 for Finnish standard time). |
