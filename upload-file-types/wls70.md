[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# WindCube Doppler lidar

Leosphere WLS70 WindCube Doppler lidar.

Instrument ID: `wls70`

## Recommended files

| File    | Format | Description           |
| ------- | ------ | --------------------- |
| `*.rtd` | text   | Real time data files. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "palaiseau"
instrument     = "wls70"
instrument_pid = "https://hdl.handle.net/21.12132/3.d5ee0db6ac964a04"
path_fmt       = "/data/dlidar/%Y%m%d/*%Y_%m_%d*.rtd"
```

## Calibration

| Field Name       | Type    | Unit    | Description                                                                      |
| ---------------- | ------- | ------- | -------------------------------------------------------------------------------- |
| `azimuth_offset` | `float` | degrees | Offset value to be added to azimuth angles if instrument doesn't point to North. |
