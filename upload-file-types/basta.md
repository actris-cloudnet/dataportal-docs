[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# BASTA cloud radar

BASTA cloud radar

Instrument ID: `basta`

## Recommended files

| File   | Format | Description   |
| ------ | ------ | ------------- |
| `*.nc` | netCDF | Level 1 data. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "palaiseau"
instrument     = "basta"
instrument_pid = "https://hdl.handle.net/21.12132/3.643b7b5b43814e6f"
path_fmt       = "/data/basta/%Y%m%d/*%Y%m%d*.nc"
```
