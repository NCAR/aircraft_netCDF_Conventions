---
layout: default
title: Changelog
nav_order: 6
description: "Version history for NCAR-RAF netCDF Conventions"
---

# Changelog

## 2.1 released Jan 2025

*   Coordinate variables for size-distributions
*   Updated global attributes for data discovery and provenance.

## 2.0 released Jun 2021

*   The primary difference is in how size-distributions are stored. The legacy zeroth bin has been removed. Now a 30 channel probe will have 30 bins, where as previously it had 31, with the zeroth (or 1st, depending on your method of numbering) bin being a fixed zero.

## Checking File Version

Check the Conventions global attribute for version number. The "NCAR-RAF/nimbus" entry is what you want. ACDD is for data discovery.

```bash
ncdump -h CAESARrf01.nc | grep Conventions
        :Conventions = "NCAR-RAF/nimbus-2.0,ACDD-1.3" ;
```

## Legacy Documentation

For files produced prior to 2021, visit **[RAF netCDF Conventions 1.3](netCDF_1_3.html)**.
