[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# Thies precipitation transmitter

Thies precipitation transmitter rain gauge.

Instrument ID: `thies-precipitation-transmitter`

## Recommended files

| File                 | Format  | Description                 |
| -------------------- | ------- | --------------------------- |
| `*.txt`, `*.nc` etc. | unknown | No strict format specified. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "mindelo"
instrument     = "thies-precipitation-transmitter"
instrument_pid = "https://hdl.handle.net/21.12132/3.057664d9f5cd46cb"
path_fmt       = "/data/thies/%Y%m%d/%Y%m%d_*.nc"
```
