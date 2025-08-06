[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Parsivel² disdrometer

OTT Parsivel² disdrometer.

Instrument ID: `parsivel`

## Recommended files

| File                  | Format | Description                                                                    |
| --------------------- | ------ | ------------------------------------------------------------------------------ |
| `*.txt`, `*.csv` etc. | text   | Log file including drop size and fall velocity spectra (fields 90, 91 and 93). |
| `*.nc`                | netCDF | Suitable if only option.                                                       |

Many different kind of formats are supported for Parsivel². If you are unsure about the formats, please contact [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi).

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "parsivel"
instrument_pid = "https://hdl.handle.net/21.12132/3.69dddc0004b64b32"
path_fmt       = "/data/disdro/%Y%m%d/%Y%m%d.txt"
```

## Calibration

| Field Name           | Type        | Unit | Description                                  |
| -------------------- | ----------- | ---- | -------------------------------------------- |
| `telegram`           | `list[int]` | -    | Data telegram if file doesn't have a header. |
| `missing_timestamps` | `bool`      | -    | Indicates missing timestamps.                |
