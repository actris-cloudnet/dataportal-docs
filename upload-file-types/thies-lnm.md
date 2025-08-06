[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Thies disdrometer

Thies Laser Precipitation Monitor (LPM) disdrometer, also known as Laser-Niederschlags-Monitor (LNM).

Instrument ID: `thies-lnm`

## Recommended files

| File                  | Format | Description                                                                 |
| --------------------- | ------ | --------------------------------------------------------------------------- |
| `*.txt`, `*.csv` etc. | text   | Log file including drop size and fall velocity spectra (data telegram 4/5). |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "palaiseau"
instrument     = "thies-lnm"
instrument_pid = "https://hdl.handle.net/21.12132/3.11d3217867474e22"
path_fmt       = "/data/disdro/%Y%m%d/%Y%m%d*.txt"
```

## Calibration

| Field Name         | Type  | Unit | Description              |
| ------------------ | ----- | ---- | ------------------------ |
| `truncate_columns` | `int` | -    | Custom truncated format. |
