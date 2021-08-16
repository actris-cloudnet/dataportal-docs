Welcome! This is the documentation of the [Cloudnet data portal](https://cloudnet.fmi.fi).

# Overview

Cloudnet is a data pipeline to process and distribute cloud remote sensing
data from measurement sites all over the Globe. Cloudnet consists of a 
[processing code](https://github.com/actris-cloudnet/cloudnetpy), 
[data portal](https://cloudnet.fmi.fi), and various APIs 
to submit raw data and access processed data products.

Cloudnet is also one key element of [ACTRIS](https://www.actris.eu/). ACTRIS Data Centre node 
for cloud profiling (CLU) utilizes Cloudnet ecosystem to serve official ACTRIS cloud 
remote sensing products through the [ACTRIS data portal](https://actris.nilu.no/).

While the [Cloudnet data portal](https://cloudnet.fmi.fi) contains more data than
the official ACTRIS data portal, it provides only partially quality 
controlled data, non-standard metadata schema, experimental products, and so on.

# API Reference

Cloudnet ecosystem provides several HTTP APIs to access different services:

* [Data portal](api/data-portal.md)
* [Data submission](api/data-upload.md)
  * [Supported file types](api/upload-file-types.md)
* [Calibration](api/calibration.md)

# Processing code and file format

Cloudnet data are processed using open source Python package [CloudnetPy](https://github.com/actris-cloudnet/cloudnetpy).
More info about CloudnetPy can be found from its own [documentation](https://cloudnetpy.readthedocs.io/en/latest/?badge=latest).

Cloudnet uses netCDF4 file format. [File format description >](https://cloudnetpy.readthedocs.io/en/latest/fileformat.html)


# Measurement sites

These are the regular, operational Cloudnet sites:

* [Hyytiälä](sites/hyytiala.md)
* [Kenttärova](sites/kenttarova.md)


# Contact

* [Email](mailto:actris.cloudnet@fmi.fi)
* [Forum](https://forum.cloudnet.fmi.fi)
