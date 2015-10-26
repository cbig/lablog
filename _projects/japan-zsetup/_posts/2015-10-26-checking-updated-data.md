---
layout: post
title: "Checking the updated Japanese data"
date: "2015-10-25"
tags:
 - japan-zsetup
---
+ Comparing the new updated plant data in `project.J-PriPA/Data.150928/plant`
to the old data in `project.J-PriPA/japan-zsetup/data/Species_distributions`
with `gdalinfo`:
  + Both compressed with DEFLATE
  + New uses NoData value -3.39999999999999996e+38 vs -1 in the old
  + E.g. `abelia_chinensis.tif` 159.6 kB in the old, 1.7 MB in the new ->
  something funny with the compression  

+ Delete all the old files (N=5585) in folder
`project.J-PriPA/japan-zsetup/data/Species_distributions`:

  machine: `LH-BIOTIE25`  
  tool/script: `rm *`
