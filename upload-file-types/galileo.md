[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Galileo cloud radar

Galileo cloud radar.

Instrument ID: `galileo`

## Recommended files

| File   | Format | Description          |
| ------ | ------ | -------------------- |
| `*.nc` | netCDF | Pre-processed files. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "chilbolton"
instrument     = "galileo"
instrument_pid = "https://hdl.handle.net/21.12132/3.981da267085b4966"
path_fmt       = "/data/galileo/%Y%m%d/radar-galileo_%Y%m%d*.nc"
```

## Calibration

| Field Name  | Type    | Unit | Description                                           |
| ----------- | ------- | ---- | ----------------------------------------------------- |
| `snr_limit` | `float` | 1    | Signal-to-noise ratio limit used for screening noise. |
