[Docs home](https://docs.cloudnet.fmi.fi) / [Data upload file types](../api/upload-file-types.md)

# RPG microwave radiometers

Microwave radiometers manufactured by RPG Radiometer Physics GmbH.

Instrument ID: `hatpro`, `lhatpro`, `lhumpro`

## Recommended files

| File            | Format | Description            |
| --------------- | ------ | ---------------------- |
| `*.LWP`         | binary | Liquid water path      |
| `*.IWV`         | binary | Integrated water vapor |
| `*.HKD`         | binary | Housekeeping data      |
| `*.BRT`         | binary | Brightness temperature |
| `*.MET`         | binary | Meteorological data    |
| `*.BLB`/`*.BLS` | binary | Boundary layer scan    |
| `*.IRT`         | binary | Infrared temperature   |

Only upload binary files (e.g. `*.LWP`) and not ASCII (e.g. `*.LWP.ASC`) or netCDF (e.g. `*.LWP.NC`) files.

Additionally, retrieval coefficient `*.ret` files should be sent to [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi). They can be found on the HATPRO PC.

## Example

Example configuration for [`cloudnet-submit`](https://github.com/actris-cloudnet/cloudnet-submit):

```toml
[[instrument]]
site           = "hyytiala"
instrument     = "hatpro"
instrument_pid = "https://hdl.handle.net/21.12132/3.f360a2375f3e4e4f"
path_fmt       = "/data/hatpro/%y%m%d.{LWP,IWV,HKD,BRT,MET,BLB,BLS,IRT}"
```

## Calibration

| Field Name         | Type        | Unit | Description                                                                                                                                                                       |
| ------------------ | ----------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `time_offset`      | `int`       | min  | Time offset compared to UTC (positive values mean ahead of UTC, e.g. 120 for Finnish standard time). Normally this is not used because the instrument should operate in UTC mode. |
| `coefficientLinks` | `list[str]` |      | Retrieval coefficient links.                                                                                                                                                      |
