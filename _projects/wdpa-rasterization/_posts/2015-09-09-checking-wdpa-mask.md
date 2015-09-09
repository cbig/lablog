---
layout: post
title: "Checking the WDPA mask"
date: "2015-09-09"
tags:
 - gpan-connectivity
 - wdpa-rasterization
---

+ Peter manually checked whether the decision rule implemented in the rasterization process had actually worked and it seems to have. Results are consistent with the `WDPA_June2015-shapefile-polygons.shp` data.

+ We also noted, that ArcGIS has troubles in drawing both the original `WDPA_June2015-shapefile-polygons.shp` as well as the derived `WDPA_June2015-shapefile-polygons_nomarine.shp` (marine PAs filtered out). QGIS has not trouble in drawing the same shapefile. This implies some sort of geometry errors, but at this point it's very difficult to say which and we decided not to go for fixing the geometries.

+ Also worth noting, that during the rasterization there are several empty or missing geometries in the data that were simply omitted. The identity of these records was not stored.

+ `WDPA_mask.tif` is now in gpan-[connectivity](https://github.com/cbig/gpan-connectivity/tree/master/data/WDPA) repo. Also added more detailed [README](https://github.com/cbig/gpan-connectivity/blob/master/data/WDPA/wdpa_mask_README.md) and [metadata](https://github.com/cbig/gpan-connectivity/blob/master/data/WDPA/wdpa_mask_meta.json).
