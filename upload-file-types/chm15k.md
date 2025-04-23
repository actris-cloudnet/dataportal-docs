[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# CHM 15k ceilometers

Lufft ceilometers (formerly Jenoptik).

Instrument ID: `chm15k` or `chm15kx`

## Recommended files

| File   | Format | Description             |
| ------ | ------ | ----------------------- |
| `*.nc` | netCDF | Daily or smaller files. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "bucharest"
instrument     = "chm15k"
instrument_pid = "https://hdl.handle.net/21.12132/3.c60c931fac9d43f0"
path_fmt       = "/data/ceilo/%Y%m%d/%Y%m%d_*.nc"
```

## Calibration

| Field Name           | Type    | Unit | Description         |
| -------------------- | ------- | ---- | ------------------- |
| `calibration_factor` | `float` | 1    | Calibration factor. |
