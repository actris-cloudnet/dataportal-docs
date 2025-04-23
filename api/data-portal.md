[Docs home](https://docs.cloudnet.fmi.fi)

# Data portal API reference

This is the documentation for the API v2 provided by the Cloudnet data portal.

## General

We provide an HTTP REST API for programmatic access to the metadata at the Cloudnet data portal.
The API responds with JSON and can be accessed via the base URL `https://cloudnet.fmi.fi/api/`.
Metadata can be queried via the following routes. For examples on how to use the API programmatically, see [Examples](#examples).

## Index

1. [Routes](#routes)
   1. [Sites](#get-apisites--site)
   1. [Products](#get-apiproducts--product)
   1. [Product variables](#get-apiproductsvariables--productvariables)
   1. [Instruments](#get-apiinstruments--instrument)
   1. [Models](#get-apimodels--model)
   1. [File by UUID](#get-apifilesuuid--file)
   1. [Product files](#get-apifiles--file)
   1. [Model files](#get-apimodel-files--modelfile)
   1. [Visualization by file UUID](#get-apivisualizationsuuid--visualization)
   1. [Visualizations](#get-apivisualizations--visualization)
   1. [Raw files](#get-apiraw-files--upload)
   1. [Raw model files](#get-apiraw-model-files--modelupload)
1. [Examples](#examples)
1. [Notes](#notes)
1. [Errors](#errors)

## Routes

### `GET /api/sites` → `Site[]`

Fetch information on all the measuring stations in the system. Responds with a list of `Site` objects,
each having the properties:

- `id`: Unique site identifier.
- `humanReadableName`: Name of the site in a human-readable format.
- `stationName`: Name or abbreviation of the measurement station at the site. `null` if the site does not have one.
- `description`: Site description in HTML format.
- `type`: List of site type identifiers. Possible values are:
  - `cloudnet`: Cloudnet sites.
  - `campaign`: Short-term measurement sites.
  - `arm`: Atmospheric Radiation Measurement (ARM) site.
  - `model`: Model only sites not visible in the search GUI.
  - `hidden`: Sites that are not visible in the search GUI.
- `latitude`: Latitude of the site given with the precision of at least three decimals. `null` if the site is a moving site (e.g. ship).
- `longitude`: Longitude of the site given with the precision of at least three decimals. `null` if the site is a moving site.
- `altitude`: Elevation of the site from mean sea level in meters.
- `gaw`: Global Atmosphere Watch identifier. `null` if the site does not have one.
- `dvasId`: DVAS data portal station identifier. `null` if site is not listed in the DVAS portal.
- `actrisId`: ACTRIS national facility identifier. `null` if site is not an ACTRIS national facility.
- `country`: Human-readable name for the country or subdivision in which the site resides.
- `countryCode`: Two-letter country code according to [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). `null` if the code is not set or available.
- `countrySubdivisionCode`: Country subdivision code according to [ISO 3166-2](https://en.wikipedia.org/wiki/ISO_3166-2). `null` if the code is not set or available.

Example query:

`GET https://cloudnet.fmi.fi/api/sites`

Response body:

```json
[
  {
    "id": "alomar",
    "humanReadableName": "Alomar",
    "stationName": null,
    "description": null,
    "type": [
      "hidden"
    ],
    "latitude": 62.278,
    "longitude": 16.008,
    "altitude": 380,
    "gaw": null,
    "dvasId": null,
    "actrisId": null,
    "country": "Norway",
    "countryCode": "NO",
    "countrySubdivisionCode": null
  },
  ...
]
```

### `GET /api/products` → `Product[]`

Fetch information on the products served in the data portal. Responds with a list of `Product` objects,
each having the properties:

- `id`: Unique identifier of the product.
- `humanReadableName`: Name of the product in a human-readable format.
- `type`: List of product types. Possible values are:
  - `instrument`: Instrument product.
  - `geophysical`: Geophysical product.
  - `evaluation`: Higher-level evaluation product (e.g. level 3).
- `experimental`: `true` on experimental products, `false` on normal ones.

Example query:

`GET https://cloudnet.fmi.fi/api/products`

Response body:

```json
[
  {
    "id": "categorize",
    "humanReadableName": "Categorize",
    "type": [
      "geophysical"
    ],
    "experimental": "false"
  },
  ...
]
```

### `GET /api/products/variables` → `ProductVariables[]`

Fetch product variables. Responds with a list of `ProductVariable` objects,
each having the properties:

- `id`: Unique identifier of the product.
- `humanReadableName`: Name of the product in a human-readable format.
- `type`: List of product types (see `Product[]` above).
- `experimental`: `true` on experimental products, `false` on normal ones.
- `variables`: List of product variables, each having the following properties:
  - `id`: Unique identifier of the variable.
  - `humanReadableName`: Variable name in a human-readable format.
  - `order`: Number indicating the relevance of the variable for the product in question. `0` is the most relevant variable for the product. Used for sorting plots.
  - `actrisName`: Variable name in ACTRIS Vocabulary.

**NOTE:** The list of returned variables is not exhaustive, the products may contain more variables than listed. Internally this list is used for plotting.

Example query:

`GET https://cloudnet.fmi.fi/api/products/variables`

Response body:

```json
[
  {
    "id": "disdrometer",
    "humanReadableName": "Disdrometer",
    "type": [
      "instrument"
    ],
    "experimental": false,
    "variables": [
      {
        "id": "disdrometer-rainfall_rate",
        "humanReadableName": "Rainfall rate",
        "order": "0"
      },
      {
        "id": "disdrometer-n_particles",
        "humanReadableName": "Number of particles",
        "order": "1"
      }
    ]
  },
  ...
]
```

### `GET /api/instruments` → `Instrument[]`

Fetch information on the instruments supported by the data portal. Responds with a list of `Instrument` objects,
each having the properties:

- `id`: Unique identifier of the instrument.
- `type`: Type of the instrument. For example, `radar`, `lidar`, or `mwr`.
- `humanReadableName`: Human-readable name for the instrument.
- `shortName`: Human-readable short name for the instrument.
- `allowedTags`: Allowed tags for this instrument. For example, `co`, `cross`.

Example query:

`GET https://cloudnet.fmi.fi/api/instruments`

Response body:

```json
[
  {
    "id": "mira",
    "type": "radar",
    "humanReadableName": "METEK MIRA-35 cloud radar",
    "shortName": "MIRA-10 cloud radar",
    "allowedTags": []
  },
  ...
]
```

### `GET /api/models` → `Model[]`

Fetch information on the different model file types served in the data portal. Responds with a list of `Model` objects,
each having the properties:

- `id`: The unique identifier of the model.
- `humanReadableName`: Human-readable name for the model.
- `optimumOrder`: Integer that signifies model quality. Better models have lower `optimumOrder`.
- `sourceModelId`: Source model identifier.
- `forecastStart`: Start hour of the forecast.
- `forecastEnd`: End hour of the forecast.

Example query:

`GET https://cloudnet.fmi.fi/api/models`

Response body:

```json
[
  {
    "id": "ecmwf",
    "humanReadableName":"ECMWF IFS forecast",
    "optimumOrder": 0,
    "sourceModelId": null,
    "forecastStart": null,
    "forecastEnd": null,
  },
  ...
]
```

### `GET /api/files/UUID` → `File`

Fetch metadata for a single data object using its UUID (Universal Unique Identifier).
All the data objects downloaded from the data portal are associated with a UUID,
which can be found in the data object's global attribute `file_uuid`.
To view the global attributes of a NetCDF file, one may use `ncdump -h file.nc`.

On a successful query the route responds with a `File` object, which has the following properties:

- `uuid`: UUIDv4 identifier.
- `version`: String identifying file version. Empty string on volatile files.
- `pid`: The persistent identifier of the data object. Empty string for data objects that do not have a PID, such as volatile files.
- `dvasId`: DVAS data portal identifier.
- `volatile`: `true` if the file has been modified recently and may change in the future,
- `tombstoneReason`: Reason for tombstoning the file. `null` for valid files.
- `legacy`: `true` if the file is legacy data, `false` if it has been processed with CloudnetPy.
  `false` if the file has not been changed recently and will not change in the future.
- `measurementDate`: The date on which the data was measured, `YYYY-MM-DD`.
- `history`: A freeform history of the file set by the creator/processor of the file. This field
  is not curated in any way and may or may not contain helpful information.
- `checksum`: The SHA-256 checksum of the file. Useful for verifying file integrity.
- `size`: Size of the file in bytes.
- `coverage`: Float between 0-1 indicating the data coverage.
- `format`: The data structure of the file. Either `NetCDF3` or `HDF5 (NetCDF4)`.
- `errorLevel`: Quality control outcome for the file. Possible values are `pass`, `info`, `warning` and `error`.
- `createdAt`: The date and time on which the file was created in ISO 8601 format.
- `updatedAt`: The date and time on which the file was last updated in ISO 8601 format.
- `dvasUpdatedAt`: The date and time on which the metadata was submitted to DVAS data portal in ISO 8601 format.
- `startTime`: Start time of the measurement in ISO 8601 format.
- `stopTime`: Stop time of the measurement in ISO 8601 format.
- `software`: List of software used to produce the file.
- `site`: `Site` object containing information of the site on which the measurement was made.
- `product`: `Product` object containing information of the data product.
- `software`: `Software` object containing information of used processing software.
- `instrument`: `Instrument` object containing information of instrument used to measure the data.
- `downloadUrl`: The full URL to the data object. Useful for downloading the file.
- `filename`: The name of the file.
- `timeliness`: [Timeliness](https://vocabulary.actris.nilu.no/actris_vocab/timeliness) of the file. Possible values are
  `rrt` ([real real-time](https://vocabulary.actris.nilu.no/actris_vocab/realreal-time)),
  `nrt` ([near real-time](https://vocabulary.actris.nilu.no/actris_vocab/nearreal-time)) and
  [`scheduled`](https://vocabulary.actris.nilu.no/actris_vocab/scheduled).
- `sourceFileIds`: List of UUIDs corresponding to the source files that were used to generate the file. If the source file information is not available, this is `[]`.

Example query:

`GET https://cloudnet.fmi.fi/api/files/f695d0e6-a58a-40d1-affe-a18d2ff044fd`

Response body:

```json
{
  "uuid": "f695d0e6-a58a-40d1-affe-a18d2ff044fd",
  "version": "",
  "pid": "https://hdl.handle.net/21.12132/1.f695d0e6a58a40d1",
  "dvasId": null,
  "volatile": true,
  "tombstoneReason": null,
  "legacy": false,
  "measurementDate": "2025-04-23",
  "checksum": "76f9d28a428e349db700dcdedd93a524c464195ba6e6b59e515efebf09bce02d",
  "size": "9545135",
  "coverage": 0.771777,
  "format": "HDF5 (NetCDF4)",
  "errorLevel": "info",
  "createdAt": "2025-04-23T01:21:05.694Z",
  "updatedAt": "2025-04-23T14:21:07.303Z",
  "dvasUpdatedAt": null,
  "startTime": "2025-04-23T00:00:10.000Z",
  "stopTime": "2025-04-23T13:47:02.000Z",
  "site": {
    "id": "bucharest",
    "humanReadableName": "Bucharest",
    "stationName": "RADO-Bucharest",
    "type": ["cloudnet"],
    "latitude": 44.344,
    "longitude": 26.012,
    "altitude": 77,
    "gaw": "INO",
    "dvasId": "lbok",
    "actrisId": 99,
    "country": "Romania",
    "countryCode": "RO",
    "countrySubdivisionCode": null
  },
  "product": {
    "id": "radar",
    "humanReadableName": "Radar",
    "type": ["instrument"],
    "experimental": false
  },
  "software": [
    {
      "id": "cloudnet-processing",
      "version": "2.54.8",
      "title": "Cloudnet processing 2.54.8",
      "url": "https://github.com/actris-cloudnet/cloudnet-processing/tree/v2.54.8"
    },
    {
      "id": "cloudnetpy",
      "version": "1.74.3",
      "title": "CloudnetPy 1.74.3",
      "url": "https://doi.org/10.5281/zenodo.15261153"
    }
  ],
  "instrument": {
    "uuid": "d98f6fd2-bec9-4e5e-b1d3-5ca422529215",
    "pid": "https://hdl.handle.net/21.12132/3.d98f6fd2bec94e5e",
    "name": "INOE MIRA-35S",
    "owners": [
      "National Institute of Research and Development for Optoelectronics (INOE)"
    ],
    "model": "METEK MIRA-35S",
    "type": "Doppler scanning cloud radar",
    "serialNumber": null
  },
  "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/f695d0e6-a58a-40d1-affe-a18d2ff044fd/20250423_bucharest_mira-35_d98f6fd2.nc",
  "filename": "20250423_bucharest_mira-35_d98f6fd2.nc",
  "timeliness": "rrt",
  "sourceFileIds": []
}
```

### `GET /api/files` → `File[]`

Queries the metadata of multiple product files. On a successful query responds with a list of `File` objects.
The results can be filtered with the following parameters:

- `site`: One or more `Site` ids, from which to fetch file metadata.
- `date`: Only fetch data from a given date. Date format is `YYYY-MM-DD`.
- `dateFrom`: Limit query to files whose `measurementDate` is `dateFrom` or later. Same date format as in `date`.
  By default `measurementDate` is not limited.
- `dateTo`: Limit query to files whose `measurementDate` is `dateTo` or earlier. Same date format as in `date`.
  If omitted will default to the current date.
- `product`: One or more `Product` ids, by which to filter the files. May NOT be `model`; for model files, see route `/api/model-files`.
- `instrument`: Limit query by instrument (e.g. "radar" or "lidar").
- `instrumentPid`: Limit query by instrument PID.
- `filename`: One or more filenames by which to filter the files.
- `allVersions`: By default the API returns only the latest version of the files. Adding this parameter will fetch all existing versions.
- `showLegacy`: By default the API does not return legacy data. Adding this parameter will fetch also legacy data.
- `updatedAtFrom`: Limit query to files whose `updatedAt` is `updatedAtFrom` or later.
  By default `updatedAt` is not limited. Accepts either `YYYY-MM-DD` or `YYYY-MM-DDTHH:MM:SS.SSSZ`.
- `updatedAtTo`: Limit query to files whose `updatedAt` is `updatedAtTo` or earlier.
  If omitted will default to the current date. Accepts same format as `updatedAtFrom`.

Note: one or more of the parameters _must_ be issued. A query without any valid parameters will result in a `400 Bad Request` error.

Example query for fetching metadata for all classification files from Bucharest starting at 24. April 2020 and ending at the current date:

`GET https://cloudnet.fmi.fi/api/files?site=bucharest&dateFrom=2020-04-24&product=classification`

Response body:

```json
[
  {
    "uuid": "689a2b82-74e9-426c-9c44-5da111eeccd7",
    "version": "",
    "pid": "https://hdl.handle.net/21.12132/1.689a2b8274e9426c",
    "dvasId": "68084189e8baa9a87477dc63",
    "volatile": true,
    "tombstoneReason": null,
    "legacy": false,
    "measurementDate": "2025-04-23",
    "checksum": "87c8a09d0adc5085c8d420d76e4efe89122724a24330ad70b238ed88e559f21f",
    "size": "106095",
    "coverage": 0.7452301,
    "format": "HDF5 (NetCDF4)",
    "errorLevel": "info",
    "createdAt": "2025-04-23T01:25:26.970Z",
    "updatedAt": "2025-04-23T14:51:17.140Z",
    "dvasUpdatedAt": "2025-04-23T14:51:19.799Z",
    "startTime": "2025-04-23T00:00:15.000Z",
    "stopTime": "2025-04-23T13:47:15.000Z",
    "site": {
      "id": "bucharest",
      "humanReadableName": "Bucharest",
      "stationName": "RADO-Bucharest",
      "type": ["cloudnet"],
      "latitude": 44.344,
      "longitude": 26.012,
      "altitude": 77,
      "gaw": "INO",
      "dvasId": "lbok",
      "actrisId": 99,
      "country": "Romania",
      "countryCode": "RO",
      "countrySubdivisionCode": null
    },
    "product": {
      "id": "classification",
      "humanReadableName": "Classification",
      "type": ["geophysical"],
      "experimental": false
    },
    "instrument": null,
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/689a2b82-74e9-426c-9c44-5da111eeccd7/20250423_bucharest_classification.nc",
    "filename": "20250423_bucharest_classification.nc",
    "timeliness": "rrt"
  }
  ...
]
```

### `GET /api/model-files` → `ModelFile[]`

Queries the metadata of model files. It offers the following parameters for filtering the results:

- `site`: One or more `Site` ids, from which to fetch file metadata.
- `date`: Only fetch data from a given date. Date format is `YYYY-MM-DD`.
- `dateFrom`: Limit query to files whose `measurementDate` is `dateFrom` or later. Same date format as in `date`.
  By default `measurementDate` is not limited.
- `dateTo`: Limit query to files whose `measurementDate` is `dateTo` or earlier. Same date format as in `date`.
  If omitted will default to the current date.
- `filename`: One or more filenames by which to filter the files.
- `model`: One or more `Model` ids, by which to filter the files.
- `allModels`: By default the API returns only the best model available matching the given search parameters. Adding this parameter will fetch all model files matching the search parameters.
- `updatedAtFrom`: Limit query to files whose `updatedAt` is `updatedAtFrom` or later.
  By default `updatedAt` is not limited. Accepts either `YYYY-MM-DD` or `YYYY-MM-DDTHH:MM:SS.SSSZ`.
- `updatedAtTo`: Limit query to files whose `updatedAt` is `updatedAtTo` or earlier.
  If omitted will default to the current date. Accepts same format as `updatedAtFrom`.

The `ModelFile` response similar to the `File` response, with an additional `model` property containing a `Model` object (see example response).
Furthermore, the `ModelFile` response omits the fields `instrument`, `software` and `sourceFileIds`.

Example query for fetching metadata for `gdas1` model from Lindenberg on 2. March 2021:

`GET https://cloudnet.fmi.fi/api/model-files?site=lindenberg&date=2021-02-03&model=gdas1`

Response body:

```json
[
  {
    "uuid": "90f95f81-4f45-4efb-a25d-80066652aece",
    "version": "TlV0KVxoVwpWBXdfJkQROoGTk9bs9f.",
    "pid": "https://hdl.handle.net/21.12132/1.90f95f814f454efb",
    "dvasId": null,
    "volatile": false,
    "tombstoneReason": null,
    "legacy": false,
    "measurementDate": "2021-02-03",
    "checksum": "ec7507be604efbec159c4c70ca815890d33bed190ffc15bccdb403e43a13b579",
    "size": "206451",
    "coverage": null,
    "format": "HDF5 (NetCDF4)",
    "errorLevel": null,
    "createdAt": "2021-02-04T10:10:28.001Z",
    "updatedAt": "2021-02-08T21:24:14.883Z",
    "dvasUpdatedAt": null,
    "startTime": null,
    "stopTime": null,
    "site": {
      "id": "lindenberg",
      "humanReadableName": "Lindenberg",
      "stationName": "MOL-RAO",
      "type": ["cloudnet"],
      "latitude": 52.208,
      "longitude": 14.118,
      "altitude": 104,
      "gaw": "LIN",
      "dvasId": "6zm2",
      "actrisId": 49,
      "country": "Germany",
      "countryCode": "DE",
      "countrySubdivisionCode": null
    },
    "product": {
      "id": "model",
      "humanReadableName": "Model",
      "type": ["model"],
      "experimental": false
    },
    "model": {
      "id": "gdas1",
      "humanReadableName": "Global Data Assimilation System (GDAS1)",
      "optimumOrder": 400,
      "sourceModelId": null,
      "forecastStart": null,
      "forecastEnd": null
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/90f95f81-4f45-4efb-a25d-80066652aece/20210203_lindenberg_gdas1.nc",
    "filename": "20210203_lindenberg_gdas1.nc",
    "timeliness": "scheduled"
  }
]
```

### `GET /api/visualizations/UUID` → `Visualization`

Fetch visualization metadata for a single data object using its UUID.
On a successful query the route responds with a `Visualization` object, which has the following properties:

- `sourceFileId`: UUID of the file for of which the visualizations were generated.
- `productHumanReadable`: Human readable name of the product.
- `locationHumanReadable`: Human readable name of the site.
- `volatile`: `true` if the file is volatile, otherwise `false`.
- `legacy`: `true` if the file is a legacy file, otherwise `false`.
- `visualizations`: List of visualization metadata, each entry having the following properties:
  - `s3key`: Filename of the visualization.
  - `productVariable`: Product variable object, see [Product variables](#get-apiproductsvariables--productvariables).
  - `dimensions`: `null` or an object with the following properties:
    - `width`: width of the image in pixels.
    - `height`: height of the image in pixels.
    - `marginTop`: space between top axis and image edge in pixels.
    - `marginRight`: space between right axis and image edge in pixels.
    - `marginBottom`: space between bottom axis and image edge in pixels.
    - `marginLeft`: space between left axis and image edge in pixels.

Example query:

`GET https://cloudnet.fmi.fi/api/visualizations/cfda5129-0a51-45d4-b674-ccf6c7f1a447`

Response body:

```json
{
  "sourceFileId": "cfda5129-0a51-45d4-b674-ccf6c7f1a447",
  "visualizations": [
    {
      "s3key": "20211025_hyytiala_cl61d-cfda5129-beta.png",
      "productVariable": {
        "id": "lidar-beta",
        "humanReadableName": "Attenuated backscatter coefficient",
        "order": "0"
      },
      "dimensions": null
    },
    {
      "s3key": "20211025_hyytiala_cl61d-cfda5129-depolarisation.png",
      "productVariable": {
        "id": "lidar-depolarisation",
        "humanReadableName": "Lidar depolarisation",
        "order": "2"
      },
      "dimensions": null
    }
  ],
  "productHumanReadable": "Lidar",
  "locationHumanReadable": "Hyytiälä",
  "volatile": true,
  "legacy": false
}
```

### `GET /api/visualizations` → `Visualization[]`

Queries the metadata of visualizations. It offers the same parameters as the [Product files](#get-apifiles--file) -route for filtering the results, with an additional parameter `variable` for fetching visualizations for a specific `ProductVariable`.
The route responds with a list of `Visualization` objects.

Example request:

`GET https://cloudnet.fmi.fi/api/visualizations/?date=2021-10-25&site=mindelo`

Response body:

```json
[
  {
    "sourceFileId": "cbfa399d-97ee-41e8-a80a-23f8f1661817",
    "visualizations": [
      {
        "s3key": "20211025_mindelo_pollyxt-cbfa399d-beta.png",
        "productVariable": {
          "id": "lidar-beta",
          "humanReadableName": "Attenuated backscatter coefficient",
          "order": 0,
          "actrisName": "atmosphere calibrated attenuated backscattered lidar signal"
        },
        "dimensions": {
          "width": 1453,
          "height": 458,
          "marginTop": 19,
          "marginRight": 136,
          "marginBottom": 68,
          "marginLeft": 72
        }
      },
      {
        "s3key": "20211025_mindelo_pollyxt-cbfa399d-beta_raw.png",
        "productVariable": {
          "id": "lidar-beta_raw",
          "humanReadableName": "Non-screened attenuated backscatter coefficient",
          "order": 1,
          "actrisName": null
        },
        "dimensions": {
          "width": 1453,
          "height": 458,
          "marginTop": 19,
          "marginRight": 136,
          "marginBottom": 68,
          "marginLeft": 72
        }
      },
      {
        "s3key": "20211025_mindelo_pollyxt-cbfa399d-depolarisation.png",
        "productVariable": {
          "id": "lidar-depolarisation",
          "humanReadableName": "Uncalibrated lidar volume linear depolarisation ratio",
          "order": 2,
          "actrisName": null
        },
        "dimensions": {
          "width": 1421,
          "height": 458,
          "marginTop": 19,
          "marginRight": 104,
          "marginBottom": 68,
          "marginLeft": 72
        }
      },
      {
        "s3key": "20211025_mindelo_pollyxt-cbfa399d-depolarisation_raw.png",
        "productVariable": {
          "id": "lidar-depolarisation_raw",
          "humanReadableName": "Non-screened uncalibrated lidar volume linear depolarisation ratio",
          "order": 3,
          "actrisName": null
        },
        "dimensions": {
          "width": 1421,
          "height": 458,
          "marginTop": 19,
          "marginRight": 104,
          "marginBottom": 68,
          "marginLeft": 72
        }
      }
    ],
    "source": {
      "uuid": "3b8d6fb7-e7e2-4f1d-89c9-f81f9de1b1b6",
      "pid": "https://hdl.handle.net/21.12132/3.3b8d6fb7e7e24f1d",
      "name": "TROPOS PollyXT (cpv)",
      "owners": ["Leibniz Institute for Tropospheric Research (TROPOS)"],
      "model": "PollyXT",
      "type": "multiwavelength Raman polarization lidar",
      "serialNumber": "pollyxt_cpv"
    },
    "productHumanReadable": "Lidar",
    "locationHumanReadable": "Mindelo",
    "volatile": false,
    "legacy": false,
    "experimental": false
  },
  ...
]
```

### `GET /api/raw-files` → `Upload[]`

Query the metadata of uploaded instrument files. You can use the following the parameters to filter the search results.

- `site`: `Site` id.
- `status`: Status of the uploaded file. By default returns files with any status. Status may be one of the following:
  - `created`: Metadata has been created but file has not been uploaded.
  - `uploaded`: File has been successfully uploaded, but it has not been processed.
  - `processed`: File has been uploaded and it has been processed.
  - `invalid`: File could not be processed due to an error.
- `date`: Limit query to files whose `measurementDate` is `date`. Date format is `YYYY-MM-DD`.
- `dateFrom`: Limit query to files whose `measurementDate` is `dateFrom` or later.
  By default `measurementDate` is not limited.
- `dateTo`: Limit query to files whose `measurementDate` is `dateTo` or earlier.
  If omitted will default to the current date.
- `instrument`: `Instrument` id.
- `updatedAtFrom`: Limit query to files whose `updatedAt` is `updatedAtFrom` or later.
  By default `updatedAt` is not limited. Accepts either `YYYY-MM-DD` or `YYYY-MM-DDTHH:MM:SS.SSSZ`.
- `updatedAtTo`: Limit query to files whose `updatedAt` is `updatedAtTo` or earlier.
  If omitted will default to the current date. Accepts same format as `updatedAtFrom`.
- `filename`: Limit query to files with `filename`.
- `filenamePrefix`: Limit query to filenames with prefix `filenamePrefix`.
- `filenameSuffix`: Limit query to filenames with suffix `filenameSuffix`.

The response is a list of `Upload` objects. The `Upload` object has the following properties:

- `uuid`: Unique identifier of the file.
- `checksum`: An MD5 checksum of the file.
- `filename`: Original name of the file.
- `measurementDate`: The date of the measurements contained in the file.
- `size`: File size in bytes.
- `status`: Status of the file. See above for possible statuses.
- `createdAt`: Date of file metadata creation.
- `updatedAt`: Date of last file and/or metadata update.
- `instrumentPid`: Persistent identifier (PID) for the instrument.
- `tags`: List of tags.
- `siteId`: Site identifier.
- `instrumentId`: Instrument identifier.
- `instrumentInfoUuid`: Reference to `InstrumentInfo` object.
- `site`: `Site` object.
- `instrument`: `Instrument` object.
- `instrumentInfo`: `InstrumentInfo` object.
- `downloadUrl`: A URL for direct download of the file.

Example query:

`GET https://cloudnet.fmi.fi/api/raw-files?site=norunda&date=2021-09-01`

Response body:

```json
[
  {
    "uuid": "dccb71c2-9671-4f23-a2d8-31193c04c533",
    "checksum": "ddb021374eddf36db15d09f0df3a332c",
    "filename": "210901_120002_P09_ZEN.LV0",
    "s3key": "norunda/dccb71c2-9671-4f23-a2d8-31193c04c533/210901_120002_P09_ZEN.LV0",
    "measurementDate": "2021-09-01",
    "size": "139723302",
    "status": "uploaded",
    "createdAt": "2021-09-01T16:58:22.436Z",
    "updatedAt": "2021-09-01T16:58:26.359Z",
    "instrumentPid": "https://hdl.handle.net/21.12132/3.bf6c0c3926c54dbb",
    "tags": [],
    "siteId": "norunda",
    "instrumentId": "rpg-fmcw-94",
    "instrumentInfoUuid": "bf6c0c39-26c5-4dbb-bd0a-054d5382989e",
    "site": {
      "id": "norunda",
      "humanReadableName": "Norunda",
      "stationName": null,
      "description": null,
      "type": ["cloudnet"],
      "latitude": 60.086,
      "longitude": 17.479,
      "altitude": 46,
      "gaw": "NOR",
      "dvasId": "d83u",
      "actrisId": null,
      "country": "Sweden",
      "countryCode": "SE",
      "countrySubdivisionCode": null
    },
    "instrument": {
      "id": "rpg-fmcw-94",
      "type": "radar",
      "humanReadableName": "RPG-Radiometer Physics RPG-FMCW-94 cloud radar",
      "shortName": "RPG-FMCW-94 cloud radar",
      "allowedTags": []
    },
    "instrumentInfo": {
      "uuid": "bf6c0c39-26c5-4dbb-bd0a-054d5382989e",
      "pid": "https://hdl.handle.net/21.12132/3.bf6c0c3926c54dbb",
      "name": "ESA RPG-FMCW-94-DP",
      "owners": ["European Space Agency (ESA)"],
      "model": "RPG-FMCW-94-DP",
      "type": "Doppler non-scanning cloud radar",
      "serialNumber": null,
      "instrumentId": "rpg-fmcw-94"
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/raw/dccb71c2-9671-4f23-a2d8-31193c04c533/210901_120002_P09_ZEN.LV0"
  },
  ...
]
```

### `GET /api/raw-model-files` → `ModelUpload[]`

Query the metadata of uploaded model files. Query parameters are as above, with the following exception:
Instead of the `instrument` parameter, it is possible to filter the results with the `model` parameter, which takes `Model` id.

Example query:

`GET https://cloudnet.fmi.fi/api/raw-model-files?site=norunda&date=2021-09-01`

Response body:

```json
[
  {
    "uuid": "71b69dbb-69f6-4b9e-839e-8427e055e3e9",
    "checksum": "089a71af295fa3b5f32d217d402113b4",
    "filename": "20210901_norunda_ecmwf.nc",
    "s3key": "norunda/71b69dbb-69f6-4b9e-839e-8427e055e3e9/20210901_norunda_ecmwf.nc",
    "measurementDate": "2021-09-01",
    "size": "501500",
    "status": "processed",
    "createdAt": "2021-09-01T21:36:52.837Z",
    "updatedAt": "2022-04-27T12:53:38.715Z",
    "siteId": "norunda",
    "modelId": "ecmwf",
    "site": {
      "id": "norunda",
      "humanReadableName": "Norunda",
      "stationName": null,
      "description": null,
      "type": ["cloudnet"],
      "latitude": 60.086,
      "longitude": 17.479,
      "altitude": 46,
      "gaw": "NOR",
      "dvasId": "d83u",
      "actrisId": null,
      "country": "Sweden",
      "countryCode": "SE",
      "countrySubdivisionCode": null
    },
    "model": {
      "id": "ecmwf",
      "humanReadableName": "ECMWF IFS forecast",
      "optimumOrder": 0,
      "sourceModelId": null,
      "forecastStart": null,
      "forecastEnd": null
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/raw/71b69dbb-69f6-4b9e-839e-8427e055e3e9/20210901_norunda_ecmwf.nc"
  },
  ...
]
```

## Examples

The following examples use the `curl` and `jq` applications. They can be installed from the package
repositories of most UNIX-based systems.

### Multiple sites and products as parameters

Fetch all categorize and classification products from Norunda and Granada:

```shell
curl "https://cloudnet.fmi.fi/api/files?site=norunda&site=granada&product=categorize&product=classification"
```

### Fetching all versions

Fetch all versions of the classification product from Granada on 2020-05-20:

```shell
curl "https://cloudnet.fmi.fi/api/files?site=granada&product=classification&date=2020-05-20&allVersions"
```

### Model data

Fetch the optimum model file from Bucharest on 2020-12-10:

```shell
curl "https://cloudnet.fmi.fi/api/model-files?site=bucharest&date=2020-12-10"
```

Fetch all available models:

```shell
curl "https://cloudnet.fmi.fi/api/model-files?site=bucharest&date=2020-12-10&allModels"
```

Fetch only the `gdas1` model:

```shell
curl "https://cloudnet.fmi.fi/api/model-files?site=bucharest&date=2020-12-10&model=gdas1"
```

### Using the API to download all data objects matching a criteria

Download all data measured on 1. October 2020, saving them to the current working directory:

```shell
curl "https://cloudnet.fmi.fi/api/files?date=2020-10-01" | jq '.[]["downloadUrl"]' | xargs -n1 curl -O
```

That is, get the filtered list of file metadata, pick the `downloadUrl` properties from each of the `File` objects
and pass them on to `curl` again to download.

### Python example

The easiest way to fetch metadata and data from the Cloudnet API is to use the [official Python client](https://github.com/actris-cloudnet/cloudnet-api-client).

Installation:

```bash
python3 -m pip install cloudnet-api-client
```

Download one day of classification data from all sites:

```python
from cloudnet_api_client import APIClient

client = APIClient()

sites = client.sites()
site_ids = [site.id for site in sites]

metadata = client.metadata(site_ids, "2025-01-01", product="classification")
client.download(metadata, "data/")
```

## Notes

The API provides `gzip` compression of responses where applicable.
We recommend using the compression for large queries to reduce transmitted response size.
The compression can be enabled effortlessly in `curl` with the `--compressed` switch.

## Errors

The API responds to errors with the appropriate HTTP status code and an `Error` object, which has the properties:

- `status`: HTTP error code.
- `message`: Human readable error message as a string or list of strings.

Example query:

`GET https://cloudnet.fmi.fi/api/files?product=sausage`

Response body:

```json
{
  "status": 404,
  "errors": ["One or more of the specified products were not found"]
}
```
