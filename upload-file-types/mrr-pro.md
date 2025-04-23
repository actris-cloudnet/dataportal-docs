[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# MRR-PRO radar

METER Micro Rain Radar MRR-PRO.

Instrument ID: `mrr-pro`

## Recommended files

| File   | Format | Description          |
| ------ | ------ | -------------------- |
| `*.nc` | netCDF | Hourly measurements. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "granada"
instrument     = "mrr-pro"
instrument_pid = "https://hdl.handle.net/21.12132/3.8db7457671ef4052"
path_fmt       = "/data/mrr/%Y%m%d/%Y%m%d_*.nc"
```
