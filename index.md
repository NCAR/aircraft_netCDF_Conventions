---
layout: default # Tells Jekyll to wrap this content with _layouts/default.html
title: NCAR-RAF netCDF Conventions
---
NCAR-RAF netCDF Conventions
===========================

* * *

Table of Contents
-----------------

*   [Introduction](#introduction)

*   [NetCDF Files and Components](#file)

*   1  [Filename](#1-filenames)
*   2  [Data types](#2-data-types)
*   3  [Naming conventions](#3-naming-conventions)
*   4  [Dimensions](#4-dimensions)
*   5  [Variables](#5-variables)
*   6  [Attributes](#6-attributes)

Appendices
----------

*   A  [References]({{ site.baseurl }}/references/)
*   B  [Size-distribution]({{ site.baseurl }}/size-distribution/)

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

All our data are stored using single precision float.  This is not a requirement.  The modern netCDF APIs have functions that will return the data in whatever format you ask, regardless of how it is stored in the file.

### 3 Naming conventions

Variable, dimension and attribute names should begin with a letter and be composed of letters, digits, and underscores.

#### 3.1 Variable names

The **underscore** has special meaning for variable names. Anything following an underscore is considered a suffix for grouping variables from the same instrument or probe. There is no standard for suffix names, but RAF regularly uses about a dozen specific ones for describing an instrument's mounting location on the aircraft.  This allows for multiple identical probes, each must have a unique sufffix.

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
                :program = "NSF NCAR" ;
                :institution = "National Center for Atmospheric Research" ;
                :Address = "P.O. Box 3000, Boulder, CO 80307-3000" ;
                :Phone = "(303) 497-1030" ;
                :source = "airborne observations" ;
                :platform = "N130AR" ;
                :platform_type = "aircraft" ;
                :project = "CAESAR" ;
                :creator_name = "NCAR EOL - Research Aviation Facility" ;
                :creator_email = "raf-pm at ucar.edu" ;
                :creator_url = "https://www.eol.ucar.edu/who-we-are/eol-organization/research-aviation-facility-raf" ;
                :creator_type = "group" ;
                :creator_group = "NSF NCAR C130 Team" ;
                :publisher_name = "UCAR NCAR - Earth Observing Laboratory" ;
                :publisher_url = "https://www.eol.ucar.edu/data-software/eol-field-data-archive" ;
                :publisher_email = "datahelp at eol.ucar.edu" ;
                :publisher_type = "group" ;
                :DOI = "https://doi.org/10.26023/44NW-3RPN-ZF0B" ;
                :naming_authority = "org.doi" ;
                :id = "10.26023/44NW-3RPN-ZF0B" ;
                :Conventions = "NCAR-RAF/nimbus-2.0,ACDD-1.3" ;
                :ConventionsURL = "https://www.eol.ucar.edu/raf/Software/netCDF.html" ;
                :standard_name_vocabulary = "CF-1.11" ;
                :ProcessorRepositoryURL = "https://github.com/NCAR/nimbus" ;
                :ProcessorRepositoryBranch = "master" ;
                :ProcessorRepositoryRevision = "v5.0-282" ;
                :ProcessorRepositoryDate = "Tue Feb 4 14:29:22 2025 -0700" ;
                :ProcessorRepositoryHASH = "b8fbb30ae4e66cc67f05fc036e0e325808da26d4" ;
                :ProjectDirectoryRepoURL = "git@github.com:NCAR/aircraft_projects.git" ;
                :ProjectDirectoryRevision = "894cf75dc2d4976e715de059206cccae1283398" ;
                :NIDASrevision = "v1.2.4" ;
                :SizeDistributionLegacyZeroBin = "no" ;
                :date_created = "2025-02-11T22:19:21 +0000" ;
                :FlightNumber = "rf01" ;
                :FlightDate = "02/28/2024" ;
                :InterpolationMethod = "Linear" ;
                :WARNING = "This file contains PRELIMINARY DATA that are NOT to be used for critical analysis." ;
                :latitude_coordinate = "LATC" ;
                :longitude_coordinate = "LONC" ;
                :zaxis_coordinate = "GGALT" ;
                :time_coordinate = "Time" ;
                :geospatial_lat_min = 67.73269f ;
                :geospatial_lat_max = 71.56526f ;
                :geospatial_lon_min = -17.13059f ;
                :geospatial_lon_max = 20.60835f ;
                :geospatial_vertical_min = 169.6806f ;
                :geospatial_vertical_max = 5996.425f ;
                :geospatial_vertical_positive = "up" ;
                :geospatial_vertical_units = "m" ;
                :wind_field = "WSC WDC WIX" ;
                :landmarks = "39.9088 -105.117 jeffco, 67.85572 20.22513 kiruna" ;
                :Categories = "Aerosol,Atmos. State,Chemistry,CloudAndPrecip,Housekeeping,NavAttitude,NavPosition,NavVelocity,Non-Standard,Radiation,Thermodynamic,Uncorrected Raw,Wind" ;
                :time_coverage_start = "2024-02-28T11:39:00 +0000" ;
                :time_coverage_end = "2024-02-28T18:36:44 +0000" ;
```

Global attributes description.  Blank descriptions can be found at the [ESIP ACDD](https://wiki.esipfed.org/Data_Discovery_(ACDD)) web page.

| Attribute Name | Category | Description |
|-----|-----|-----|
|program | data discovery - source | |
|institution | data discovery - source | |
|Address | data discovery - source | | 
|Phone | data discovery - source | |
|source | data discovery - source | |
|creator_name | data discovery - source | |
|creator_email | data discovery - source | |
|creator_url | data discovery - source | |
|creator_type | data discovery - source | |
|creator_group | data discovery - source | |
|publisher_name | data discovery - source | |
|publisher_url | data discovery - source | |
|publisher_email | data discovery - source | |
|publisher_type | data discovery - source | |
|DOI | data discovery - id | Complete DOI name, consisting of the components below (naming authority and id). |
|naming_authority | data discovery - id | | 
|id | data discovery - id | | 
|Conventions | data discovery - file format | List of conventions used. |
|ConventionsURL | data discovery - file format | |
|standard_name_vocabulary | data discovery - file format | |
|ProcessorRepositoryURL | provenance | Processor code repository, derived calculations. |
|ProcessorRepositoryBranch | provenance | |
|ProcessorRepositoryRevision | provenance | |
|ProcessorRepositoryDate | provenance | |
|ProcessorRepositoryHASH | provenance | |
|ProcessorRepositoryDirt | provenance | Lists processor source code files that were modified and not committed. |
|ProjectDirectoryRepoURL | provenance | Project configuration files repository. |
|ProjectDirectoryRevision | provenance | |
|ProjectDirectoryDirt | provenance | Lists files that are modified and not committed in the repository. |
|NIDASrevision | provenance | Processor code repository, raw data decoding processing. |
|SizeDistributionLegacyZeroBin | provenance | |
|InterpolationMethod | provenance | |
|date_created | data discovery - about | |
|platform | data discovery - about | |
|platform_type | data discovery - about | |
|project | data discovery - about | |
|FlightNumber | data discovery - about | |
|FlightDate | data discovery - about | |
|time_coverage_start | data discovery - about | |
|time_coverage_end | data discovery - about | |
|latitude_coordinate | data discovery - about | Variable name of platform best latitude |
|longitude_coordinate | data discovery - about | Variable name of platform best longitude |
|zaxis_coordinate | data discovery - about | Variable name of platform best altitude |
|time_coordinate | data discovery - about | Variable name of the time variable. |
|geospatial_lat_min | data discovery - about | |
|geospatial_lat_max | data discovery - about | |
|geospatial_lon_min | data discovery - about | |
|geospatial_lon_max | data discovery - about | |
|geospatial_vertical_min | data discovery - about | |
|geospatial_vertical_max | data discovery - about | |
|geospatial_vertical_positive | data discovery - about | |
|geospatial_vertical_units | data discovery - about | |
|wind_field | data discovery - about | List of three dimensional wind variables.  wind_speed wind_direction vertical_wind |
|landmarks | ancillary information | Comma separated list of land marks.  lat lon label |
|Categories | ancillary information | Most variables have a Category attribute, this is the list of all possible categories. |


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