---
layout: default
title: NCAR-RAF netCDF Conventions
nav_order: 1
description: "NCAR-RAF netCDF Conventions for airborne atmospheric research data - Version 2.1"
permalink: /
---

# NCAR-RAF netCDF Conventions
{: .no_toc }

## Version 2.1
{: .no_toc .no_anchor }

* * *

### National Center for Atmospheric Research
{: .no_toc .no_anchor}

### Research Aviation Facility
{: .no_toc .no_anchor}

### NOAA AOC
{: .no_toc .no_anchor}

### University of Wyoming (King Air)
{: .no_toc .no_anchor}

* * *

## Table of Contents
{: .no_toc }

1. TOC
{:toc}

## Appendix
{: .no_toc }

<ul id="appendix-toc">
  <li><a href="#references">References</a></li>
  <li>
    <a href="#size-distribution-probes">Size-Distribution Probes</a>
    <ul>
      <li><a href="#variable-names">Variable names</a></li>
      <li><a href="#cell-sizes">Cell sizes</a></li>
    </ul>
  </li>
</ul>

* * *

## Introduction

The goal is to support synchronous time series including the following properties:

* Both scalar and histogram variables.
* Different sample rates among variables

RAF's primary datasets consist of synchronous time series data. Data are written to the netCDF file one record per second, even for data at higher rates.

### Histogram data

Instruments which measure particle size distributions produce histograms. These instruments include the suite of aerosol and cloud probes.

### Multi-rate files

RAF's standard data product is a netCDF file where all measurements are output at 1 sps. Certain projects require high-rate data. RAF has standardized at 25 sps for high-rate data. In general we don't want to put out variables sampled at 1 sps at 25 sps, as there is no information in the data, and it would take up excess space.

There is also an occasional need to look at data at the sample rate.

