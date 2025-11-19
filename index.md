# Cloudnet documentation

Cloudnet collects, processes, and distrubutes cloud remote sensing data
from [measurement sites all over the globe](https://cloudnet.fmi.fi/sites).

Cloudnet includes:

- [Data portal](https://cloudnet.fmi.fi) to browse and download data
- [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy) to process data
- [Cloudnet submit tool](https://github.com/actris-cloudnet/cloudnet-submit) to submit data
- [Cloudnet API client](https://github.com/actris-cloudnet/cloudnet-api-client) to search and download data
- [API](#api-reference) to access data and various services

Cloudnet is part of [ACTRIS research infrastructure](https://www.actris.eu/)
and is developed and maintained by
the [Finnish Meteorological Institute](https://en.ilmatieteenlaitos.fi/).

See [publications](https://cloudnet.fmi.fi/publications) using Cloudnet data or methods.

## API Reference

Cloudnet provides several HTTP APIs to access different services:

- [Fetching data and metadata](api/data-portal.md)
- [Data submission](api/data-upload.md)
- [Supported file types for submission](api/upload-file-types.md)
- [Calibration](api/calibration.md)

## Processing code and files

- Cloudnet uses open source Python package
  [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy)
  for processing data.
  See the [documentation](https://cloudnetpy.readthedocs.io/en/latest/?badge=latest).

- Cloudnet uses netCDF4 file format.
  See the [Cloudnet NetCDF convention](netcdf.md) and [descriptions of Cloudnet products](https://cloudnet.fmi.fi/products/).

## Contact

- Questions and feedback: [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi)
- News and updates: [Twitter](https://twitter.com/actris_cloudnet)
- Questions about code: [GitHub](https://github.com/actris-cloudnet)

### Bugs and feature requests

Create an issue in GitHub:

- [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy/issues/new)
- [Cloudnet data submission tool](https://github.com/actris-cloudnet/cloudnet-submit/issues/new)
- [RPG cloud radar reader](https://github.com/actris-cloudnet/rpgpy/issues/new)

## License

Cloudnet data is licensed under a [Creative Commons Attribution 4.0 international licence](https://creativecommons.org/licenses/by/4.0).
This is a human-readable summary of (and not a substitute for) the [licence](https://creativecommons.org/licenses/by/4.0/legalcode).

You are free to:

- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially

Under the following terms:

- **Attribution** — You must give appropriate credit, provide a link to the licence, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use
- **No additional restrictions** — You may not apply legal terms or technological measures that legally restrict others from doing anything the licence permits

The licensor cannot revoke these freedoms as long as you follow the licence terms.
