[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Copernicus cloud radar

Copernicus cloud radar.

Instrument ID: `copernicus`

## Recommended files

| File   | Format | Description          |
| ------ | ------ | -------------------- |
| `*.nc` | netCDF | Pre-processed files. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "chilbolton"
instrument     = "copernicus"
instrument_pid = "https://hdl.handle.net/21.12132/3.c3e825b4aa71435c"
path_fmt       = "/data/copernicus/%Y%m%d/radar-copernicus_%Y%m%d*.nc"
```

## Calibration

| Field Name           | Type    | Unit | Description                               |
| -------------------- | ------- | ---- | ----------------------------------------- |
| `calibration_offset` | `float` | 1    | Calibration offset added to reflectivity. |
| `range_offset`       | `int`   | m    | Offset added to range.                    |
