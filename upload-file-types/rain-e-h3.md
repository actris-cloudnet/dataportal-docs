[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# rain[e]H3 rain gauge

LAMBRECHT rain[e]H3 rain gauge.

Instrument ID: `rain-e-h3`

## Recommended files

| File                 | Format  | Description                 |
| -------------------- | ------- | --------------------------- |
| `*.txt`, `*.nc` etc. | unknown | No strict format specified. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "lindenberg"
instrument     = "rain-e-h3"
instrument_pid = "https://hdl.handle.net/21.12132/3.3292ee29e461405a"
path_fmt       = "/data/raine/%Y%m%d/%Y%m%d_*.csv"
```