![netCDF record structure](http://www.eol.ucar.edu/raf/Software/record.png)

* * *

## Changelog

### Version 2.1 released Jan 2025

* Coordinate variables for size-distributions
* Updated global attributes for data discovery and provenance.

### Version 2.0 released Jun 2021

* The primary difference is in how size-distributions are stored. The legacy zeroth bin has been removed. Now a 30 channel probe will have 30 bins, where as previously it had 31, with the zeroth (or 1st, depending on your method of numbering) bin being a fixed zero.

### Checking File Version

Check the Conventions global attribute for version number. The "NCAR-RAF/nimbus" entry is what you want. ACDD is for data discovery.

```bash
ncdump -h CAESARrf01.nc | grep Conventions
        :Conventions = "NCAR-RAF/nimbus-2.0,ACDD-1.3" ;
```

### Legacy Documentation

For files produced prior to 2021, visit **[RAF netCDF Conventions 1.3](https://archive.eol.ucar.edu/raf/Software/netCDF_1_3.html)**.

* * *

## NetCDF File Components

### Filenames

RAF uses a naming convention consisting of a project designator followed by the flight type, followed by a 2 digit flight number, followed by an indicator of sample rate ( 'h' to indicate high-rate data, low-rate data has no indicator), followed by ".nc". Prior to about 2005/2006, field projects were designated with a 3 digit number. With the adoption of ADS-3 / nidas in 2005/2006, a switch was made to upper-case project names.

For example, `PREDICTtf01.nc` would be low-rate data from **t**est **f**light 1 from the PREDICT project.
`SPICULEff01s.nc` would be sample-rate data from **f**erry **f**light 1 from the SPICULE project.
`RICOrf08h.nc` would be high-rate data from **r**esearch **flight 8 of the RICO project.

Flight types used by RAF are `ff`, `tf`, and `rf`.

### Data types

All our data are stored using single precision float.  This is not a requirement.  The modern netCDF APIs have functions that will return the data in whatever format you ask, regardless of how it is stored in the file.

### Naming conventions

Variable, dimension and attribute names should begin with a letter and be composed of letters, digits, and underscores.

#### Variable names

The **underscore** has special meaning for variable names. Anything following an underscore is considered a suffix for grouping variables from the same instrument or probe. There is no standard for suffix names, but RAF regularly uses about a dozen specific ones for describing an instrument's mounting location on the aircraft.  This allows for multiple identical probes, each must have a unique suffix.

For example.
  `AFSSP_RPO`,
  `CFSSP_RPO`,
  `PLWC_RPO`,
  `CONCF_RPO`
all belong to the same instrument. In our files RPO stands for "Right Pod Outboard".

#### Dimension names

Dimension names we use are:

* `Time` This is the UNLIMITED dimension for the number of rows or seconds in the file.
* `sps`# for variable data rate (sps1, sps5, sps25, sps1000, etc.)
* `histogram`# for histogram lengths (histogram16, histogram30, etc.)
* `bnds` size is 2, this for coordinate variable bounds.
* Probe Serial Numbers from coordinate variables will be used (e.g. CDP016, UHSAS011, F2DS020).

### Dimensions

#### First dimension - Time

`Time` is the unlimited dimension, and most every variable has the `Time` dimension. No more than one record is to be written per second; all subsamples are to be contained in the record, though data averaged down slower than 1 sps may be written (e.g., when you have 1 sample every 10 seconds).

#### Second dimension - data rate

The second dimension, if present, represents the number of samples per second (e.g. `sps25`). The size of this dimension will vary according to the output rate of the variable (e.g., RAF uses 1, 5, 10, 25, 50, 250, and 1000). If the dimension does not exist, then there is only one sample for the second.

#### Third dimension - histogram length

The third dimension, if present, signifies a histogram and is the length of the array. (This will be present only for array data).

### Variables

<http://www.cfconventions.org/>.

#### Time

Time follows the recommendations set forth in the [CF conventions](http://cfconventions.org/latest.html) section 4.4. This always has one dimension, the Time dimension. It is assumed that subsamples, dimension two in a variable, are evenly spaced. Data that are averaged down to 1hz, the timestamp is the mid-point of the second (i.e. it is the average of all the data that was acquired during that second).

```cdl
        int Time(Time) ;
                Time:long_name = "time of measurement" ;
                Time:standard_name = "time" ;
                Time:units = "seconds since 2004-08-02 00:00:00 +0000" ;
                Time:strptime_format = "seconds since %F %T %z" ;
```

RAF sets the `Time` units to midnight prior to take-off and then the values of Time are _seconds since midnight_. This helps with correlation of NASA Ames and ICARTT data formats.

#### Missing data

At this time we use the _FillValue attribute.

* * *

### Attributes

#### Global attributes

Global attributes description. Blank descriptions can be found at the [ESIP ACDD](https://wiki.esipfed.org/Data_Discovery_(ACDD)) web page.

##### Data Discovery - Source

| Attribute | Example | Description |
|-----------|---------|-------------|
| `program` | `"NSF NCAR"` | Program name |
| `institution` | `"National Center for Atmospheric Research"` | Institution name |
| `Address` | `"P.O. Box 3000, Boulder, CO 80307-3000"` | Institution address |
| `Phone` | `"(303) 497-1030"` | Contact phone number |
| `source` | `"airborne observations"` | Data source |
| `creator_name` | `"NCAR EOL - Research Aviation Facility"` | Creator name |
| `creator_email` | `"raf-pm at ucar.edu"` | Creator email |
| `creator_url` | `"https://www.eol.ucar.edu/who-we-are/eol-organization/research-aviation-facility-raf"` | Creator URL |
| `creator_type` | `"group"` | Creator type (person, group, institution) |
| `creator_group` | `"NSF NCAR C130 Team"` | Creator group name |
| `publisher_name` | `"UCAR NCAR - Earth Observing Laboratory"` | Publisher name |
| `publisher_url` | `"https://www.eol.ucar.edu/data-software/eol-field-data-archive"` | Publisher URL |
| `publisher_email` | `"datahelp at eol.ucar.edu"` | Publisher email |
| `publisher_type` | `"group"` | Publisher type |
{: .global-attributes-table}


##### Data Discovery - ID

| Attribute | Example | Description |
|-----------|---------|-------------|
| `DOI` | `"https://doi.org/10.26023/44NW-3RPN-ZF0B"` | Complete DOI name |
| `naming_authority` | `"org.doi"` | Naming authority |
| `id` | `"10.26023/44NW-3RPN-ZF0B"` | Unique identifier |

##### Data Discovery - File Format

| Attribute | Example | Description |
|-----------|---------|-------------|
| `Conventions` | `"NCAR-RAF/nimbus-2.0,ACDD-1.3"` | List of conventions used |
| `ConventionsURL` | `"https://www.eol.ucar.edu/raf/Software/netCDF.html"` | Conventions documentation URL |
| `standard_name_vocabulary` | `"CF-1.11"` | Standard name vocabulary |

##### Data Discovery - About (Spatiotemporal)

| Attribute | Example | Description |
|-----------|---------|-------------|
| `date_created` | `"2025-02-11T22:19:21 +0000"` | File creation date |
| `platform` | `"N130AR"` | Platform identifier |
| `platform_type` | `"aircraft"` | Platform type |
| `project` | `"CAESAR"` | Project name |
| `FlightNumber` | `"rf01"` | Flight number |
| `FlightDate` | `"02/28/2024"` | Flight date |
| `time_coverage_start` | `"2024-02-28T11:39:00 +0000"` | Start time of data coverage |
| `time_coverage_end` | `"2024-02-28T18:36:44 +0000"` | End time of data coverage |
| `latitude_coordinate` | `"LATC"` | Variable name of platform best latitude |
| `longitude_coordinate` | `"LONC"` | Variable name of platform best longitude |
| `zaxis_coordinate` | `"GGALT"` | Variable name of platform best altitude |
| `time_coordinate` | `"Time"` | Variable name of the time variable |
| `geospatial_lat_min` | `67.73269f` | Minimum latitude |
| `geospatial_lat_max` | `71.56526f` | Maximum latitude |
| `geospatial_lon_min` | `-17.13059f` | Minimum longitude |
| `geospatial_lon_max` | `20.60835f` | Maximum longitude |
| `geospatial_vertical_min` | `169.6806f` | Minimum altitude (m) |
| `geospatial_vertical_max` | `5996.425f` | Maximum altitude (m) |
| `geospatial_vertical_positive` | `"up"` | Vertical positive direction |
| `geospatial_vertical_units` | `"m"` | Vertical coordinate units |
| `wind_field` | `"WSC WDC WIX"` | List of wind variables |

##### Provenance

| Attribute | Example | Description |
|-----------|---------|-------------|
| `ProcessorRepositoryURL` | `"https://github.com/NCAR/nimbus"` | Processor code repository |
| `ProcessorRepositoryBranch` | `"master"` | Repository branch |
| `ProcessorRepositoryRevision` | `"v5.0-282"` | Repository revision |
| `ProcessorRepositoryDate` | `"Tue Feb 4 14:29:22 2025 -0700"` | Repository commit date |
| `ProcessorRepositoryHASH` | `"b8fbb30ae4e66cc67f05fc036e0e325808da26d4"` | Repository commit hash |
| `ProcessorRepositoryDirt` | | Lists processor source code files that were modified and not committed |
| `ProjectDirectoryRepoURL` | `"git@github.com:NCAR/aircraft_projects.git"` | Project configuration repository |
| `ProjectDirectoryRevision` | `"894cf75dc2d4976e715de059206cccae1283398"` | Project directory revision |
| `ProjectDirectoryDirt` | | Lists files modified and not committed |
| `NIDASrevision` | `"v1.2.4"` | NIDAS version for raw data decoding |
| `SizeDistributionLegacyZeroBin` | `"no"` | Legacy size distribution zero bin flag |
| `InterpolationMethod` | `"Linear"` | Interpolation method used |

##### Ancillary Information

| Attribute | Example | Description |
|-----------|---------|-------------|
| `landmarks` | `"39.9088 -105.117 jeffco, 67.85572 20.22513 kiruna"` | Comma-separated list of landmarks (lat lon label) |
| `Categories` | `"Aerosol,Atmos. State,Chemistry,CloudAndPrecip,Housekeeping,NavAttitude,NavPosition,NavVelocity,Non-Standard,Radiation,Thermodynamic,Uncorrected Raw,Wind"` | List of all possible variable categories |
{: #global-attributes-table}

#### Variable attributes

All variables contain the basic attributes `units`, `long_name`, and `_FillValue` (`missing_value` has been deprecated).

Every variable will have either `SampledRate` or `Dependencies`. This is how you can determine if it is a raw vs. derived (calculated) variable.

##### All Variables

| Attribute | Example | Description |
|-----------|---------|-------------|
| `units` | `"degree"`, `"#/cm3"`, `"m/s"` | **Required.** Units of measurement |
| `long_name` | `"IRS Aircraft Pitch Angle"`, `"CDP Concentration (per cell)"` | **Required.** Description of the variable |
| `_FillValue` | `-32767.f` | **Recommended.** Missing value flag |
| `actual_range` | `-3.31724 15.88532f`, `0.f, 538.7375f` | Recommended. Min/max values in file |
| `standard_name` | `"platform_pitch_angle"`, `"upward_air_velocity"` | Optional. CF convention standard name |
| `DataQuality` | `"Preliminary"`, `"Good"` | Optional. Quality assessment (Bad, Preliminary, Good) |
| `Category` | `"NavAttitude"`, `"CloudAndPrecip"`, `"Wind"` | Optional. Variable category from CategoryList |
| `valid_range` | `-180.f, 180.f` | Optional. Valid range of variable |

##### Raw Variables (Sampled)

| Attribute | Example | Description |
|-----------|---------|-------------|
| `SampledRate` | `50`, `10` | **Optional.** Sample rate in samples per second (sps) |
| `TimeLag` | `-180` | Optional. Time lag in milliseconds (negative = correction) |
| `TimeLagUnits` | `"milliseconds"` | Optional. Units of TimeLag attribute |
| `CalibrationCoefficients` | `0.26f, 1.f` | Optional. DC voltage to engineering units conversion (already applied) |
| `DespikeSlope` | | Optional. Max valid slope between consecutive samples |
| `SerialNumber` | `"CDP016"` | Optional. Instrument serial number |

##### Derived Variables (Calculated)

| Attribute | Example | Description |
|-----------|---------|-------------|
| `Dependencies` | `"2 ACDP_LWOO TASX"`, `"9 TASX VEWC VNSC PITCH ROLL THDG ATTACK SSLIP VSPD"` | **Optional.** Input variables used in calculation (count + variable names) |

##### Special Attributes for Size-Distribution Probes

| Attribute | Example | Description |
|-----------|---------|-------------|
| `FirstBin` | `0` | Optional. First valid bin index (inclusive) |
| `LastBin` | `29` | Optional. Last valid bin index (inclusive) |
| `CellSizes` | `2.f, 3.f, 4.f, ...` | Optional. Lower bin limits in micrometers |
| `CellSizeUnits` | `"micrometers"` | Optional. Units of CellSizes |
| `CellSizeNote` | | Optional. Notes about bin edges |
| `DepthOfField` | `1.2f` | Optional. Probe depth of field |
| `BeamDiameter` | `0.2f` | Optional. Laser beam diameter |
| `Density` | `1.f` | Optional. Particle density assumption |
| `PLWfactor` | `1.e-06f` | Optional. Liquid water content conversion factor |
| `DBZfactor` | `1000000.f` | Optional. Radar reflectivity conversion factor |
{: #probe-table}

#### Example Variable Data

##### PITCH (Aircraft Pitch Angle) - Raw Variable

| Attribute | Value |
|-----------|-------|
| Variable Type | `float PITCH(Time)` |
| `_FillValue` | `-32767.f` |
| `units` | `"degree"` |
| `long_name` | `"IRS Aircraft Pitch Angle"` |
| `standard_name` | `"platform_pitch_angle"` |
| `valid_range` | `-180.f, 180.f` |
| `actual_range` | `-3.31724 15.88532f` |
| `Category` | `"NavAttitude"` |
| `SampledRate` | `50` |
| `TimeLag` | `-180` |
| `TimeLagUnits` | `"milliseconds"` |
| `DataQuality` | `"Preliminary"` |
| `CalibrationCoefficients` | `0.26f, 1.f` |

##### CCDP_LWOO (CDP Concentration) - Derived Variable

| Attribute | Value |
|-----------|-------|
| Variable Type | `float CCDP_LWOO(Time, sps1, CDP016)` |
| `_FillValue` | `-32767.f` |
| `units` | `"#/cm3"` |
| `long_name` | `"CDP Concentration (per cell)"` |
| `actual_range` | `0.f, 538.7375f` |
| `Category` | `"CloudAndPrecip"` |
| `SerialNumber` | `"CDP016"` |
| `DataQuality` | `"Preliminary"` |
| `Dependencies` | `"2 ACDP_LWOO TASX"` |
| `FirstBin` | `0` |
| `LastBin` | `29` |
| `CellSizes` | `2.f, 3.f, 4.f, 5.f, 6.f, 7.f, 8.f, 9.f, 10.f, 11.f, 12.f, 13.f, 14.f, 16.f, 18.f, 20.f, 22.f, 24.f, 26.f, 28.f, 30.f, 32.f, 34.f, 36.f, 38.f, 40.f, 42.f, 44.f, 46.f, 48.f, 50.f` |
| `CellSizeUnits` | `"micrometers"` |
| `CellSizeNote` | `"CellSizes are lower bin limits as particle"` |
| `DepthOfField` | `1.2f` |
| `BeamDiameter` | `0.2f` |
| `Density` | `1.f` |
| `PLWfactor` | `1.e-06f` |
| `DBZfactor` | `1000000.f` |

##### WIC (Wind Vector, Vertical Component) - Derived Variable

| Attribute | Value |
|-----------|-------|
| Variable Type | `float WIC(Time, sps25)` |
| `_FillValue` | `-32767.f` |
| `units` | `"m/s"` |
| `long_name` | `"GPS-Corrected Wind Vector, Vertical Gust Component"` |
| `standard_name` | `"upward_air_velocity"` |
| `actual_range` | `-3.393437 5.762867f` |
| `Category` | `"Wind"` |
| `DataQuality` | `"Good"` |
| `Dependencies` | `"9 TASX VEWC VNSC PITCH ROLL THDG ATTACK SSLIP VSPD"` |

* * *

## Appendices
{: .no_toc }

### References
{: .no_toc }

[NetCDF Software Package](http://www.unidata.ucar.edu/software/netcdf/), from the UNIDATA Program Center of the University Corporation for Atmospheric Research.

[UDUNITS Software Package](http://www.unidata.ucar.edu/software/udunits/), from the UNIDATA Program Center of the University Corporation for Atmospheric Research.

[CF Conventions](http://www.cfconventions.org/conventions.html), Climate Forcast data and meta-data conventions.

[ACDD](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3), Attribute Conventions for Data Discovery

* * *

### Size-Distribution Probes
{: .no_toc }

Units for all size-distributions will always be #/vol. They will never be normalized (i.e. divided by bin width or log bin width).

#### Probe variable names
{: .no_toc }

Each PMS probe has two histograms stored in the netCDF file. First is the raw counts and this variable always starts with the letter 'A' (for "Actual"). Second is the size-distribution array which always starts with the letter 'C'. Below you will see the variable names for the orginal PMS probe and for the DMT converted electronics.

|Probe|# bins|Counts name|Concentration name|DMT Converted|# bins|Counts name|Concentration name|
|-----|-----|-----|-----|-----|-----|-----|-----|
|FSSP|16|AFSSP_xxx|CFSSP_xxx|S100|30|AS100_xxx|CS100_xxx|
|PCASP|16|APCAS_xxx|CPCAS_xxx|S200|30|AS200_xxx|CS200_xxx|
|F300|32|AF300_xxx|CF300_xxx|S300|30|AS300_xxx|CS300_xxx|
|||||CDP|30|ACDP_xxx|CCDP_xxx|
|||||UHSAS|99|AUHSAS_xxx|CUHSAS_xxx|
|260X|64|A260X_xxx|C260X_xxx|||||
|2D-C (entire-in algo)|32|A1DC_xxx|C1DC_xxx|Fast2DC|64|A1DC_xxx|C1DC_xxx|
|2D-C (center-in algo)|64|A2DC_xxx|C2DC_xxx|Fast2DC|128|A2DC_xxx|C2DC_xxx|
{: .variable-attributes-table}

SPEC probes are only processed through our OAP post-processing tool. This will produce two sets of data for each probe. One for round particles, and one for all particles. This processor uses center-in algorithm.

|SPEC Probe|# bins|Type|Counts name|Size-distribution name|
|-----|-----|-----|-----|-----|
|Fast2DS (vertical)|128|all|A2DSVA_xxx|C2DSVA_xxx|
||128|round|A2DSVR_xxx|C2DSVR_xxx|
|Fast2DS (horizontal)|128|all|A2DSHA_xxx|C2DSHA_xxx|
||128|round|A2DSHR_xxx|C2DSHR_xxx|
|HVPS|128|all|A2DHA_xxx|C2DH\_xxx|
||128|round|A2DHR_xxx|C2DHR_xxx|
{: .variable-attributes-table}

* The _xxx is any number of alpha-numeric characters uniquely identifying the probe in this dataset. This allows for multiple identical probes to exist in the same file.
* Two sets of arrays are produced for each 2D probe. The 1DC pair is produced from the 2D probe data using an "entire-in" algorithm. The 2DC pair is produced using a "center-in" algorithm (see our 2D processing document for further information).

#### Cell sizes
{: .no_toc }

Cell sizes (aka bin edges) are stored as an attribute of the size-distribution array. They are **lower bin limits**. These are diameters. The first cell size is the lower bin limit for the first bin of data. Also note the attributes **FirstBin** and **LastBin**, these should be used to determine which bins are considered valid, both are inclusive.

e.g. The bin limits for CCDP[n] are CellSizes[n] and CellSizes[n+1], lower and upper respectivily.

In the examples below, there are 31 cell sizes to go with the 30 used values of the size-distribution for the current value. Older version of RAF netCDF files (pre 2022) have a "legacy zeroth bin" in the histogram. Old PMS probes had a rotating housekeeping value in the first bin, and we wanted to preserve the raw data, and have a one-to-one correspondance between the counts array and the size-distribution array.

##### ACDP_LWOO (CDP Raw Accumulation - per cell)
{: .no_toc }

| Attribute | Value (Current) | Legacy Value |
|-----------|-----------|-----------|
| Variable Type | `float ACDP_LWOO(Time, sps1, Vector30)` | `float ACDP_LWOO(Time, sps1, Vector31)` |
| `_FillValue` | `-32767.f` | `-32767.f` |
| `units` | `"count"` | `"count"` |
| `long_name` | `"CDP Raw Accumulation (per cell)"` | `"CDP Raw Accumulation (per cell)"` |
| `actual_range` | `0.f, 8328.f` | `0.f, 8328.f` |
| `Category` | `"CloudAndPrecip"` | `"PMS Probe"` |
| `SerialNumber` | `"CDP016"` | `"CDP016"` |
| `SampledRate` | `10` | `10` |
| `DataQuality` | `"Preliminary"` | `"Preliminary"` |
{: .probe_table}

##### CCDP_LWOO (CDP Concentration - per cell)

| Attribute | Value (Current) | Legacy Value |
|-----------|-----------|-----------|
| Variable Type | `float CCDP_LWOO(Time, sps1, Vector30)` | `float CCDP_LWOO(Time, sps1, Vector31)` |
| `_FillValue` | `-32767.f` | `-32767.f` |
| `units` | `"#/cm3"` | `"#/cm3"` |
| `long_name` | `"CDP Concentration (per cell)"` | `"CDP Concentration (per cell)"` |
| `actual_range` | `0.f, 273.5868f` | `0.f, 273.5868f` |
| `Category` | `"CloudAndPrecip"` | `"PMS Probe"` |
| `SerialNumber` | `"CDP016"` | `"CDP016"` |
| `DataQuality` | `"Preliminary"` | `"Preliminary"` |
| `Dependencies` | `"2 ACDP_LWOO TASX"` | `"2 ACDP_LWOO TASX"` |
| `FirstBin` | `0` | `1` |
| `LastBin` | `29` | `30` |
| `CellSizes` | `2.12f, 3.14f, 4.16f, 5.18f, 6.2f, 7.22f, 8.24f, 9.26f, 10.29f, 11.31f, 12.33f, 13.35f, 14.37f, 16.41f, 18.46f, 20.5f, 22.54f, 24.58f, 26.63f, 28.67f, 30.71f, 32.75f, 34.8f, 36.84f, 38.88f, 40.92f, 42.97f, 45.01f, 47.05f, 49.1f, 51.14f` | `2.12f, 3.14f, 4.16f, 5.18f, 6.2f, 7.22f, 8.24f, 9.26f, 10.29f, 11.31f, 12.33f, 13.35f, 14.37f, 16.41f, 18.46f, 20.5f, 22.54f, 24.58f, 26.63f, 28.67f, 30.71f, 32.75f, 34.8f, 36.84f, 38.88f, 40.92f, 42.97f, 45.01f, 47.05f, 49.1f, 51.14f` |
| `CellSizeUnits` | `"micrometers"` | `"micrometers"` |
| `CellSizeNote` | `"CellSizes are lower bin limits as particle"` | `"CellSizes are upper bin limits as particle"` |
| `HistogramNote` | (not present) | `"Zeroth data bin is an unused legacy placeholder."` |
| `DepthOfField` | `1.2f` | `1.2f` |
| `BeamDiameter` | `0.2f` | `0.2f` |
| `Density` | `1.f` | `1.f` |
| `PLWfactor` | `1.e-06f` | `1.e-06f` |
| `DBZfactor` | `1000000.f` | `1000000.f` |
{: .probe_table}

##### CONCD_LWOO (CDP Concentration - all cells)

| Attribute | Value (Current) | Legacy Value |
|-----------|-----------|-----------|
| Variable Type | `float CONCD_LWOO(Time)` | `float CONCD_LWOO(Time)` |
| `_FillValue` | `-32767.f` | `-32767.f` |
| `units` | `"#/cm3"` | `"#/cm3"` |
| `long_name` | `"CDP Concentration (all cells)"` | `"CDP Concentration (all cells)"` |
| `actual_range` | `0.f, 963.0727f` | `0.f, 963.0727f` |
| `Category` | `"CloudAndPrecip"` | `"PMS Probe"` |
| `SerialNumber` | `"CDP016"` | `"CDP016"` |
| `DataQuality` | `"Preliminary"` | `"Preliminary"` |
| `Dependencies` | `"1 CCDP_LWOO"` | `"1 CCDP_LWOO"` |
{: .probe_table}

##### Key Differences Between Versions

| Attribute | Change |
|--------|--------|
| **Vector dimension** | Changed from Vector31 (31 bins) to Vector30 (30 bins) |
| **FirstBin** | Changed from 1 to 0 |
| **LastBin** | Changed from 30 to 29 |
| **Category** | Changed from "PMS Probe" to "CloudAndPrecip" |
| **CellSizeNote** | Changed from "upper bin limits" to "lower bin limits" |
| **HistogramNote** | Removed entirely |
{: .probe_table}

FirstBin is typically set to two or three for OAP probes as we find the data to be questionable at the smaller sizes.

* * *

## Contact

<div style="background-color: #f5f7fa; border-left: 4px solid #3860B0; padding: 1.5rem; margin: 2rem 0; border-radius: 4px;">

**For further information, please contact:**

**Chris Webster**  
National Center for Atmospheric Research  
Research Aviation Facility

📞 [(303) 497-1044](tel:+13034971044)  
✉️ [cjw@ucar.edu](mailto:cjw@ucar.edu?Subject=RAF%20netCDF%20Conventions%20Inquiry)

</div>

* * *
