---
layout: default
title: Attributes
nav_order: 3
description: "Global and variable attributes specifications for NCAR-RAF netCDF files"
---

# Attributes
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

* * *

## 6.1 Global attributes
```cdl
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


## 6.2 Variable attributes

All variables contain the basic attributes units, long_name, and _FillValue (missing_value has been deprecated).

Every variable will have either SampledRate or Dependencies, this is how you can determine if it is a raw vs. derived (calculated) variable.

In addition we provide some other attributes; not all are used for every variable:

|Attribute name| | Applies to | Description |
|-----|-----|-----|-----|
| _FillValue | highly recommended | all | This acts as the missing value flag. |
| units | required | all | Self explanitory. |
| long_name | required | all | Description of teh variable. |
| standard_name | optional | all | We use CF [CF conventions](http://cfconventions.org/latest.html) section 3.3 use of standard_name where applicable. |
| SampledRate | optional | raw | The rdata date (sps) at which the variable was sampled onboard the aircraft. All variables recorded by the data acquistion system will have this attribute. All derived variables will not.|
| TimeLag | optional | raw | Present if a time lag is applied to the variable.  Negative to correct for data lag or slow response. milliseconds |
| DespikeSlope | optional | raw | If despiking is being applied, this is the slope between two consecutive sampled values.  i.e. what is a valid a max response to changing conditions. |
| DataQuality| optional | all | attempts to inform the user of the quality of these data. Some values are Bad, Preliminary and Good. Data distributed in the field will always be set to Preliminary. |
| Category | optional | all | See CategoryList in the global_attributes.  Since a file can contain hundreds of variables with not very descriptive names, this is provided to help create sub-lists of variables. |
| CalibrationCoefficients | optional | raw | These are the values used to produce engineering units from a measurement's DC voltage. It is used by the analog/digital group. **These values have already been applied!** They are present for documentation. |
| Dependencies | optional | derived | These are the input variables that were used to produce this derived (calculated) variable. Like CalibrationCoefficients, these are present for documentation. |
| actual_range | recommended | all | This contains the min/max for this variable in this file. |
| valid_range | | all | We have this for some variables.  Contains the valid range of the variable. |



For processing purposes, variables in **our** files are organized into two sorted lists. The first sorted list consists of raw sampled variables. These only have calibration coefficients applied to them. The second group consists of derived variables. I have included one example from each of the 3 groups (which also show different dimension schemes):

```cdl
        float PITCH(Time) ;
                PITCH:_FillValue = -32767.f ;
                PITCH:units = "degree" ;
                PITCH:long_name = "IRS Aircraft Pitch Angle" ;
                PITCH:standard_name = "platform_pitch_angle" ;
                PITCH:valid_range = -180.f, 180.f ;
                PITCH:actual_range = -3.31724 15.88532f ;
                PITCH:Category = "NavAttitude" ;
                PITCH:SampledRate = 50 ;
                PITCH:TimeLag = -180 ;
                PITCH:TimeLagUnits = "milliseconds" ;
                PITCH:DataQuality = "Preliminary" ;
                PITCH:CalibrationCoefficients = 0.26f, 1.f ;

        float CCDP_LWOO(Time, sps1, CDP016) ;
                CCDP_LWOO:_FillValue = -32767.f ;
                CCDP_LWOO:units = "#/cm3" ;
                CCDP_LWOO:long_name = "CDP Concentration (per cell)" ;
                CCDP_LWOO:actual_range = 0.f, 538.7375f ;
                CCDP_LWOO:Category = "CloudAndPrecip" ;
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
```
