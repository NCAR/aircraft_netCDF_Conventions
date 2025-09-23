NCAR-RAF netCDF Conventions  <!-- body { background: white; color: black; margin: 2em; margin-left: 3em; } h1,h2 { font-size: 1.5em } h3 { font-size: 1.3em } h4 { font-size: 1.2em } h1, h2, h3, h4 { font-style: normal; font-weight: normal; color: #3860B0; } span.runin { font-size: 1.1em; font-weight: bold; color: #3860B0; } a:link { color:navy; } a:visited { color:#386090; } li.toc { list-style: none; padding: .1em; } kbd.att { font-weight: normal; color: blue; } div.example { margin-left: 3em; margin-right: 3em; } div.definition { margin-left: 1em; } div.sidebar { margin-left: 50px; margin-right: 50px; padding: 20px; background-color: #f5deb4; } pre.CDL { color: brown; } table { padding: .25em; } span.new { background-color: #FF6666; } span.old { background-color: #FFFF88; text-decoration: line-through; } span.comment { background-color: #88FF88; font-style: italic; } -->

NCAR-RAF netCDF Conventions
===========================

### Version 2.1

* * *

#### National Center for Atmospheric Research

#### Research Aviation Facility

#### NOAA AOC

#### University of Wyoming (King Air)

* * *

Table of Contents
-----------------

*   [Introduction](#intro)

*   [NetCDF Files and Components](#file)

*   1  [Filename](#1-filenames)
*   2  [Data types](#2-data-types)
*   3  [Naming conventions](#3-naming-conventions)
*   4  [Dimensions](#4-dimensions)
*   5  [Variables](#5-variables)
*   6  [Attributes](#6-attributes)

Appendices
----------

*   A  [References](#A-references)
*   B  [Size-distribution Variable Description](#size)

* * *

## Changelog
---------

### 2.1 released Sep 2025

*   Coordinate variables for size-distributions

### 2.0 released Jun 2021

*   The primary difference is in how size-distributions are stored. The legacy zeroth bin has been removed. Now a 30 channel probe will have 30 bins, where as previously it had 31, with the zeroth (or 1st, depending on your method of numbering) bin being a fixed zero.

Check the Conventions global attribute for version number. The "NCAR-RAF/nimbus" entry is what you want. ACDD is for data discovery.

34% ncdump -h CAESARrf01.nc | grep Conventions
                :Conventions = "NCAR-RAF/nimbus-2.0,ACDD-1.3" ;

For files produced prior to 2021, visit **[RAF netCDF Conventions 1.3](netCDF_1_3.html)**.

* * *

## Introduction
------------

The goal is to support synchronous time series including the following properties:

*   Both scalar and histogram variables.
*   Different sample rates among variables

RAF's primary datasets consist of synchronous time series data. Data are written to the netCDF file one record per second, even for data at higher rates.

#### 1.0.1 Histogram data

Instruments which measure particle size distributions produce histograms. These instruments include the suite of aerosol and cloud probes.

#### 1.0.2 Multi-rate files

RAF's standard data product is a netCDF file where all measurements are output at 1 sps. Certain projects require high-rate data. RAF has standardized at 25 sps for high-rate data. In general we don't want to put out variables sampled at 1 sps at 25 sps, as there is no information in the data, and it would take up excess space.

There is also an occasional need to look at data at the sample rate.

![](http://www.eol.ucar.edu/raf/Software/record.png)

* * *

## NetCDF File Component Descriptions
----------------------------------

### 1 Filenames

RAF uses a naming convention consisting of a project designator followed by the flight type, followed by a 2 digit flight number, followed by an indicator of sample rate ( 'h' to indicate high-rate data, low-rate data has no indicator), followed by ".nc". Prior to about 2005/2006, field projects were designated with a 3 digit number. With the adoption of ADS-3 / nidas in 2005/2006, a switch was made to upper-case project names.

e.g.  

PREDICTtf01.nc would be low-rate data from **test** flight 1 from the PREDICT project.  
SPICULEff01s.nc would be sample-rate data from **ferry** flight 1 from the SPICULE project.  
RICOrf08h.nc would be high-rate data from **research** flight 8 of the RICO project.  

Flight types used by RAF are "ff", "tf", and "rf".

### 2 Data types

All data are stored using type float.

### 3 Naming conventions

Variable, dimension and attribute names should begin with a letter and be composed of letters, digits, and underscores.

#### 3.1 Variable names

The **underscore** has special meaning for variable names. Anything following an underscore is considered a suffix for grouping variables from the same instrument or probe. There is no standard for suffix names, but RAF regularly uses about a dozen specific ones for describing an instrument's mounting location on the aircraft.

e.g.,  
```
  AFSSP_RPO  
  CFSSP_RPO  
  PLWC_RPO  
  CONCF_RPO
```
all belong to the same intrument.

In our files RPO stands for "Right Pod Outboard".

#### 3.2 Dimension names

Dimension names we use are:

*   Time This is the UNLIMITED dimension for the number of rows or seconds in the file.
*   sps# for variable data rate (sps1, sps5, sps25, sps1000, etc.)
*   histogram# for histogram lengths (histogram16, histogram30, etc.)
*   bnds size is 2, this for coordinate variable bounds.
*   Probe Serial Numbers from coordinate variables will be used (e.g. CDP016, UHSAS011, F2DS020).

### 4 Dimensions

#### 4.1 First dimension - Time

**Time** is the **unlimited dimension**, and most every variable has the **Time** dimension. No more than one record is to be written per second; all subsamples are to be contained in the record, though data averaged down slower than 1 sps may be written (e.g., when you have 1 sample every 10 seconds).

#### 4.2 Second dimension - data rate

The second dimension, if present, represents the number of samples per second. The size of this dimension will vary according to the output rate of the variable (e.g., RAF uses 1, 5, 10, 25, 50, 250, and 1000). If the dimension does not exist, then there is only one sample for the second.

#### 4.3 Third dimension - histogram length

The third dimension, if present, signifies a histogram and is the length of the array. (This will be present only for array data).

### 5 Variables

http://www.cfconventions.org/.

#### 5.1 Time

Time follows the recomendations set forth in the [CF conventions](http://cfconventions.org/latest.html) section 4.4. This always has one dimension, the Time dimension. It is assumed that subsamples, dimension two in a variable, are evenly spaced. Data that are averaged down to 1hz, the timestamp is the mid-point of the second (i.e. it is the average of all the data that was acquired during that second).
```
        int Time(Time) ;
                Time:long_name = "time of measurement" ;
                Time:standard_name = "time" ;
                Time:units = "seconds since 2004-08-02 00:00:00 +0000" ;
                Time:strptime_format = "seconds since %F %T %z" ;
```
RAF sets the Time:units to midnight prior to take-off and then the values of Time are seconds since midnight. This helps with correlation of NASA Ames and ICARTT data formats.

#### 5.2 Missing data

At this time we use the _FillValue attribute.

### 6 Attributes

#### 6.1 Global attributes
```
// global attributes:
                :institution = "NCAR Research Aviation Facility" ;
                :Address = "P.O. Box 3000, Boulder, CO 80307-3000" ;
                :Phone = "(303) 497-1030" ;
                :creator_url = "http://www.eol.ucar.edu" ;
                :creator_email = "codiac at ucar.edu" ;
                :RepositoryURL = "https://github.com/NCAR/nimbus" ;
                :RepositoryBranch = "master" ;
                :RepositoryRevision = "v5.0-80M" ;
                :RepositoryDate = "Sat Apr 24 12:26:46 2021 -0600" ;
                :RepositoryHASH = "8fd13287c0ce7a4cdbc387e355e3fa306fbb3f6b" ;
                :RepositoryStatus = "M src/amlib/std/fssp.c" ;
                :ProjectDirectoryRepoURL = "https://github.com/ncar/aircraft_projects" ;
                :ProjectDirectoryRevision = "da43da3aeb11dcc9abde6a10bf00cdb707a6763" ;
                :Conventions = "NCAR-RAF/nimbus" ;
                :ConventionsURL = "http://www.eol.ucar.edu/raf/Software/netCDF.html" ;
                :Metadata_Conventions = "Unidata Dataset Discovery v1.0" ;
                :ConventionsVersion = "2.0" ;
                :standard_name_vocabulary = "CF-1.0" ;
                :NIDASrevision = "v1.2-1469" ;
                :WARNING = "This file contains PRELIMINARY DATA that are NOT to be used for critical analysis." ;
                :date_created = "2021-06-06T23:41:31 +0000" ;
                :ProjectName = "SPICULE" ;
                :project = "SPICULE" ;
                :Platform = "N677F" ;
                :FlightNumber = "rf04" ;
                :FlightDate = "06/05/2021" ;
                :TimeInterval = "11:54:29-22:09:16" ;
                :InterpolationMethod = "Linear" ;
                :latitude_coordinate = "LATC" ;
                :longitude_coordinate = "LONC" ;
                :zaxis_coordinate = "GGALT" ;
                :time_coordinate = "Time" ;
                :geospatial_lat_min = 33.74531f ;
                :geospatial_lat_max = 40.17661f ;
                :geospatial_lon_min = -105.1467f ;
                :geospatial_lon_max = -96.44314f ;
                :geospatial_vertical_min = 304.9131f ;
                :geospatial_vertical_max = 13012.9f ;
                :geospatial_vertical_positive = "up" ;
                :geospatial_vertical_units = "m" ;
                :wind_field = "WSC WDC WIC" ;
                :landmarks = "39.9088 -105.117 BJC, 40.536 -104.8206 radar" ;
                :Categories = "Aerosol,Atmos. State,Chemistry,CloudAndPrecip,Housekeeping,NavAttitude,NavPosition,NavVelocity,Non-Standard,Radiation,Thermodynamic,Uncorrected Raw,Wind" ;
                :time_coverage_start = "2021-06-05T11:54:29 +0000" ;
                :time_coverage_end = "2021-06-05T22:09:16 +0000" ;
```
ConventionVersion is the RAF nimbus conventions versions number.

RepositoryStatus and ProjectDirectoryStatus, if present, indicate files that were used but not commited into github. Results may not be reproducable.

*_coordinate is required starting with version 1.3. These are the aircrafts "best" position variables.  

wind_field lists the variables comprising the 3D wind field. Wind Speed, Wind Direction, and Vertical Wind Speed respectively.

landmarks is a comma separated list of relevant locations on a map. Each landmark comprises three fields, "latitude longitude label".

#### 6.2 Variable attributes

All variables contain the basic attributes units, long_name, and _FillValue (missing_value has been deprecated).

Every variable will have either SampledRate or Dependencies, this is how you can determine if it is a raw vs. derived (calculated) variable.

In addition we provide some other attributes; not all are used for every variable:

standard_name we have adopted the CF [CF conventions](http://cfconventions.org/latest.html) section 3.3 use of standard_name where applicable.

SampledRate is the rate (sps) at which the variable was sampled onboard the aircraft. This does not apply to derived variables. All variables recorded by the data acquistion system will have this attribute. All derived variables will not.

DataQuality attempts to inform the user of the quality of these data. Some values are Bad, Preliminary and Good. Data distributed in the field will always be set to Preliminary.

Category Since a file can contain hundreds of variables with not very descriptive names, this is provided to help create sub-lists of variables. Comma separated list.

CalibrationCoefficients are the values used to produce engineering units from a measurement's DC voltage. It is used by the analog/digital group. **These values have already been applied!** They are present for documentation.

Dependencies are the input variables that were used to produce this derived (calculated) variable. Like CalibrationCoefficients , these are present for documentation. All derived variables will have this attribute.

For processing purposes, variables in **our** files are organized into two sorted lists. The first sorted list consists of raw sampled variables. These only have calibration coefficients applied to them. The second group consists of derived variables. I have included one example from each of the 3 groups (which also show different dimension schemes):

        float PITCH(Time) ;
                PITCH:_FillValue = -32767.f ;
                PITCH:units = "degree" ;
                PITCH:long_name = "IRS Aircraft Pitch Angle" ;
                PITCH:standard_name = "platform_pitch_angle" ;
                PITCH:valid_range = -180.f, 180.f ;
                PITCH:actual_range = -3.31724 15.88532f ;
                PITCH:Category = "Analog" ;
                PITCH:SampledRate = 50 ;
                PITCH:TimeLag = -180 ;
                PITCH:TimeLagUnits = "milliseconds" ;
                PITCH:DataQuality = "Preliminary" ;
                PITCH:CalibrationCoefficients = 0.26f, 1.f ;

        float CCDP_LWOO(Time, sps1, Vector30) ;
                CCDP_LWOO:_FillValue = -32767.f ;
                CCDP_LWOO:units = "#/cm3" ;
                CCDP_LWOO:long_name = "CDP Concentration (per cell)" ;
                CCDP_LWOO:actual_range = 0.f, 538.7375f ;
                CCDP_LWOO:Category = "PMS Probe" ;
                CCDP_LWOO:SerialNumber = "CDP016" ;
                CCDP_LWOO:DataQuality = "Preliminary" ;
                CCDP_LWOO:Dependencies = "2 ACDP_LWOO TASX" ;
                CCDP_LWOO:FirstBin = 0 ;
                CCDP_LWOO:LastBin = 29 ;
                CCDP_LWOO:CellSizes = 2.f, 3.f, 4.f, 5.f, 6.f, 7.f, 8.f, 9.f, 10.f, 11.f, 12.f, 13.f, 14.f, 16.f, 18.f, 20.f, 22.f, 24.f, 26.f, 28.f, 30.f, 32.f, 34.f, 36.f, 38.f, 40.f, 42.f, 44.f, 46.f, 48.f, 50.f ;
                CCDP_LWOO:CellSizeUnits = "micrometers" ;
                CCDP_LWOO:CellSizeNote = "CellSizes are lower bin limits as particle " ;
                CCDP_LWOO:DepthOfField = 1.2f ;
                CCDP_LWOO:BeamDiameter = 0.2f ;
                CCDP_LWOO:Density = 1.f ;
                CCDP_LWOO:PLWfactor = 1.e-06f ;
                CCDP_LWOO:DBZfactor = 1000000.f ;

        float WIC(Time, sps25) ;
                WIC:_FillValue = -32767.f ;
                WIC:units = "m/s" ;
                WIC:long_name = "GPS-Corrected Wind Vector, Vertical Gust Component" ;
                WIC:standard_name = "upward_air_velocity" ;
                WIC:actual_range = -3.393437 5.762867f ;
                WIC:Category = "Wind" ;
                WIC:DataQuality = "Good" ;
                WIC:Dependencies = "9 TASX VEWC VNSC PITCH ROLL THDG ATTACK SSLIP VSPD" ;

## Appendices
----------

### A  References

[NetCDF]

[NetCDF Software Package](http://www.unidata.ucar.edu/software/netcdf/), from the UNIDATA Program Center of the University Corporation for Atmospheric Research.

  

[UDUNITS]

[UDUNITS Software Package](http://www.unidata.ucar.edu/software/udunits/), from the UNIDATA Program Center of the University Corporation for Atmospheric Research.

  

[CF Conventions]

[CF Conventions](http://www.cfconventions.org/conventions.html), Climate Forcast data and meta-data conventions.

  

[ACDD]

[ACDD](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3), Attribute Conventions for Data Discovery

### B  Size-distribution Probe Variables Description

Units for all size-distributions will always be #/vol. They will never be normalized (i.e. divided by bin width or log bin width).

#### B.1 Variable names

Each PMS probe has two histograms stored in the netCDF file. First is the raw counts and this variable always starts with the letter 'A' (for "Actual"). Second is the size-distribution array which always starts with the letter 'C'. Below you will see the variable names for the orginal PMS probe and for the DMT converted electronics.

|Probe|# bins|Counts name|Concentration name|DMT Converted|# bins|Counts name|Concentration name|
|-----|-----|-----|-----|-----|-----|-----|-----|
|FSSP|16|AFSSP_xxx|CFSSP_xxx|S100|30|AS100_xxx|CS100_xxx|
|PCASP|16|APCAS_xxx|CPCAS_xxx|S200|30|AS200_xxx|CS200_xxx|
|F300|32|AF300_xxx|CF300_xxx|S300|30|AS300_xxx|CS300_xxx|
|||||CDP|30|ACDP_xxx|CCDP_xxx|
|||||UHSAS|99|AUHSAS_xxx|CUHSAS_xxx|
|260X|64|A260X_xxx|C260X_xxx||||
|2D-C (entire-in algo)|32|A1DC_xxx|C1DC_xxx|Fast2DC|64|A1DC_xxx|C1DC_xxx|
|2D-C (center-in algo)|64|A2DC_xxx|C2DC_xxx|Fast2DC|128|A2DC_xxx|C2DC_xxx|

  
SPEC probes are only processed through our OAP post-processing tool. This will produce two sets of data for each probe. One for round particles, and one for all particles. This processor uses center-in algorithm.

|SPEC Probe|# bins|Type|Counts name|Size-distribution name|
|-----|-----|-----|-----|-----|
|Fast2DS (vertical)|128|all|A2DSVA_xxx|C2DSVA_xxx|
||128|round|A2DSVR_xxx|C2DSVR_xxx|
|Fast2DS (horizontal)|128|all|A2DSHA_xxx|C2DSHA_xxx|
||128|round|A2DSHR_xxx|C2DSHR_xxx|
|HVPS|128|all|A2DHA_xxx|C2DH\_xxx|
||128|round|A2DHR_xxx|C2DHR_xxx|

*   The _xxx is any number of alpha-numeric characters uniquely identifying the probe in this dataset. This allows for multiple identical probes to exist in the same file.
*   Two sets of arrays are produced for each 2D probe. The 1DC pair is produced from the 2D probe data using an "entire-in" algorithm. The 2DC pair is produced using a "center-in" algorithm (see our 2D processing document for further information).

#### B.2 Cell sizes

Cell sizes (aka bin edges) are stored as an attribute of the size-distribution array. They are **lower bin limits**. These are diameters. In the example below, there are 31 cell sizes to go with the 30 used values of the size-distribution. The first cell size is the lower bin limit for the first bin of data. Also note the attributes **FirstBin** and **LastBin**, these should be used to determine which bins are considered valid, both are inclusive.

e.g. The bin limits for CS100[n] are CellSizes[n] and CellSizes[n+1], lower and upper respectivily.

        float ACDP_LWOO(Time, sps1, Vector30) ;
                ACDP_LWOO:_FillValue = -32767.f ;
                ACDP_LWOO:units = "count" ;
                ACDP_LWOO:long_name = "CDP Raw Accumulation (per cell)" ;
                ACDP_LWOO:actual_range = 0.f, 8328.f ;
                ACDP_LWOO:Category = "PMS Probe" ;
                ACDP_LWOO:SerialNumber = "CDP016" ;
                ACDP_LWOO:SampledRate = 10 ;
                ACDP_LWOO:DataQuality = "Preliminary" ;

        float CCDP_LWOO(Time, sps1, Vector30) ;
                CCDP_LWOO:_FillValue = -32767.f ;
                CCDP_LWOO:units = "#/cm3" ;
                CCDP_LWOO:long_name = "CDP Concentration (per cell)" ;
                CCDP_LWOO:actual_range = 0.f, 273.5868f ;
                CCDP_LWOO:Category = "PMS Probe" ;
                CCDP_LWOO:SerialNumber = "CDP016" ;
                CCDP_LWOO:DataQuality = "Preliminary" ;
                CCDP_LWOO:Dependencies = "2 ACDP_LWOO TASX" ;
                CCDP_LWOO:FirstBin = 0 ;
                CCDP_LWOO:LastBin = 29 ;
                CCDP_LWOO:CellSizes = 2.12f, 3.14f, 4.16f, 5.18f, 6.2f, 7.22f, 8.24f, 9.26f, 10.29f, 11.31f, 12.33f, 13.35f, 14.37f, 16.41f, 18.46f, 20.5f, 22.54f, 24.58f, 26.63f, 28.67f, 30.71f, 32.75f, 34.8f, 36.84f, 38.88f, 40.92f, 42.97f, 45.01f, 47.05f, 49.1f, 51.14f ;
                CCDP_LWOO:CellSizeUnits = "micrometers" ;
                CCDP_LWOO:CellSizeNote = "CellSizes are lower bin limits as particle " ;
                CCDP_LWOO:DepthOfField = 1.2f ;
                CCDP_LWOO:BeamDiameter = 0.2f ;
                CCDP_LWOO:Density = 1.f ;
                CCDP_LWOO:PLWfactor = 1.e-06f ;
                CCDP_LWOO:DBZfactor = 1000000.f ;

        float CONCD_LWOO(Time) ;
                CONCD_LWOO:_FillValue = -32767.f ;
                CONCD_LWOO:units = "#/cm3" ;
                CONCD_LWOO:long_name = "CDP Concentration (all cells)" ;
                CONCD_LWOO:actual_range = 0.f, 963.0727f ;
                CONCD_LWOO:Category = "PMS Probe" ;
                CONCD_LWOO:SerialNumber = "CDP016" ;
                CONCD_LWOO:DataQuality = "Preliminary" ;
                CONCD_LWOO:Dependencies = "1 CCDP_LWOO" ;

Older version of RAF netCDF files (pre 2022) have a "legacy zeroth bin" in the histogram. Old PMS probes had a rotating housekeeping value in the first bin, and we wanted to preserve the raw data, and have a one-to-one correspondance between the counts array and the size-distribution array. Previously:

        float ACDP_LWOO(Time, sps1, Vector31) ;
                ACDP_LWOO:_FillValue = -32767.f ;
                ACDP_LWOO:units = "count" ;
                ACDP_LWOO:long_name = "CDP Raw Accumulation (per cell)" ;
                ACDP_LWOO:actual_range = 0.f, 8328.f ;
                ACDP_LWOO:Category = "PMS Probe" ;
                ACDP_LWOO:SerialNumber = "CDP016" ;
                ACDP_LWOO:SampledRate = 10 ;
                ACDP_LWOO:DataQuality = "Preliminary" ;

        float CCDP_LWOO(Time, sps1, Vector31) ;
                CCDP_LWOO:_FillValue = -32767.f ;
                CCDP_LWOO:units = "#/cm3" ;
                CCDP_LWOO:long_name = "CDP Concentration (per cell)" ;
                CCDP_LWOO:actual_range = 0.f, 273.5868f ;
                CCDP_LWOO:Category = "PMS Probe" ;
                CCDP_LWOO:SerialNumber = "CDP016" ;
                CCDP_LWOO:DataQuality = "Preliminary" ;
                CCDP_LWOO:Dependencies = "2 ACDP_LWOO TASX" ;
                CCDP_LWOO:FirstBin = 1 ;
                CCDP_LWOO:LastBin = 30 ;
                CCDP_LWOO:CellSizes = 2.12f, 3.14f, 4.16f, 5.18f, 6.2f, 7.22f, 8.24f, 9.26f, 10.29f, 11.31f, 12.33f, 13.35f, 14.37f, 16.41f, 18.46f, 20.5f, 22.54f, 24.58f, 26.63f, 28.67f, 30.71f, 32.75f, 34.8f, 36.84f, 38.88f, 40.92f, 42.97f, 45.01f, 47.05f, 49.1f, 51.14f ;
                CCDP_LWOO:CellSizeUnits = "micrometers" ;
                CCDP_LWOO:CellSizeNote = "CellSizes are upper bin limits as particle " ;
                CCDP_LWOO:HistogramNote = "Zeroth data bin is an unused legacy placeholder." ;
                CCDP_LWOO:DepthOfField = 1.2f ;
                CCDP_LWOO:BeamDiameter = 0.2f ;
                CCDP_LWOO:Density = 1.f ;
                CCDP_LWOO:PLWfactor = 1.e-06f ;
                CCDP_LWOO:DBZfactor = 1000000.f ;

Difference (plus is new, minus is old):
```
+       float ACDP_LWOO(Time, sps1, Vector30) ;
-       float ACDP_LWOO(Time, sps1, Vector31) ;

+       float CCDP_LWOO(Time, sps1, Vector30) ;
-       float CCDP_LWOO(Time, sps1, Vector31) ;

+               CCDP_LWOO:FirstBin = 0 ;
+               CCDP_LWOO:LastBin = 29 ;

-               CCDP_LWOO:FirstBin = 1 ;
-               CCDP_LWOO:LastBin = 30 ;

+               CCDP_LWOO:CellSizeNote = "CellSizes are lower bin limits as particle " ;
-               CCDP_LWOO:CellSizeNote = "CellSizes are upper bin limits as particle " ;

-               CCDP_LWOO:HistogramNote = "Zeroth data bin is an unused legacy placeholder." ;
```
|bin#|0|1|2|3|...|29|
|----|----|----|----|----|----|----|
||FirstBin|||||LastBin|

FirstBin is typically set to two or three for OAP probes as we find the data to be questionable at the smaller sizes.

* * *

For further information contact:

Chris Webster  
National Center for Atmospheric Research  
Research Aviation Facility  
(303) 497-1044

[email](mailto:cjw@ucar.edu?Subject=from_RAF_Web_Software_Page_netCDF)

* * *
