[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# HALO Doppler lidar

HALO Photonics StreamLine Doppler lidars.

Instrument ID: `halo-doppler-lidar`

## Recommended files

| File                     | Format | Description                                                                         |
| ------------------------ | ------ | ----------------------------------------------------------------------------------- |
| `*.hpl`                  | text   | Hourly measurements (Stare, VAD, etc.)                                              |
| `Background*.txt`        | text   | Hourly background measurements.                                                     |
| `system_parameters*.txt` | text   | Monthly system parameters. These should be submitted to the first day of the month. |

If you have depolarisation measurements stored in two different directories,
you need to specify tags for `*.hpl` files:

| Tag     | Description                      |
| ------- | -------------------------------- |
| `co`    | Co-polarisation measurements.    |
| `cross` | Cross-polarisation measurements. |

Sometimes `Background*.txt` files are also stored in different directories
alongside the co and cross measurements. In this case, you can specify tags for
`Background*.txt` files.

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "halo-doppler-lidar"
instrument_pid = "https://hdl.handle.net/21.12132/3.421272f219be4f97"
path_fmt       = "/data/dlidar34/%Y/%Y%m/%Y%m%d/*.{hpl,txt}"

[[instrument]]
site           = "hyytiala"
instrument     = "halo-doppler-lidar"
instrument_pid = "https://hdl.handle.net/21.12132/3.421272f219be4f97"
path_fmt       = "/data/dlidar34/%Y/%Y%m/system_parameters_*.txt"
periodicity    = "monthly"
```

For depolarisation measurements:

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "halo-doppler-lidar"
instrument_pid = "https://hdl.handle.net/21.12132/3.421272f219be4f97"
path_fmt       = "/data/dlidar34/%Y/%Y%m/%Y%m%d/*.{hpl,txt}"
tags           = ["co"]

[[instrument]]
site           = "hyytiala"
instrument     = "halo-doppler-lidar"
instrument_pid = "https://hdl.handle.net/21.12132/3.421272f219be4f97"
path_fmt       = "/data/dlidar34/cross/%Y/%Y%m/%Y%m%d/*.{hpl,txt}"
tags           = ["cross"]
```

## Calibration

| Field Name       | Type    | Unit    | Description                                                                                          |
| ---------------- | ------- | ------- | ---------------------------------------------------------------------------------------------------- |
| `time_offset`    | `int`   | min     | Time offset compared to UTC (positive values mean ahead of UTC, e.g. 120 for Finnish standard time). |
| `azimuth_offset` | `float` | degrees | Offset value to be added to azimuth angles if instrument doesn't point to North.                     |
