# Cloudnet NetCDF Convention

## Motivation

The Cloudnet convention is applicable to any dataset on a time-height grid,
including radar and lidar data, single-site model forecasts and derived
meteorological products. It adopts many of the components of other NetCDF
conventions, specifically the [Climate and Forecast (CF) Metadata
Conventions](https://cfconventions.org/). Conventions generally relate to the
attributes that should be supplied, or those that it is recommended to use if a
certain piece of information needs to be conveyed.

Suggestions for improvements and clarifications to the convention are welcome.

## Files and filenames

Each level 1 (instrumental or model data) or level 2 (meteorological product)
file should contain data from a single day. The times reported in the file
should be in hours UTC, so for instruments that operate continuously, each
individual file should run from midnight to midnight UTC.

Filenames should be of the form `YYYYMMDD_WHERE_WHAT.nc` or
`YYYYMMDD_WHERE_WHAT_ID.nc`, where the fields are as follows:

<dl>
  <dt><tt>YYYYMMDD</tt></dt>
  <dd>The date, UTC.</dd>
  <dt><tt>WHERE</tt></dt>
  <dd>
    A lower case string identifying the site (e.g. <tt>chilbolton</tt>,
    <tt>arm-sgp</tt>).
  </dd>
  <dt><tt>WHAT</tt></dt>
  <dd>
    This field either identifies the instrument (e.g. <tt>galileo</tt> for the
    Galileo radar), the model (e.g. <tt>harmonie-fmi-0-5</tt> for the 0-5 hour
    forecast of the HARMONIE-AROME model) or the meteorological product (e.g.
    <tt>iwc-Z-T-method</tt> for ice water content derived using the
    reflectivity+temperature method).
  </dd>
  <dt><tt>ID</tt></dt>
  <dd>
    Additional unique identifier to distinguish multiple files of the same type
    (e.g. two radars in the same location) from each other.
  </dd>
</dl>

The fields should not contain underscores (`_`); hyphens (`-`) should be used
to separate information within fields. This then allows for an additional field
to be added in a future revision of the convention. Thus filenames should
contain only the characters [`-_.a-z0-9`]. Spaces are forbidden as they have
the habit of breaking Unix scripts.

## Dimensions

The NetCDF dataset should contain the dimension `time`, which should be the
first dimension defined. The vertical dimension may be `range`, `height` or
`level`:

<dl>
  <dt><tt>time</tt></dt>
  <dd>
    This dimension may have the length <tt>unlimited</tt>. NetCDF permits one
    dimension to be unlimited, which means that variables using this dimension
    can grow along this dimension. However, if the data are read one variable
    at a time then the use of an unlimited dimension seems to slow down the
    read speed.
  </dd>
  <dt><tt>range</tt></dt>
  <dd>
    This dimension is used for instrumental data up to level 1b, and indicates
    that distance is measured from the instrument rather than from mean sea
    level, and also allows for instruments not pointing at zenith.
  </dd>
  <dt><tt>height</tt></dt>
  <dd>
    For level 1c and 2, ranges from instruments are converted to heights above
    mean sea level, and this dimension name is used.
  </dd>
  <dt><tt>level</tt></dt>
  <dd>
    For level 1 model data this is used to indicate model level rather than
    <tt>height</tt>, since model levels often do not correspond to unique
    heights.
  </dd>
</dl>

Other dimensions may be defined. For example, the level 1b model data contains
microwave propagation parameters derived from the model fields for several
different frequencies, so uses the dimension `frequency`. The level 1c
categorize dataset holds model data on the original vertical model grid (to
save space), which is referenced using the `model_height` dimension.

## Variables

### Compulsory variables

The following compulsory variables are stored as variables rather than global
attributes because they have a unit or other describing attribute associated
with them; the attributes that should be set are shown indented after each
variable name. Each NetCDF attribute consists of a "name" and a "value", where
the value can be a text string or a vector of numbers. All these variables are
of type `float`, i.e. a 4-byte floating-point number.

<dl>
  <dt><tt>latitude</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>degrees_north</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Latitude of site</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>longitude</tt></dt>
  <dd>
    It is conventional to always report positive longitudes, i.e. +359.0 rather than -1.0.
    <dl>
      <dt><tt>units</tt> = "<tt>degrees_east</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Longitude of site</tt>"</dt>
    </dl>
  </dd>
</dl>

For each dimension a "coordinate variable" must be defined, i.e. a vector
variable with the same name as the dimension. Typically these would be of type
`float`. Thus all datasets should contain a `time` variable:

<dl>
  <dt><tt>time(time)</tt></dt>
  <dd>
    Note that the <tt>float</tt> type has enough precision for time in hours to
    be discretised to better than 0.007 seconds.
    <dl>
      <dt><tt>units</tt> = "<tt>hours since YYYY-MM-DD 00:00:00 00:00</tt>"</dt>
      <dd>where YYYY-MM-DD must contain the date that the data were taken (e.g.
      2002-09-05). The zeros at the end indicate that the time is from midnight
      UTC (i.e. timezone 00:00). This reporting of time is from the CF
      convention. Note that reporting time in hours rather than seconds from
      midnight is much more convenient for the user.</dd>
      <dt><tt>long_name</tt> = "<tt>Time UTC</tt>"</dt>
      <dt><tt>axis</tt> = "<tt>T</tt>"</dt>
    </dl>
  </dd>
</dl>

A `range`, `height` or `level` variable should then also be defined, depending
on the dimensions present, e.g.

<dl>
  <dt><tt>range(range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>km</tt>"</dt>
      <dd>Note that reporting range in metres ("<tt>m</tt>") is also permissible.</dd>
      <dt><tt>long_name</tt> = "<tt>Range from antenna to the centre of each range gate</tt>"</dt>
      <dd>An example long name.</dd>
      <dt><tt>axis</tt> = "<tt>Z</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>height(height)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>m</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Height above mean sea level</tt>"</dt>
      <dt><tt>axis</tt> = "<tt>Z</tt>"</dt>
    </dl>
  </dd>
</dl>

Note that the `axis` attribute is the CF way of stating the dominant temporal
and vertical variables against which 2D variables in the file should be
plotted. No more than one axis of a given type should be present in the file.

### Compulsory variable attributes

All variables should set the following two attributes:

<dl>
  <dt><tt>units</tt></dt>
  <dd>
    The units should be readable by the UDUNITS package, as required by the CF
    convention description. An additionally accepted unit is <tt>dBZ</tt>. If
    possible, units should be SI. The main points for uniform use of units are
    as follows:
    <ul>
      <li>Exponents should be expressed by "<tt>g m-3</tt>", not "<tt>g
      m^{-3}</tt>", "<tt>gm-3</tt>", "<tt>g/m3</tt>", "<tt>g(m)^-1</tt>"
      etc.</li>
      <li>If conventional modifiers such as "kilo" are used, please use the
      correct case, i.e. "<tt>km</tt>" not "<tt>Km</tt>" for kilometers.</li>
      <li>The appropriate way to express microns is "<tt>um</tt>", not
      "<tt>microns</tt>" or "<tt>1e-6 m</tt>".</li>
      <li>The units for <tt>time</tt> should conform to the use indicated in the section above.</li>
      <li>Dimensionless variables use the unit "<tt>1</tt>".</li>
    </ul>
    Note that <i>bit fields</i> and <i>status fields</i>, defined below, need
    not use the <tt>units</tt> attribute.
  </dd>
  <dt><tt>long_name</tt></dt>
  <dd>
    This should be a concise but informative phrase describing the variable,
    short enough to fit comfortably in the axis or title of a plot (i.e.
    shorter than around 60 characters). It should start with an upper case
    letter.
  </dd>
</dl>

### Recommended variable attributes

The following attributes are good ways to express information about a variable.
They should conform to the conventions indicated.

<dl>
  <dt><tt>comment</tt></dt>
  <dd>
    This is by far the most important attribute that a variable can have as it
    describes to the user what the variable is. Do not assume that the user has
    a copy of documentation that should have been distributed with the file:
    put enough information here to explain what the variable contains, how it
    was derived, what the calibration convention was and things the user should
    be aware of when using this variable. If there are references specific to
    this variable (i.e. those that would be inappropriate in the global
    <tt>references</tt> attribute) then include them here. Ideally this
    attribute should start with "<tt>This variable contains...</tt>", such that
    it may be used as a general description of the variable for use with
    programs that generate automatic documentation from a NetCDF file; for
    example, the detailed descriptions of variables on the <a
    href="/radar/cloudnet/data/products/iwc-Z-T-method.html">IWC product
    page</a> were contained in <tt>comment</tt> attributes. Use complete
    sentences terminated with a full-stop/period so that extra comments can be
    easily appended. New line characters (ASCII code: decimal 10) should be
    used to break long lines. Note that the use of the plural <tt>comments</tt>
    has been deprecated.
  </dd>
  <dt><tt>_FillValue</tt> and <tt>missing_value</tt></dt>
  <dd>
    If the variable contains missing data (e.g. because an instrument was not
    working or the variable indicates cloud particle size but not cloud is
    present etc.) then both <tt>_FillValue</tt> and <tt>missing_value</tt>
    should be present to indicate which value has been used to flag that no
    valid data are available. They must be of the same type as the variable
    itself. The use of two different attributes is an unfortunate consequence
    of the fact that older programs may only expect <tt>missing_value</tt>
    while newer programs tend to use <tt>_FillValue</tt>.
  </dd>
  <dt><tt>source</tt></dt>
  <dd>
    For datasets containing variables derived from different sources, it is
    useful to indicate the particular source here. Typically one would take the
    global <tt>source</tt> attribute from the dataset from which this variable
    was derived.
  </dd>
</dl>

### Variables indicating error and sensitivity

All derived meteorological products at level 2 and above should ideally be
accompanied by an indication of their error. Typically errors can be divided
into _random error_ that decorrelates rapidly with time, and a _bias_ due to
the accuracy with which an instrument was calibrated and which may affect all
measurements in a day uniformly. Additionally, many instruments and the
products derived from them have a _sensitivity_, or a _minimum detectable
value_, which should be reported in order that comparison with models be fair.
Variables affected in this way should define one or more of the following
attributes:

<dl>
  <dt><tt>error_variable</tt></dt>
  <dd>Contains the name of the variable in the file that indicates the random error of the variable in question. Typically if the variable name were <tt>Z</tt>, then the corresponding <tt>error_variable</tt> would be <tt>Z_error</tt>.</dd>
  <dt><tt>bias_variable</tt></dt>
  <dd>As above, but for the bias. Similarly, the typical name for the bias in <tt>Z</tt> would be <tt>Z_bias</tt>.</dd>
  <dt><tt>sensitivity_variable</tt></dt>
  <dd>As above, but for the sensitivity. The typical name for the minimum detectable <tt>Z</tt> would be <tt>Z_sensitivity</tt>.</dd>
</dl>

Sometimes errors can have a long (and difficult to define) decorrelation time,
and it is not obvious how to differentiate between random error and bias. In
this case only an `error_variable` need be defined. The variables used to
report error and sensitivity should conform to the following conventions:

<ul>
  <li>An error/sensitivity variable may be a function of all, some or none of
  the dimensions used by the corresponding meteorological variable. For
  example, a random error might vary with <tt>time</tt> and <tt>height</tt>,
  while a bias might be a constant value for the whole file, indicating the
  expected accuracy of the calibration of the instrument from which the
  meteorological variable was derived (so therefore needing no dimensions). In
  the case of radar, the sensitivity of radar reflectivity is predominantly a
  function of <tt>height</tt>, as will be parameters derived from it.</li>
  <li>
    The <tt>units</tt> of the error/sensitivity variable would typically be the
    same as those of the meteorological variable. However, for errors and
    biases, two additional units are permissible:
    <dl>
      <dt><tt>%</tt></dt>
      <dd>If the error is of a fractional nature then it is appropriate to
      report a percentage error. However, this becomes ambiguous in the case of
      large fractional errors, as an error of 200% implies that a variable
      might range between 300% of its value to the negative (i.e. -100%) of its
      value. When errors are likely to exceed 25%, it is better to use units of
      <tt>dB</tt>.</dd>
      <dt><tt>dB</tt></dt>
      <dd>While decibels are often not familiar to non-instrumentalists, they
      are convenient for expressing fractional errors of any magnitude. Simply
      put, a "bel" is an order of magnitude so a "decibel" is a tenth of an
      order of magnitude. Hence an error of 3 dB corresponds to a "factor of 2"
      or +100%/-50% and an error of 10 dB corresponds to a "factor of 10" or
      +900%/-90%. Hence an error of a "factor of <tt>X</tt>" corresponds to
      10log<sub>10</sub>(<tt>X</tt>) dB.</dd>
    </dl>
  </li>
  <li>The error should correspond to <i>one standard deviation</i>, not be a
  95% confidence interval, which is around two standard deviations. This should
  be stated in the <tt>comment</tt> attribute, and ideally the
  <tt>long_name</tt> attribute. So for variable <tt>Z</tt> , its random error
  variable would typically have the long name <tt>Random error in Z, one
  standard deviation</tt>.</li>
  <li>The <tt>comment</tt> attribute should start with, for example, "<tt>This
  variable is the [random error in|expected bias in|approximate calibration
  error in|minimum detectable]...</tt>", and indicate how it was
  calculated.</li>
</ul>

### Bit fields and status fields

It is often necessary to indicate the status of a retrieval, enabling the user
to distinguish pixels for which the retrieval was (for example) "reliable",
"probably reliable but...", "unreliable", "not possible". Sometimes targets
need to be distinguished between a number of different types, such as "liquid
clouds", "ice clouds", "aerosol", "insects". In this case one can use a _status
field_, where the integer variable will be one of a limited number of values,
or a <i>bit field</i>, where each bit of the integer variable should be
interpretted as a separate flag. Such variables should always be of NetCDF type
`byte`, to avoid the byte-order confusion that is likely to arise with two-byte
and four-byte integers due to different CPU architectures. Additionally, it is
probably best not to use the most significant bit of the field, as this is used
to indicate the sign of the byte and could easily be misread by badly written
programs. Hence use no more than 7 bits per byte, and if you need more bits,
consider providing two bit fields. Rather than use a `units` attribute, the
variable should use a `definition` attribute, where each line (separated by the
newline character) indicates the meaning either of each value, or of each bit.
In the case of status fields, we could have:

<dl>
  <dt><tt>definition</tt> =</dt>
  <dd>"<tt>0: No cloud present<br>
  1: Reliable retrieval<br>
  2: Possibly unreliable retrieval due to spiders in the waveguide<br>
  3: Unreliable retrieval</tt>"</dd>
</dl>while in the case of bit fields we could have:
<dl>
  <dt><tt>definition</tt> =</dt>
  <dd>"<tt>Bit 0: Liquid droplets are present<br>
  Bit 1: Ice particles are present<br>
  Bit 2: Raindrops are present<br>
  Bit 3: Aerosol particles are present</tt>"</dd>
</dl>

Note that `definition` is used by programs such as `chilncplot` in the key at
the side of the plot to indicate the meaning of each colour, so the
descriptions should be fairly concise. Use of a `long_definition` attribute is
therefore recommended where more complete descriptions may be placed, but the
same format should be used, with a single line terminated by a newline
character (except the last) for each entry.

## Global attributes

Global attributes provide important information about the data in a NetCDF
file.

### Compulsory global attributes

The following attributes should be present and of type `text`:

<dl>
  <dt><tt>Conventions = "CF-1.8"</tt></dt>
  <dd>
    Indicates that your data satisfies the CF conventions. If your data doesn't
    satisfy the CF conventions, don't include this attribute.
  </dd>
  <dt><tt>day</tt></dt>
  <dd>The day of the month on which the data were taken, two-digits (e.g. "01").</dd>
  <dt><tt>month</tt></dt>
  <dd>The month of the year, two digits (e.g. "01" for January)</dd>
  <dt><tt>year</tt></dt>
  <dd>The year as a full four-digit number (e.g. "2024")</dd>
  <dt><tt>cloudnet_file_type</tt></dt>
  <dd>Identifier for product (e.g. "lidar" or "classification").</dd>
  <dt><tt>location</tt></dt>
  <dd>
    The site at which the instrument was operating, such as "Chilbolton",
    "Cabauw", "Palaiseau" and "ARM Southern Great Plains".
  </dd>
  <dt><tt>title</tt></dt>
  <dd>
    A suitable title for plots created from the dataset, such as "Ice water
    content from Chilbolton", "Chilbolton 94-GHz Cloud Radar (Galileo)" or
    "Cabauw 905-nm CT75K Vaisala Lidar Ceilometer".
  </dd>
  <dt><tt>history</tt></dt>
  <dd>
    Each program that acts on the file should append to this attribute a brief
    description of what they did, and when they did it (again using the
    newline character as a separator). Extra information can include the user
    and the name of the machine. For example, "<tt>Wed Nov 28 18:38:12 GMT 2001
    - NetCDF generated from original data by Robin Hogan
    &lt;r.j.hogan@reading.ac.uk&gt; on voldemort</tt>". If the calibration
    needs to be changed then it may be appended by "<tt>\nThu Nov 29 18:38:12
    GMT 2001 - Recalibrated (+3 dB) by Robin Hogan
    &lt;r.j.hogan@reading.ac.uk&gt; on voldemort</tt>", where '<tt>\n</tt>'
    indicates the newline character (i.e. not a backslash character followed
    by an "n" character).
  </dd>
  <dt><tt>source</tt></dt>
  <dd>
    In the case of instrumental data, this would contain a brief specification
    of the instrument. The spec of a radar should include frequency, antenna
    diameter, pulse repetition freqiency, pulse width (in microseconds) and
    peak power, and the spec of a lidar should include wavelength, divergence,
    field of view and pulse repetition frequency. The fields would be newline
    separated. In the case of model data a single-line title for the model is
    sufficient, e.g. "UK Met Office mesoscale model". Data derived
    from a variety of sources should concatenate the global <tt>source</tt>
    attributes from the input datasets, separated by semi-colon (<tt>;</tt>)
    and newline.
  </dd>
  <dt><tt>file_uuid</tt></dt>
  <dd>Universally unique identifier (UUID) of the file.</dd>
  <dt><tt>references</tt></dt>
  <dd>
    Any web-based or published information about the data, e.g. "Information on
    the data is available at http://www.met.rdg.ac.uk/radar/doc/galileo.html".
    Obviously please ensure that the web site referred to is maintained for the
    likely lifetime of the data.
  </dd>
</dl>

### Recommended global attributes

<dl>
  <dt><tt>comment</tt></dt>
  <dd>
    Any further general information for the user (that is not specific to
    individual variables) should be added here. Use complete sentences
    terminated with a full-stop/period so that extra comments can be easily
    appended. It is also useful to add newline characters to break up long
    lines.
  </dd>
  <dt><tt>source_file_uuids</tt></dt>
  <dd>
    Newline-separated list of source file UUIDs in products like categorize.
  </dd>
  <dt><tt>instrument_pid</tt></dt>
  <dd>
    Persistent identifier (PID) of the source instrument (e.g. <a
    href="https://hdl.handle.net/21.12132/3.d98f6fd2bec94e5e">
    https://hdl.handle.net/21.12132/3.d98f6fd2bec94e5e</a>).
  </dd>
  <dt><tt>pid</tt></dt>
  <dd>
    Persistent identifier (PID) of the file (e.g. <a
    href="https://hdl.handle.net/21.12132/1.ce67fc697f3f4aa5">
    https://hdl.handle.net/21.12132/1.ce67fc697f3f4aa5</a>).
  </dd>
  <dt><tt>serial_number</tt></dt>
  <dd>
    Serial number of the source instrument.
  </dd>
  <dt><tt>software_version</tt></dt>
  <dd>
    If the processing program changes over time then it is useful to store the
    version number (as a string) of the program here.
  </dd>
</dl>

## Recommended variables for radar and lidar data

The following describes additional conventions that should make radar and lidar
data from different sites as similar as possible.

### Scalar variables

The following variables are single values that are stored as variables rather
than global attributes because they have a unit or other describing attribute
associated with them; the attributes that should be set are shown indented
after each variable name. All these variables are of type <tt>float</tt>.

<dl>
  <dt><tt>altitude</tt></dt>
  <dd>
    To get the altitude of each range gate above mean sea level, the user of
    this data should add this value to the values in the <tt>range</tt>
    variable (assuming the instrument is vertically pointing, and taking
    account of the fact that <tt>altitude</tt> is in metres and <tt>range</tt>
    is in km).
    <dl>
      <dt><tt>units</tt> = "<tt>m</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Altitude of antenna above mean sea level</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>elevation</tt></dt>
  <dd>
    Most radars will be vertically pointing, so their elevation will be
    90&deg;. Lidars may be deployed off-zenith to avoid specular reflection
    from horizontally aligned plate crystals, in which case the elevation will
    be less than 90&deg;.
    <dl>
      <dt><tt>units</tt> = "<tt>degrees</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Elevation above horizon</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>azimuth</tt></dt>
  <dd>
    An optional variable that gives the azimuth of instruments that are not
    vertically pointing.
    <dl>
      <dt><tt>units</tt> = "<tt>degrees</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Azimuth clockwise from due north</tt>"</dt>
    </dl>
  </dd>
</dl>

For radar the following should also be defined:

<dl>
  <dt><tt>frequency</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>GHz</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Radar frequency</tt>"</dt>
    </dl>
  </dd>
</dl>

For lidar, use:

<dl>
  <dt><tt>wavelength</tt></dt>
  <dd>
    If this is a multi-wavelength lidar, then <tt>wavelength</tt> should be a
    one-dimensional array containing all the wavelengths available. This
    requires an extra dimension, also with name <tt>wavelength</tt>.
    <dl>
      <dt><tt>units</tt> = "<tt>nm</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Lidar wavelength</tt>"</dt>
    </dl>
  </dd>
</dl>

### Two-dimensional variables

Most two-dimensional variables will be of type <tt>float</tt>. However, for
some data it may make sense to use the <tt>short</tt> data type (a signed
2-byte integer; The CT75K lidar ceilometer is a good candidate as the raw data
are stored to this precision so no information is lost. You may then use
<tt>scale_factor</tt> and/or <tt>add_offset</tt> attributes to get the data
into suitable units and to provide the correct calibration. If both are present
then the data in the file should be scaled first before the offset is added.
Note also that the <tt>missing_value</tt> and <tt>\_FillValue</tt> attributes
apply to the data <i>before</i> it has been scaled and shifted in this way.
Usually <tt>scale_factor</tt> and <tt>add_offset</tt> would be of type
<tt>float</tt>.

For some variables, notably radar reflectivity, accurate calibration can be
difficult and the data may need to be recalibrated after the initial release.
These variables should therefore indicate the calibration that has been applied
to them in the processing stage in the <tt>calibration_applied</tt> attribute.

The following are variable names that could be used in radar data, and some of
the attributes that should be present:

<dl>
  <dt><tt>Z(time, range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>dBZ</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Radar reflectivity factor</tt>"</dt>
      <dt><tt>comment</tt> = "<tt>Calibration convention: in the absence of
      attenuation, a cloud at 273 K containing one million 100-micron droplets
      per cubic metre will have a reflectivity of 0 dBZ at all
      frequencies.</tt>"</dt>
      <dt><tt>calibration_applied</tt></dt>
      <dd>...in dB.</dd>
    </dl>
  </dd>
  <dt><tt>v(time, range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>m s-1</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Doppler velocity</tt>"</dt>
      <dt><tt>comment</tt> = "<tt>Positive velocities are away from the radar.</tt>"</dt>
      <dt><tt>folding_velocity</tt></dt>
      <dd>
	This attribute indicates that the velocities may be folded, lying in
	the range <tt>-folding_velocity</tt> to <tt>folding_velocity</tt>.
      </dd>
    </dl>
  </dd>
  <dt><tt>width(time, range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>m s-1</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Spectral width</tt>"</dt>
      <dt><tt>comment</tt> = "<tt>This variable is the standard deviation of
      the reflectivity-weighted velocities in the radar pulse
      volume.</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>sigma_v(time, range)</tt></dt>
  <dd>
    Level 1 data is typically averaged to 30 seconds, so the velocity variable
    in the NetCDF file is typically an average of a number of high-resolution
    mean velocity values measured in the averaging time. The <tt>sigma_v</tt>
    variable is the standard deviation of these high-resolution mean
    velocities. Spectral width is the standard deviation of actual particle
    velocities measured within the radar pulse volume in a short time
    (typically around 1 second), so tends to be dominated by the differential
    fall speeds of the different sized particles. This variable, on the other
    hand, is dominated by turbulence.
    <dl>
      <dt><tt>units</tt> = "<tt>m s-1</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Standard deviation of mean velocity</tt>"</dt>
      <dt><tt>comment</tt> = "<tt>The data in this file are at a lower
      resolution than the raw data, and this variable is the standard deviation
      of the raw Doppler velocities measured during in each output gate and
      ray.</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>ldr(time, range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>dB</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Linear depolarisation ratio</tt>"</dt>
    </dl>
  </dd>
</dl>

Similarly, the following are variable names that could be used with lidar data:

<dl>
  <dt><tt>beta(time, range)</tt></dt>
  <dd>
    If attenuated backscatter coefficient is measured at more than one
    wavelength, then the wavelength could be indicated in the variable name,
    such as <tt>beta1064</tt>, <tt>beta532</tt> etc.
    <dl>
      <dt><tt>units</tt> = "<tt>m-1 sr-1</tt>"</dt>
      <dt><tt>long_name</tt> = "<tt>Attenuated backscatter coefficient</tt>"</dt>
    </dl>
  </dd>
  <dt><tt>ldr(time, range)</tt></dt>
  <dd>
    <dl>
      <dt><tt>units</tt> = "<tt>1</tt>"</dt>
      <dd>Lidar depolarisation ratio normally lies in the range <tt>0</tt> to <tt>1</tt>.</dd>
      <dt><tt>long_name</tt> = "<tt>Linear depolarisation ratio</tt>"</dt>
    </dl>
  </dd>
</dl>

If there is a need to have an unprocessed version of a variable in the file
then I suggest using the names <tt>Z_raw</tt>, <tt>beta_raw</tt> and so on.
