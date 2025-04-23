[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Radiometrics microwave radiometers

Radiometrics (MP3014, MWP1, etc.) microwave radiometers.

Instrument ID: `radiometrics`

## Recommended files

| File        | Format | Description   |
| ----------- | ------ | ------------- |
| `*_lv2.csv` | text   | Level 2 data. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "lindenberg"
instrument     = "radiometrics"
instrument_pid = "https://hdl.handle.net/21.12132/3.ca8e16977a634793"
path_fmt       = "/data/rdx/%Y%m%d/%Y-%m-%d_*_lv2.csv"
```
