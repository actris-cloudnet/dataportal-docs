[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# WindCube Doppler lidar

Vaisala/Leosphere WindCube long-range scanning Doppler lidars.

Instrument ID: `wls100s`, `wls200s`, `wls400s`

## Recommended files

| File                         | Format | Description                           |
| ---------------------------- | ------ | ------------------------------------- |
| `*_fixed_*.nc`               | netCDF | Vertical stare measurements.          |
| `*_vad_*.nc`                 | netCDF | Velocity-azimuth display (VAD) scans. |
| `*_dbs_*.nc`                 | netCDF | Doppler-beam swinging (DBS) scans.    |
| `*_environmental_data_*.csv` | text   | Housekeeping data.                    |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "payerne"
instrument     = "wls200s"
instrument_pid = "https://hdl.handle.net/21.12132/3.a3c0f804d91e4966"
path_fmt       = "/data/dlidar/%Y%m%d/*%Y-%m-%d*.{nc,csv}"
```

## Calibration

| Field Name       | Type    | Unit    | Description                                                                      |
| ---------------- | ------- | ------- | -------------------------------------------------------------------------------- |
| `azimuth_offset` | `float` | degrees | Offset value to be added to azimuth angles if instrument doesn't point to North. |
