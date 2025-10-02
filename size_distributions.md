---
layout: default
title: Size-distribution Probe Variables Description
permalink: /size-distribution/
---
## Size-distribution Probe Variables Description
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
