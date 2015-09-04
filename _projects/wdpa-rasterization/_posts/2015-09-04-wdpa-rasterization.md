---
layout: post
title: "Rasterizing WDPA in parts"
date: "2015-09-04"
tags:
 - wdpa-rasterization
---

## 2015-09-04

+ The rasterization process has bene divided to `MRGTESLA` and `arnold`
roughly based on hemispheres. (see code [here](https://github.com/cbig/gpan-connectivity/blob/master/src/01_preprocess/test_multiprocessing.py))
`arnold` finished western hemisphere in about 4.5 day, `MRGTESLA` is still
working. Started a new quarter world run on `arnold`:

start-time: 2015-09-04 13:52  
end-time: YYYY-MM-DD HH:MM  
machine: `arnold`  
tool/script: test_multiprocessing.py  
parameters:   [8a2bf4cd22764c990c291439c0ae11b9100adcf5](https://github.com/cbig/gpan-connectivity/commit/8a2bf4cd22764c990c291439c0ae11b9100adcf5#diff-d9f33b47568dc296115e28cb804573eb)
