[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# WIBS-5/NEO bioaerosol sensor

Droplet Measurement Technologies (DMT) Wideband Integrated Bioaerosol Sensor (WIBS-5/NEO)

Instrument ID: `wibs-5`

## Recommended files

| File   | Format | Description                    |
| ------ | ------ | ------------------------------ |
| `*.h5` | HDF5   | Particle by particle data file |

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "villum"
instrument     = "wibs-5"
instrument_pid = "https://hdl.handle.net/21.12132/3.43291806c31c4a99"
path_fmt       = "/data/wibs/%Y%m%d*.h5"
```
