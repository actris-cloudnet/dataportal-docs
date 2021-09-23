# Cloudnet documentation

Cloudnet is a data pipeline to process and distribute cloud remote sensing
data from measurement sites all over the Globe. Cloudnet consists of
[Cloudnet data portal](https://cloudnet.fmi.fi),
[processing software CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy),
and various APIs that can be used, for example, to submit raw data or
programmatically access processed data products.

Cloudnet is also one key element of [ACTRIS research infrastructure](https://www.actris.eu/) that is
currently in its implementation phase. ACTRIS Data Centre node
for cloud profiling (CLU) utilizes Cloudnet ecosystem to serve official ACTRIS cloud
remote sensing products through the [ACTRIS data portal](https://actris.nilu.no/).
More info about ACTRIS data can be found from the
[ACTRIS data management plan](https://github.com/actris/data-management-plan/blob/master/DMP/ACTRIS-DMP.md).

The Cloudnet data portal contains more data than the official ACTRIS data portal,
but it may also provide only partially quality controlled data, non-standard metadata
schema, experimental products, and so on.

Cloudnet is developed and maintained by the [Finnish Meteorological Institute](https://en.ilmatieteenlaitos.fi/).

## Index

1. [API reference](#api-reference)
2. [Processing code and file format](#processing-code-and-file-format)
3. [Measurement sites](#measurement-sites)
3. [License](#license)
4. [Contact](#contact)

## API Reference

Cloudnet ecosystem provides several HTTP APIs to access different services:

* [Fetching data and metadata](api/data-portal.md)
* [Data submission](api/data-upload.md)
  * [Supported file types](api/upload-file-types.md)
* [Calibration](api/calibration.md)

## Processing code and files

* Cloudnet data are processed using open source Python package [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy).
More info about CloudnetPy can be found from its own [documentation](https://cloudnetpy.readthedocs.io/en/latest/?badge=latest).

* Cloudnet uses netCDF4 file format.
See the [file format description](https://cloudnetpy.readthedocs.io/en/latest/fileformat.html).

* Cloudnet files exist in different [data levels](levels.md).


## Measurement sites

These are the regular, operational Cloudnet sites:

* [Hyytiälä](sites/hyytiala.md)
* [Kenttärova](sites/kenttarova.md)

*(The site list is not exhaustive and will be completed later.)*

## License

Cloudnet data is licensed under a [Creative Commons Attribution 4.0 international licence](https://creativecommons.org/licenses/by/4.0).
  This is a human-readable summary of (and not a substitute for) the [licence](https://creativecommons.org/licenses/by/4.0/legalcode).

  You are free to:

  - **Share** — copy and redistribute the material in any medium or format</li>
  - **Adapt** — remix, transform, and build upon the material for any purpose, even commercially</li>

  Under the following terms:

  - **Attribution** — You must give appropriate credit, provide a link to the licence, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use</li>
  - **No additional restrictions** — You may not apply legal terms or technological measures that legally restrict others from doing anything the licence permits</li>

  The licensor cannot revoke these freedoms as long as you follow the licence terms.

## Contact

You can contact as by email at [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi). 
Questions can be also asked using [Cloudnet forum](https://forum.cloudnet.fmi.fi).
