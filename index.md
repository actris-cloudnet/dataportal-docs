# Cloudnet documentation

Cloudnet is a data pipeline to process and distribute cloud remote sensing
data from measurement sites all over the Globe. Cloudnet consists of
[Cloudnet data portal](https://cloudnet.fmi.fi), processing software
[CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy),
and various APIs to submit raw data and access processed data products.

Cloudnet is also one key element of [ACTRIS](https://www.actris.eu/). ACTRIS Data Centre node 
for cloud profiling (CLU) utilizes Cloudnet ecosystem to serve official ACTRIS cloud 
remote sensing products through the [ACTRIS data portal](https://actris.nilu.no/).

The Cloudnet data portal contains more data than the official ACTRIS data portal,
but it provides only partially quality controlled data, non-standard metadata
schema, experimental products, and so on.

## Index

1. [API reference](#api-reference)
2. [Processing code and file format](#processing-code-and-file-format)
3. [Measurement sites](#measurement-sites)
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

* Cloudnet files exist in different [Data Levels](levels.md).


## Measurement sites

These are the regular, operational Cloudnet sites:

* [Hyytiälä](sites/hyytiala.md)
* [Kenttärova](sites/kenttarova.md)

*(The site list is not exhaustive and will be completed later.)*


## Contact

You can contact as by email at [actris-cloudnet@fmi.fi](mailto:actris-cloudnet@fmi.fi). 
Questions can be also asked using [Cloudnet forum](https://forum.cloudnet.fmi.fi).
