---
layout: post
title: "Rasterizing WDPA in parts"
date: "2015-09-04"
tags:
 - wdpa-rasterization
---

## 2015-09-04 Friday

+ The rasterization process has bene divided to `MRGTESLA` and `arnold`
roughly based on hemispheres. (see code [here](https://github.com/cbig/gpan-connectivity/blob/master/src/01_preprocess/test_multiprocessing.py))
`arnold` finished western hemisphere in about 4.5 day, `MRGTESLA` is still
working. Started a new quarter world run on `arnold`:

start-time: 2015-09-04 13:52  
end-time: ABORTED  
machine: `arnold`  
tool/script: test_multiprocessing.py  
parameters:   [8a2bf4cd22764c990c291439c0ae11b9100adcf5](https://github.com/cbig/gpan-connectivity/commit/8a2bf4cd22764c990c291439c0ae11b9100adcf5#diff-d9f33b47568dc296115e28cb804573eb)

## 2015-09-07 Monday

+ Rasterization run on `MRGTESLA` had finnished producing what looked like a valid
output, so the run started in the previous section on `arnold` was cancelled.

+ Outputs on the respective computers were renamed as follows:  
  `MRGTESLA`: `wdpa_mask_eastern.tif`  
  `arnold`: `wdpa_mask_western.tif`   

+ Metadata for these rasters (`gdalinfo`):

```
Driver: GTiff/GeoTIFF
Files: wdpa_mask_eastern.tif
Size is 10801, 9001
Coordinate System is:
GEOGCS["WGS 84",
    DATUM["WGS_1984",
        SPHEROID["WGS 84",6378137,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0],
    UNIT["degree",0.0174532925199433],
    AUTHORITY["EPSG","4326"]]
Origin = (0.000000000000000,90.000000000000000)
Pixel Size = (0.016666666666667,-0.016666666666667)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  COMPRESSION=DEFLATE
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (   0.0000000,  90.0000000) (  0d 0' 0.01"E, 90d 0' 0.00"N)
Lower Left  (   0.0000000, -60.0166667) (  0d 0' 0.01"E, 60d 1' 0.00"S)
Upper Right ( 180.0166667,  90.0000000) (180d 1' 0.00"E, 90d 0' 0.00"N)
Lower Right ( 180.0166667, -60.0166667) (180d 1' 0.00"E, 60d 1' 0.00"S)
Center      (  90.0083333,  14.9916667) ( 90d 0'30.00"E, 14d59'30.00"N)
Band 1 Block=10801x1 Type=Int32, ColorInterp=Gray
  NoData Value=-9999
```

```
Driver: GTiff/GeoTIFF
Files: wdpa_mask_western.tif
Size is 10801, 10801
Coordinate System is:
GEOGCS["WGS 84",
    DATUM["WGS_1984",
        SPHEROID["WGS 84",6378137,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0],
    UNIT["degree",0.0174532925199433],
    AUTHORITY["EPSG","4326"]]
Origin = (-180.000000000000000,90.000000000000000)
Pixel Size = (0.016666666666667,-0.016666666666667)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  COMPRESSION=DEFLATE
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (-180.0000000,  90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"N)
Lower Left  (-180.0000000, -90.0166667) (180d 0' 0.00"W, 90d 1' 0.00"S)
Upper Right (   0.0166667,  90.0000000) (  0d 1' 0.00"E, 90d 0' 0.00"N)
Lower Right (   0.0166667, -90.0166667) (  0d 1' 0.00"E, 90d 1' 0.00"S)
Center      ( -89.9916667,  -0.0083333) ( 89d59'30.00"W,  0d 0'30.00"S)
Band 1 Block=10801x1 Type=Int32, ColorInterp=Gray
  NoData Value=-9999

```

+ **Note** that the extents corresponds to the actual extents of the data. This has to be accounted for in merging the two rasters.

+ Both outputs were manually copied over to `gpan-connectivity/data/WDPA`.

+ Rasters `wdpa_mask_eastern.tif` and `wdpa_mask_western.tif` were merged
together:

  computer: `arnold`  
  tool: `gdal_merge.py (1.10.0)`  
  command:  
  `gdal_merge.py -ul_lr -180.0 90 180.0 -90.0 -a_nodata -9999 -o wdpa_mask_nocomp.tif wdpa_mask_eastern.tif wdpa_mask_western.tif`

  ```
  Driver: GTiff/GeoTIFF
Files: wdpa_mask_nocomp.tif
Size is 21600, 10800
Coordinate System is:
GEOGCS["WGS 84",
    DATUM["WGS_1984",
        SPHEROID["WGS 84",6378137,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0],
    UNIT["degree",0.0174532925199433],
    AUTHORITY["EPSG","4326"]]
Origin = (-180.000000000000000,90.000000000000000)
Pixel Size = (0.016666666666667,-0.016666666666667)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (-180.0000000,  90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"N)
Lower Left  (-180.0000000, -90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"S)
Upper Right ( 180.0000000,  90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"N)
Lower Right ( 180.0000000, -90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"S)
Center      (   0.0000000,   0.0000000) (  0d 0' 0.01"E,  0d 0' 0.01"N)
Band 1 Block=21600x1 Type=Int32, ColorInterp=Gray
  NoData Value=-9999
  ```
+ Compressed the raster:

computer: `arnold`  
tool: `gdal_translate (1.10.0)`  
command:  
`gdal_translate -co "COMPRESS=DEFLATE" wdpa_mask_nocomp.tif wdpa_mask.tif`

```
Driver: GTiff/GeoTIFF
Files: wdpa_mask.tif
       wdpa_mask.tif.aux.xml
Size is 21600, 10800
Coordinate System is:
GEOGCS["WGS 84",
    DATUM["WGS_1984",
        SPHEROID["WGS 84",6378137,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0],
    UNIT["degree",0.0174532925199433],
    AUTHORITY["EPSG","4326"]]
Origin = (-180.000000000000000,90.000000000000000)
Pixel Size = (0.016666666666667,-0.016666666666667)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  COMPRESSION=DEFLATE
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (-180.0000000,  90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"N)
Lower Left  (-180.0000000, -90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"S)
Upper Right ( 180.0000000,  90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"N)
Lower Right ( 180.0000000, -90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"S)
Center      (   0.0000000,   0.0000000) (  0d 0' 0.01"E,  0d 0' 0.01"N)
Band 1 Block=21600x1 Type=Int32, ColorInterp=Gray
  Min=-9999.000 Max=555592835.000
  Minimum=-9999.000, Maximum=555592835.000, Mean=3805570.612, StdDev=45785463.920
  NoData Value=-9999
  Metadata:
    STATISTICS_MAXIMUM=555592835
    STATISTICS_MEAN=3805570.6117941
    STATISTICS_MINIMUM=-9999
    STATISTICS_STDDEV=45785463.919539
```

+ Removed raster `wdpa_mask_nocomp.tif`
