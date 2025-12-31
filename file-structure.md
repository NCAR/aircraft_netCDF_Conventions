---
layout: default
title: File Structure
nav_order: 2
description: "NetCDF file components: filenames, data types, naming conventions, dimensions, and variables"
---

# NetCDF File Component Descriptions
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

* * *

## 1 Filenames

RAF uses a naming convention consisting of a project designator followed by the flight type, followed by a 2 digit flight number, followed by an indicator of sample rate ( 'h' to indicate high-rate data, low-rate data has no indicator), followed by ".nc". Prior to about 2005/2006, field projects were designated with a 3 digit number. With the adoption of ADS-3 / nidas in 2005/2006, a switch was made to upper-case project names.

e.g.

PREDICTtf01.nc would be low-rate data from **test** flight 1 from the PREDICT project.
SPICULEff01s.nc would be sample-rate data from **ferry** flight 1 from the SPICULE project.
RICOrf08h.nc would be high-rate data from **research** flight 8 of the RICO project.

Flight types used by RAF are "ff", "tf", and "rf".

## 2 Data types

All our data are stored using single precision float.  This is not a requirement.  The modern netCDF APIs have functions that will return the data in whatever format you ask, regardless of how it is stored in the file.

## 3 Naming conventions

Variable, dimension and attribute names should begin with a letter and be composed of letters, digits, and underscores.

### 3.1 Variable names

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

### 3.2 Dimension names

Dimension names we use are:

*   Time This is the UNLIMITED dimension for the number of rows or seconds in the file.
*   sps# for variable data rate (sps1, sps5, sps25, sps1000, etc.)
*   histogram# for histogram lengths (histogram16, histogram30, etc.)
*   bnds size is 2, this for coordinate variable bounds.
*   Probe Serial Numbers from coordinate variables will be used (e.g. CDP016, UHSAS011, F2DS020).

## 4 Dimensions

### 4.1 First dimension - Time

**Time** is the **unlimited dimension**, and most every variable has the **Time** dimension. No more than one record is to be written per second; all subsamples are to be contained in the record, though data averaged down slower than 1 sps may be written (e.g., when you have 1 sample every 10 seconds).

### 4.2 Second dimension - data rate

The second dimension, if present, represents the number of samples per second. The size of this dimension will vary according to the output rate of the variable (e.g., RAF uses 1, 5, 10, 25, 50, 250, and 1000). If the dimension does not exist, then there is only one sample for the second.

### 4.3 Third dimension - histogram length

The third dimension, if present, signifies a histogram and is the length of the array. (This will be present only for array data).

## 5 Variables

http://www.cfconventions.org/.

### 5.1 Time

Time follows the recomendations set forth in the [CF conventions](http://cfconventions.org/latest.html) section 4.4. This always has one dimension, the Time dimension. It is assumed that subsamples, dimension two in a variable, are evenly spaced. Data that are averaged down to 1hz, the timestamp is the mid-point of the second (i.e. it is the average of all the data that was acquired during that second).
```cdl
        int Time(Time) ;
                Time:long_name = "time of measurement" ;
                Time:standard_name = "time" ;
                Time:units = "seconds since 2004-08-02 00:00:00 +0000" ;
                Time:strptime_format = "seconds since %F %T %z" ;
```
RAF sets the Time:units to midnight prior to take-off and then the values of Time are seconds since midnight. This helps with correlation of NASA Ames and ICARTT data formats.

### 5.2 Missing data

At this time we use the _FillValue attribute.
