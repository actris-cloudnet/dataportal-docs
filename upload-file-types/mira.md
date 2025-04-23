[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# MIRA cloud radars

METEK MIRA-10 or MIRA-35 (formerly MIRA-36) cloud radar.

Instrument ID: `mira-10` or `mira-35`

## Recommended files

| File                      | Format | Description                                     |
| ------------------------- | ------ | ----------------------------------------------- |
| `*.znc.gz` or `*.znc`     | netCDF | Newer format, preferably compressed using gzip. |
| `*.mmclx.gz` or `*.mmclx` | netCDF | Older format, preferably compressed using gzip. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "bucharest"
instrument     = "mira-35"
instrument_pid = "https://hdl.handle.net/21.12132/3.d98f6fd2bec94e5e"
path_fmt       = "/data/mira/%Y%m%d_*.mmclx.gz"
```

## Calibration

| Field Name       | Type    | Unit    | Description                                           |
| ---------------- | ------- | ------- | ----------------------------------------------------- |
| `azimuth_offset` | `float` | degrees | Offset added to azimuth angle.                        |
| `zenith_offset`  | `float` | degrees | Offset added to zenith angle.                         |
| `snr_limit`      | `float` | 1       | Signal-to-noise ratio limit used for screening noise. |
