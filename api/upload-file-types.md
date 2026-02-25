[Docs home](https://docs.cloudnet.fmi.fi)

## Data upload file types

The Cloudnet data submission API does **not** check the type or content of the uploaded files.
If you accidentally upload some incorrect files, or files that we can't process,
we still archive those but perhaps do nothing more. Clearly incorrect files might
get deleted.

### Cloud radars

| ID                                                                                                       | Instrument                                   |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [`basta`](../upload-file-types/basta.html)                                                               | BASTA cloud radar                            |
| [`copernicus`](../upload-file-types/copernicus.html)                                                     | Copernicus cloud radar                       |
| [`galileo`](../upload-file-types/galileo.html)                                                           | Galileo cloud radar                          |
| [`mira-10`](../upload-file-types/mira.html)                                                              | METEK MIRA-10 cloud radar                    |
| [`mira-35`](../upload-file-types/mira.html), (deprecated `mira`)                                         | METEK MIRA-35 cloud radar (formerly MIRA-36) |
| [`rpg-fmcw-94`](../upload-file-types/rpg-fmcw.html), [`rpg-fmcw-35`](../upload-file-types/rpg-fmcw.html) | RPG-Radiometer Physics cloud radars          |
| `xpol`                                                                                                   | ProSensing XPOL weather radar                |

### Lidars

| ID                                                                                                                                                                                                                                  | Instrument                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| [`chm15k`](../upload-file-types/chm15k.html), [`chm15kx`](../upload-file-types/chm15k.html)                                                                                                                                         | Lufft ceilometers (formerly Jenoptik)  |
| [`cl61d`](../upload-file-types/cl61d.html)                                                                                                                                                                                          | Vaisala CL61 ceilometer                |
| [`cs135`](../upload-file-types/cs135.html)                                                                                                                                                                                          | Campbell Scientific CS135 ceilometer   |
| [`da10`](../upload-file-types/da10.html)                                                                                                                                                                                            | Vaisala DIAL DA10 Atmospheric Profiler |
| [`ld40`](../upload-file-types/vaisala-ceilometers.html), [`ct25k`](../upload-file-types/vaisala-ceilometers.html), [`cl31`](../upload-file-types/vaisala-ceilometers.html), [`cl51`](../upload-file-types/vaisala-ceilometers.html) | Vaisala ceilometers                    |
| [`pollyxt`](../upload-file-types/pollyxt.html)                                                                                                                                                                                      | PollyXT Raman lidar                    |

### Microwave radiometers

| ID                                                       | Instrument                                             |
| -------------------------------------------------------- | ------------------------------------------------------ |
| [`hatpro`](../upload-file-types/hatpro.html)             | RPG HATPRO microwave radiometer                        |
| [`radiometrics`](../upload-file-types/radiometrics.html) | Radiometrics (MP3014, MWP1, etc.) microwave radiometer |

### Disdrometers

| ID                                                 | Instrument                       |
| -------------------------------------------------- | -------------------------------- |
| [`parsivel`](../upload-file-types/parsivel.html)   | OTT Parsivel² disdrometer        |
| [`thies-lnm`](../upload-file-types/thies-lnm.html) | Thies LPM a.k.a. LNM disdrometer |
| `rd-80`                                            | Distromet RD-80 disdrometer      |

### Doppler lidars

| ID                                                                                                                                                | Instrument                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| [`halo-doppler-lidar`](../upload-file-types/halo-doppler-lidar.html)                                                                              | HALO Photonics StreamLine Doppler lidars                      |
| [`wls100s`](../upload-file-types/windcube.html), [`wls200s`](../upload-file-types/windcube.html), [`wls400s`](../upload-file-types/windcube.html) | Vaisala/Leosphere WindCube long-range scanning Doppler lidars |
| [`wls70`](../upload-file-types/wls70.html)                                                                                                        | Leosphere WLS70 WindCube Doppler lidar                        |

### Rain gauges

| ID                                                                                             | Instrument                                 |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------ |
| [`pluvio`](../upload-file-types/pluvio.html)                                                   | OTT Pluvio² rain gauge                     |
| [`rain-e-h3`](../upload-file-types/rain-e-h3.html)                                             | LAMBRECHT rain[e]H3 rain gauge             |
| [`thies-precipitation-transmitter`](../upload-file-types/thies-precipitation-transmitter.html) | Thies Precipitation Transmitter rain gauge |

### Other

| ID                                                             | Instrument                                         |
| -------------------------------------------------------------- | -------------------------------------------------- |
| [`fd12`](../upload-file-types/fd12.html)                       | Vaisala FD12P present weather sensor               |
| [`minimpl`](../upload-file-types/minimpl.html)                 | Droplet MiniMPL lidar                              |
| [`mrr-pro`](../upload-file-types/mrr-pro.html)                 | METEK MRR-PRO rain radar                           |
| [`weather-station`](../upload-file-types/weather-station.html) | Weather station data (temperature, humidity, etc.) |
| `wibs-5`                                                       | WIBS-5/NEO bioaerosol sensor                       |
