[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Vaisala CL61

Vaisala CL61 ceilometers. See also other [Vaisala ceilometers](vaisala-ceilometers.html).

Instrument ID: `cl61d`

## Recommended files

| File   | Format | Description                                       |
| ------ | ------ | ------------------------------------------------- |
| `*.nc` | netCDF | Non-concatenated files, e.g. 1 or 5 minute files. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "cl61d"
instrument_pid = "https://hdl.handle.net/21.12132/3.241bda142975460b"
path_fmt       = "/data/cl61/%Y%m%d/live_%Y%m%d_*.nc"
```

## Calibration

| Field Name           | Type    | Unit | Description         |
| -------------------- | ------- | ---- | ------------------- |
| `calibration_factor` | `float` | 1    | Calibration factor. |
