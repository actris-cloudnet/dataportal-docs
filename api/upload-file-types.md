[Docs home](https://docs.cloudnet.fmi.fi)

## Data upload file types

The Cloudnet data submission API does **not** check the type or content of the uploaded files. 
If you accidentally upload some incorrect files, or files that we can't process, 
we still archive those but perhaps do nothing more. Clearly incorrect files might 
get deleted.

We recommend uploading the following files:

| ID                              | Instrument                                               | File extension / description                                                             | Format    |
|---------------------------------|----------------------------------------------------------|------------------------------------------------------------------------------------------|-----------|
| `mira`                          | METEK MIRA-35 cloud radar                                | Compressed `.mmclx.gz` files.                                                            | netCDF    |
| `rpg-fmcw-94`, `rpg-fmcw-35`    | RPG cloud radars                                         | `.LV1` and compressed `.LV0` files.                                                      | binary    |
| `ld40`, `ct25k`, `cl31`, `cl51` | Vaisala ceilometers                                      | `.DAT` files. File extension may be different depending on collection system.            | text      |
| `cl61d`                         | Vaisala CL61-D ceilometer                                | Non-concatenated `.nc` files.                                                            | netCDF    |
| `chm15k`, `chm15kx`             | Lufft ceilometers                                        | `.nc` files. Either non-concatenated or concatenated files but not both.                 | netCDF    |
| `hatpro`                        | RPG HATPRO microwave radiometer                          | `.LWP`, `.IWV`, `.HKD`, `.BRT`, `.MET`, `.BLB`/`.BLS`, `.IRT`, and similar binary files. | binary    |
| `radiometrics`                  | Radiometrics (MP3014, MWP1, etc.) microwave radiometer   | `.csv` files.                                                                            | CSV       |
| `dwd-mwr`                       | MWR dual-wavelength microwave radiometer from Lindenberg | `unknown`                                                                                | CSV       |
| `copernicus`                    | Copernicus cloud radar                                   | `.nc` files.                                                                             | netCDF    |
| `galileo`                       | Galileo cloud radar                                      | `.nc` files.                                                                             | netCDF    |
| `basta`                         | BASTA cloud radar                                        | Daily `.nc` files.                                                                       | netCDF    |
| `rasta`                         | RASTA cloud radar                                        | Daily `.nc` files.                                                                       | netCDF    |
| `parsivel`                      | OTT Parsivel2 disdrometer                                | `.log` files.                                                                            | text      |
| `thies-lnm`                     | Thies LNM disdrometer                                    | `.txt` files.                                                                            | text      |
| `halo-doppler-lidar`            | Halo Photonics Doppler lidar                             | `.hpl`, `Background*.txt` and `system_parameters*.txt` files.                            | text      |
| `pollyxt`                       | PollyXT Raman lidar                                      | `*att_bsc.nc` and `*vol_depol.nc` files.                                                 | netCDF    |
| `wls100s`, `wls200s`, `wls400s` | Leosphere windcube long-range scanning Doppler lidars    | `.nc` files.                                                                             | netCDF    |
| `weather-station`               | Weather station data (temperature, humidity, etc.)       | `unknown`                                                                                | `unknown` |

We plan to also accept the following instrument types in the future. Note that the API will not accept these yet. 
If you have other instruments you would like to include (such as other disdrometers, lidars or ancillary instrumentation), please 
let us know, and we will add them to our to-do list.

| ID                              | Instrument                                            | Possible file extensions                   | Format |
|---------------------------------|-------------------------------------------------------|--------------------------------------------|--------|
| `hsrl`                          | ARM HSRL                                              | `.nc` files produced by ARM / Ed Eloranta. | netCDF |
| `mpl`                           | ARM or MPLnet Micropulse Lidar                        | `.nc` files produced by ARM or similar.    | netCDF |
