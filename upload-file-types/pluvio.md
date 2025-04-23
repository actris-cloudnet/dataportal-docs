[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Pluvio² rain gauge.

OTT Pluvio² rain gauge.

Instrument ID: `pluvio`

## Recommended files

| File                 | Format  | Description                 |
| -------------------- | ------- | --------------------------- |
| `*.txt`, `*.nc` etc. | unknown | No strict format specified. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "juelich"
instrument     = "pluvio"
instrument_pid = "https://hdl.handle.net/21.12132/3.49ca09deca9a4e3e"
path_fmt       = "/data/pluvio/%Y%m%d/%Y%m%d_*.nc"
```
