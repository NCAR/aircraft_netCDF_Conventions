---
layout: default
title: Home
nav_order: 1
description: "NCAR-RAF netCDF Conventions for airborne atmospheric research data - Version 2.1"
permalink: /
---

# NCAR-RAF netCDF Conventions
{: .no_toc }

### Version 2.1
{: .no_toc }

* * *

#### National Center for Atmospheric Research
{: .no_toc }

#### Research Aviation Facility
{: .no_toc }

#### NOAA AOC
{: .no_toc }

#### University of Wyoming (King Air)
{: .no_toc }

* * *

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

* * *

## Changelog

### 2.1 released Jan 2025

*   Coordinate variables for size-distributions
*   Updated global attributes for data discovery and provenance.

### 2.0 released Jun 2021

*   The primary difference is in how size-distributions are stored. The legacy zeroth bin has been removed. Now a 30 channel probe will have 30 bins, where as previously it had 31, with the zeroth (or 1st, depending on your method of numbering) bin being a fixed zero.

Check the Conventions global attribute for version number. The "NCAR-RAF/nimbus" entry is what you want. ACDD is for data discovery.

```bash
ncdump -h CAESARrf01.nc | grep Conventions
        :Conventions = "NCAR-RAF/nimbus-2.0,ACDD-1.3" ;
```

For files produced prior to 2021, visit **[RAF netCDF Conventions 1.3](netCDF_1_3.html)**.

* * *

## Introduction

The goal is to support synchronous time series including the following properties:

*   Both scalar and histogram variables.
*   Different sample rates among variables

RAF's primary datasets consist of synchronous time series data. Data are written to the netCDF file one record per second, even for data at higher rates.

### Histogram data

Instruments which measure particle size distributions produce histograms. These instruments include the suite of aerosol and cloud probes.

### Multi-rate files

RAF's standard data product is a netCDF file where all measurements are output at 1 sps. Certain projects require high-rate data. RAF has standardized at 25 sps for high-rate data. In general we don't want to put out variables sampled at 1 sps at 25 sps, as there is no information in the data, and it would take up excess space.

There is also an occasional need to look at data at the sample rate.

![netCDF record structure](http://www.eol.ucar.edu/raf/Software/record.png)

* * *

## Navigation

- [File Structure]({% link file-structure.md %}) - Filenames, data types, naming conventions, dimensions, and variables
- [Attributes]({% link attributes.md %}) - Global and variable attributes specifications
- [Size-Distribution Probes]({% link size-distributions.md %}) - Histogram variable descriptions (Appendix B)
- [References]({% link references.md %}) - Links to NetCDF, CF Conventions, ACDD, and UDUNITS (Appendix A)
- [Changelog]({% link changelog.md %}) - Complete version history

* * *

## Contact

For further information contact:

Chris Webster
National Center for Atmospheric Research
Research Aviation Facility
(303) 497-1044

[email](mailto:cjw@ucar.edu?Subject=from_RAF_Web_Software_Page_netCDF)
