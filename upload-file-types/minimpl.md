[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# DMT MiniMPL lidar

Droplet Measurement Technologies (DMT) Mini Micro Pulse LiDAR (MiniMPL).

Instrument ID: `minimpl`

## Recommended files

| File    | Format | Description          |
| ------- | ------ | -------------------- |
| `*.mpl` | binary | Hourly measurements. |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "neumayer"
instrument     = "minimpl"
instrument_pid = "https://hdl.handle.net/21.12132/3.f997d051867a43ed"
path_fmt       = "/data/mpl/%Y%m%d/%y%m%d_*.mpl"
```
