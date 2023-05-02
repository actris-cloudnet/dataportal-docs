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
   1. [File by uuid](#get-apifilesuuid--file)
   1. [Product files](#get-apifiles--file)
   1. [Model files](#get-apimodel-files--modelfile)
   1. [Visualization by file uuid](#get-apivisualizationsuuid--visualization)
   1. [Visualizations](#get-apivisualizations--visualization)
   1. [Raw files](#get-apiraw-files--upload)
   1. [Raw model files](#get-apiraw-model-files--modelupload)
1. [Examples](#examples)
1. [Notes](#notes)
1. [Errors](#errors)

## Routes

### `GET /api/sites` → `Site[]`

Fetch information on all the measuring stations in the system. Responds with an array of `Site` objects,
each having the properties:

- `id`: Unique site identifier.
- `humanReadableName`: Name of the site in a human-readable format.
- `type`: An array of site type identifiers. Types are as follows:
  - `arm`: Atmospheric Radiation Measurement (ARM) site.
  - `campaign`: Temporary measurement sites.
  - `cloudnet`: Official Cloudnet sites.
  - `hidden`: Sites that are not visible in the search GUI.
- `latitude`: Latitude of the site given with the precision of three decimals.
- `longitude`: Longitude of the site given with the precision of three decimals.
- `altitude`: Elevation of the site from mean sea level in meters.
- `gaw`: Global Atmosphere Watch identifier. `null` if the site does not have one.
- `dvasId`: DVAS data portal station identifier. `null` if site is not listed in the DVAS portal.
- `country`: The country or subdivision in which the site resides. Human-readable.
- `countryCode`: Two-letter country code according to [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). `null` if code is not set or available.
- `countrySubdivisionCode`: Country subdivision code according to [ISO 3166-2](https://en.wikipedia.org/wiki/ISO_3166-2). `null` if code is not set or available.

Example query:

`GET https://cloudnet.fmi.fi/api/sites`

Response body:

```json
[
    {
    "id": "alomar",
    "humanReadableName": "Alomar",
    "type": [
      "hidden"
    ],
    "latitude": 62.278,
    "longitude": 16.008,
    "altitude": 380,
    "gaw": null,
    "dvasId": null,
    "country": "Norway",
    "countryCode": "NO",
    "countrySubdivisionCode": null
  },
...
]
```

### `GET /api/products` → `Product[]`

Fetch information on the products served in the data portal. Responds with an array of `Product` objects,
each having the properties:

- `id`: Unique identifier of the product.
- `humanReadableName`: Name of the product in a human-readable format.
- `level`: Product level. Is either `1b`, `1c`, `2` or `3`.
- `experimental`: `true` on experimental products, `false` on normal ones.

Example query:

`GET https://cloudnet.fmi.fi/api/products`

Response body:

```json
[
  {
    "id": "categorize",
    "humanReadableName": "Categorize",
    "level": "1c"
  },
...
]
```

### `GET /api/products/variables` → `ProductVariables[]`

Fetch product variables. Responds with an array of `ProductVariable` objects,
each having the properties:

- `id`: Unique identifier of the product.
- `humanReadableName`: Name of the product in a human-readable format.
- `level`: Product level. Is either `1b`, `1c`, or `2`.
- `variables`: List of product variables, each having the following properties:
  - `id`: Unique identifier of the variable.
  - `humanReadableName`: Variable name in a human-readable format.
  - `order`: Number indicating the relevance of the variable for the product in question. `0` is the most relevant variable for the product. Used for sorting plots.

**NOTE:** The list of returned variables is not exhaustive, the products may contain more variables than listed. Internally this list is used for plotting.

Example query:

`GET https://cloudnet.fmi.fi/api/products/variables`

Response body:

```json
  [
  {
    "id": "disdrometer",
    "humanReadableName": "Disdrometer",
    "level": "1b",
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

Fetch information on the instruments supported by the data portal. Responds with an array of `Instrument` objects,
each having the properties:

- `id`: Unique identifier of the instrument.
- `humanReadableName`: Name of the instrument in a human readable format.
- `type`: Instrument type. May be, for example, `radar`, `lidar`, or `mwr`.
- `allowedTags`: Allowed tags for this instrument. May be, for example, `co`, `cross`, or `calibration`.

Example query:

`GET https://cloudnet.fmi.fi/api/instruments`

Response body:

```json
[
  {
    "id": "mira",
    "humanReadableName": "METEK MIRA-35 cloud radar",
    "type": "radar",
    "allowedTags": []
  },
...
]
```

### `GET /api/models` → `Model[]`

Fetch information on the different model file types served in the data portal. Responds with an array of `Model` objects,
each having the properties:

- `id`: The unique identifier of the model.
- `optimumOrder`: Integer that signifies model quality. Better models have lower `optimumOrder`.

Example query:

`GET https://cloudnet.fmi.fi/api/models`

Response body:

```json
[
  {
    "id": "ecmwf",
    "optimumOrder": 0
  },
...
]
```

### `GET /api/files/UUID` → `File`

Fetch metadata for a single data object using its UUID (Universal Unique IDentifier).
All the data objects downloaded from the data portal are associated with a UUID,
which can be found in the data object's global attribute `file_uuid`.
To view the global attributes of a NetCDF file, one may use `ncdump -h file.nc`.

On a successful query the route responds with a `File` object, which has the following properties:

- `uuid`: UUIDv4 identifier.
- `version`: String identifying file version. Empty string on volatile files.
- `pid`: The persistent identifier of the data object. Empty string for data objects that do not have a PID, such as volatile files.
- `volatile`: `true` if the file has been modified recently and may change in the future,
- `legacy`: `true` if the file is legacy data, `false` if it has been processed with CloudnetPy.
  `false` if the file has not been changed recently and will not change in the future.
- `measurementDate`: The date on which the data was measured, `YYYY-MM-DD`.
- `history`: A freeform history of the file set by the creator/processor of the file. This field
  is not curated in any way and may or may not contain helpful information.
- `checksum`: The SHA-256 checksum of the file. Useful for verifying file integrity.
- `size`: Size of the file in bytes.
- `format`: The data structure of the file. Either `NetCDF3` or `HDF5 (NetCDF4)`.
- `createdAt`: The datetime on which the file was created. In ISO 8601 -format.
- `updatedAt`: The datetime on which the file was last updated. In ISO 8601 -format.
- `sourceFileIds`: Comma-separated list of `uuid` strings corresponding to the source files that were used to generate the file. If the source file information is not available, this is `null`.
- `cloudnetpyVersion`: The version of the [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy) library.
- `site`: `Site` object containing information of the site on which the measurement was made.
- `product`: `Product` object containing information of the data product.
- `downloadUrl`: The full URL to the data object. Useful for downloading the file.
- `filename`: The name of the file.
- `quality`: Quality status of the product. Either `nrt` (Near Real Time) or `qc` (Quality Controlled).
- `qualityScore`: A float in the range [0, 1] signifying the results of automatic quality tests, or `null` when the quality test report is not available. `1` means that all tests passed, `0` that no tests pass.

Example query:

`GET https://cloudnet.fmi.fi/api/files/911bd5b1-3104-4732-9bd3-34ed8208adad`

Response body:

```json
{
  "uuid": "911bd5b1-3104-4732-9bd3-34ed8208adad",
  "version": "icRZfkXzTHB2-uuRTDaSV-eQu6N5wNm",
  "pid": "https://hdl.handle.net/21.12132/1.911bd5b131044732",
  "volatile": false,
  "legacy": false,
  "measurementDate": "2020-01-05",
  "history": "2020-09-25 02:51:44 - categorize file created\n2020-09-25 02:50:07 - radar file created\n2020-09-25 02:49:27 - ceilometer file created",
  "checksum": "51db399d73e69842988d35e7e14fff815342f6925c70c0bdc9296b2847960738",
  "size": "8836007",
  "format": "HDF5 (NetCDF4)",
  "createdAt": "2020-12-01T09:30:40.016Z",
  "updatedAt": "2020-12-01T09:30:40.016Z",
  "sourceFileIds": [
    "f4cb2b92bd0c49779606b87440afa7b7",
    "6ac91e0483934db2afb3bacc97a7d8c0",
    "3171e11d022549b29e863138c406dce8"
  ],
  "cloudnetpyVersion": "1.3.1",
  "site": {
    "id": "bucharest",
    "humanReadableName": "Bucharest",
    "type": [
      "cloudnet"
    ],
    "latitude": 44.348,
    "longitude": 26.029,
    "altitude": 93,
    "gaw": "Unknown",
    "dvasId": "INO",
    "country": "Romania"
  },
  "product": {
    "id": "categorize",
    "humanReadableName": "Categorize",
    "level": "1c"
  },
  "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/911bd5b1-3104-4732-9bd3-34ed8208adad/20200105_bucharest_categorize.nc",
  "filename": "20200105_bucharest_categorize.nc"
  "quality": "nrt",
  "qualityScore": 0.9
}
```

### `GET /api/files` → `File[]`

Queries the metadata of multiple product files. On a successful query responds with an array of `File` objects.
The results can be filtered with the following parameters:

- `site`: One or more `Site` ids, from which to fetch file metadata.
- `date`: Only fetch data from a given date. Date format is `YYYY-MM-DD`.
- `dateFrom`: Limit query to files whose `measurementDate` is `dateFrom` or later. Same date format as in `date`.
  By default `measurementDate` is not limited.
- `dateTo`: Limit query to files whose `measurementDate` is `dateTo` or earlier. Same date format as in `date`.
  If omitted will default to the current date.
- `product`: One or more `Product` ids, by which to filter the files. May NOT be `model`; for model files, see route `/api/model-files`.
- `filename`: One or more filenames by which to filter the files.
- `allVersions`: By default the API returns only the latest version of the files. Adding this parameter will fetch all existing versions.
- `showLegacy`: By default the API does not return legacy data. Adding this parameter will fetch also legacy data.

Note: one or more of the parameters _must_ be issued. A query without any valid parameters will result in a `400 Bad Request` error.

Example query for fetching metadata for all classification files from Bucharest starting at 24. April 2020 and ending at the current date:

`GET https://cloudnet.fmi.fi/api/files?site=bucharest&dateFrom=2020-04-24&product=classification`

Response body:

```json
[
  {
    "uuid": "f63628b4-7e68-4c98-87ea-86a247f7fcfd",
    "version": "",
    "pid": "",
    "volatile": true,
    "legacy": false,
    "measurementDate": "2021-02-21",
    "history": "2021-02-23 08:04:18 - classification file created\n2021-02-23 08:04:01 - categorize file created\n2021-02-23 02:04:13 - radar file created\n2021-02-23 02:04:24 - ceilometer file created",
    "checksum": "9ccd36e4c6ee94c5179e34e08ff7898bf31979277badeb5806b688a30e23feda",
    "size": "82547",
    "format": "HDF5 (NetCDF4)",
    "createdAt": "2021-02-23T02:05:03.970Z",
    "updatedAt": "2021-02-23T08:04:20.186Z",
    "cloudnetpyVersion": "1.9.2",
    "site": {
      "id": "bucharest",
      "humanReadableName": "Bucharest",
      "type": [
        "cloudnet"
      ],
      "latitude": 44.348,
      "longitude": 26.029,
      "altitude": 93,
      "gaw": "Unknown",
      "dvasId": "INO",
      "country": "Romania"
    },
    "product": {
      "id": "classification",
      "humanReadableName": "Classification",
      "level": "2"
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/f63628b4-7e68-4c98-87ea-86a247f7fcfd/20210221_bucharest_classification.nc",
    "filename": "20210221_bucharest_classification.nc",
    "quality": "nrt",
    "qualityScore": 0.9
  },
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
- `allModels`: By default the API returns only the best model available matching the given search parameters. Adding this parameter will fetch all model files matching the search paramterers.

The `ModelFile` response similar to the `File` response, with an additional `model` property. The `model` property containing a `Model` object (see example response).
Furthermore, the `ModelFile` response omits the fields `cloudnetPyVersion` and `sourceFileIds`, as this information is not available for model files.

Example query for fetching metadata for `gdas1` model from Lindenberg on 2. March 2021:

`GET https://cloudnet.fmi.fi/api/model-files?site=lindenberg&date=2021-02-03&model=gdas1`

Response body:

```json
[
  {
    "uuid": "90f95f81-4f45-4efb-a25d-80066652aece",
    "version": "",
    "pid": "",
    "volatile": true,
    "legacy": false,
    "measurementDate": "2021-02-03",
    "history": "2021-02-04 08:10:26 - File content harmonized by the CLU unit.\n03-Feb-2021 18:59:45: Created from GDAS1 profiles produced with the profile binary in the HYSPLIT offline package using convert_gdas12pro.sh.",
    "checksum": "0b8b621ad2d76ca629451f56c94b79b432caba9c2839b3fcc535910544b3b854",
    "size": "194983",
    "format": "HDF5 (NetCDF4)",
    "createdAt": "2021-02-04T08:10:28.001Z",
    "updatedAt": "2021-02-04T08:10:28.001Z",
    "site": {
      "id": "lindenberg",
      "humanReadableName": "Lindenberg",
      "type": ["cloudnet"],
      "latitude": 52.208,
      "longitude": 14.118,
      "altitude": 104,
      "gaw": "LIN",
      "dvasId": "LIN",
      "country": "Germany"
    },
    "product": {
      "id": "model",
      "humanReadableName": "Model",
      "level": "1b"
    },
    "model": {
      "id": "gdas1",
      "optimumOrder": 3
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/product/90f95f81-4f45-4efb-a25d-80066652aece/20210203_lindenberg_gdas1.nc",
    "filename": "20210203_lindenberg_gdas1.nc"
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
- `visualizations`: An array of visualization metadata, each entry having the following properties:
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
The route responds with an array of `Visualization` objects.

Example request:

`GET https://cloudnet.fmi.fi/api/visualizations/?date=2021-10-25&site=mindelo`

Response body:

```json
[
  {
    "sourceFileId": "36a95fab-f07f-4182-99b8-0082b26b6069",
    "visualizations": [
      {
        "s3key": "20211025_mindelo_hatpro-36a95fab-LWP.png",
        "productVariable": {
          "id": "mwr-LWP",
          "humanReadableName": "Liquid water path",
          "order": "0"
        },
        "dimensions": null
      }
    ],
    "productHumanReadable": "Microwave radiometer",
    "locationHumanReadable": "Mindelo",
    "volatile": true,
    "legacy": false
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

The response is an array of `Upload` objects. The `Upload` object has the following properties:

- `uuid`: Unique identifier of the file.
- `checksum`: An MD5 checksum of the file.
- `filename`: Original name of the file.
- `measurementDate`: The date of the measurements contained in the file.
- `tags`: Array of tags.
- `size`: File size in bytes.
- `status`: Status of the file. See above for possible statuses.
- `createdAt`: Date of file metadata creation.
- `updatedAt`: Date of last file and/or metadata update.
- `site`: `Site` object.
- `instrument`: `Instrument` object.
- `instrumentPid`: Persistent identifier (PID) for the instrument.
- `downloadUrl`: A URL for direct download of the file.

Example query:

`GET https://cloudnet.fmi.fi/api/raw-files?site=norunda&updatedAtFrom=2021-09-01`

Response body:

```json
[
  {
    "uuid": "890c7130-8cf0-44a2-b5bc-75fcec344d97",
    "checksum": "1a3ab11370e1b3c4833fb812c0a0f82e",
    "filename": "220701_050008_P09_ZEN.LV0",
    "measurementDate": "2022-07-01",
    "size": "975360456",
    "status": "uploaded",
    "createdAt": "2022-07-01T10:24:44.130Z",
    "updatedAt": "2022-07-01T12:25:05.188Z",
    "instrumentPid": "https://hdl.handle.net/21.12132/3.bf6c0c3926c54dbb",
    "tags": [],
    "siteId": "norunda",
    "instrumentId": "rpg-fmcw-94",
    "site": {
      "id": "norunda",
      "humanReadableName": "Norunda",
      "type": [
        "cloudnet"
      ],
      "latitude": 60.086,
      "longitude": 17.479,
      "altitude": 46,
      "gaw": "NOR",
      "dvasId": "NOR",
      "actrisId": null,
      "country": "Sweden",
      "countryCode": "SE",
      "countrySubdivisionCode": null
    },
    "instrument": {
      "id": "rpg-fmcw-94",
      "type": "radar",
      "humanReadableName": "RPG-Radiometer Physics RPG-FMCW-94 cloud radar",
      "allowedTags": []
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/raw/890c7130-8cf0-44a2-b5bc-75fcec344d97/220701_050008_P09_ZEN.LV0"
  }
  ...
]
```

### `GET /api/raw-model-files` → `ModelUpload[]`

Query the metadata of uploaded model files. Query parameters are as above, with the following exception:
Instead of the `instrument` parameter, it is possible to filter the results with the `model` parameter, which takes `Model` id.

Example query:

`GET https://cloudnet.fmi.fi/api/raw-files?site=norunda&updatedAtFrom=2021-09-01`

Response body:

```json
[
  {
    "uuid": "375e32c1-d30c-4265-98ff-d07623b0c524",
    "checksum": "f9c59e2db978a2d2b6cb55d3af342b80",
    "filename": "20210904_norunda_ecmwf.nc",
    "measurementDate": "2021-09-04",
    "size": "501500",
    "status": "processed",
    "createdAt": "2021-09-04T18:37:02.852Z",
    "updatedAt": "2021-09-07T18:37:00.945Z",
    "site": {
      "id": "norunda",
      "humanReadableName": "Norunda",
      "type": [
        "cloudnet"
      ],
      "latitude": 60.086,
      "longitude": 17.479,
      "altitude": 46,
      "gaw": "NOR",
      "dvasId": "NOR",
      "country": "Sweden"
    },
    "model": {
      "id": "ecmwf",
      "optimumOrder": 0
    },
    "downloadUrl": "https://cloudnet.fmi.fi/api/download/raw/375e32c1-d30c-4265-98ff-d07623b0c524/20210904_norunda_ecmwf.nc"
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

Download one day of classification data from all sites using the `requests` package:

```python
import requests

url = 'https://cloudnet.fmi.fi/api/files'
payload = {
    'date': '2020-10-01',
    'product': 'classification'
}
metadata = requests.get(url, payload).json()
for row in metadata:
    res = requests.get(row['downloadUrl'])
    with open(row['filename'], 'wb') as f:
        f.write(res.content)
```

## Notes

The API provides `gzip` compression of responses where applicable.
We recommend using the compression for large queries to reduce transmitted response size.
The compression can be enabled effortlessly in `curl` with the `--compressed` switch.

## Errors

The API responds to errors with the appropriate HTTP status code and an `Error` object, which has the properties:

- `status`: HTTP error code.
- `message`: Human readable error message as a string or an array of strings.

Example query:

`GET https://cloudnet.fmi.fi/api/files?product=sausage`

Response body:

```json
{
  "status": 404,
  "errors": ["One or more of the specified products were not found"]
}
```
