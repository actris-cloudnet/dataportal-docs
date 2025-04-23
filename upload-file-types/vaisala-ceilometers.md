[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Vaisala ceilometers

Various older Vaisala ceilometers. See also [Vaisala CL61](cl61d.html).

Instrument ID: `ld40`, `ct25k`, `cl31` or `cl51`

## Recommended files

| File    | Format | Description                                                     |
| ------- | ------ | --------------------------------------------------------------- |
| `*.DAT` | text   | File extension may be different depending on collection system. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "chilbolton"
instrument     = "cl51"
instrument_pid = "https://hdl.handle.net/21.12132/3.1ca2b7d6323f486b"
path_fmt       = "/data/ceilo/%Y%m%d/*%m%d*.DAT"
```

## Calibration

| Field Name           | Type    | Unit | Description                                                                                                                |
| -------------------- | ------- | ---- | -------------------------------------------------------------------------------------------------------------------------- |
| `calibration_factor` | `float` | 1    | Calibration factor.                                                                                                        |
| `range_corrected`    | `bool`  |      | False if noise is not range corrected (i.e. `noise_h2 off` mode). For new measurements, `noise_h2 on` mode should be used. |
| `time_offset`        | `int`   | min  | Time offset compared to UTC (positive values mean ahead of UTC, e.g. 120 for Finnish standard time).                       |
