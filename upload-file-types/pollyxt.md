[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# PollyXT lidar

PollyXT Ramas lidar.

Instrument ID: `pollyxt`

## Recommended files

| File             | Format | Description                  |
| ---------------- | ------ | ---------------------------- |
| `*_att_bsc.nc`   | netCDF | Attenuated backscatter.      |
| `*_vol_depol.nc` | netCDF | Volume depolarisation ratio. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "mindelo"
instrument     = "pollyxt"
instrument_pid = "https://hdl.handle.net/21.12132/3.3b8d6fb7e7e24f1d"
path_fmt       = "/data/polly/%Y%m%d/%Y_%m_%d*.nc"
```

## Calibration

| Field Name  | Type    | Unit | Description                                           |
| ----------- | ------- | ---- | ----------------------------------------------------- |
| `snr_limit` | `float` | 1    | Signal-to-noise ratio limit used for screening noise. |
