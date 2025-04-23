[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# RPG cloud radars

RPG-Radiometer Physics FMCW-35 or FMCW-94 cloud radars

Instrument ID: `rpg-fmcw-35` or `rpg-fmcw-94`

## Recommended files

| File    | Format | Description                                 |
| ------- | ------ | ------------------------------------------- |
| `*.LV0` | binary | Raw data with Doppler spectra.              |
| `*.LV1` | binary | Pre-processed data with calculated moments. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "rpg-fmcw-94"
instrument_pid = "https://hdl.handle.net/21.12132/3.191564170f8a4686"
path_fmt       = "/data/rpg/%y%m%d_*_ZEN.{LV0,LV1}"
```
