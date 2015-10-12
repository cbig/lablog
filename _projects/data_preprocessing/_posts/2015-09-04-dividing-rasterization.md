---
layout: post
title: "Processing the modelled mammal distribution maps"
date: "2015-10-12"
tags:
 - gpan-connectivity
 - data_preprocessing
---


+ Luca Santini sent me (Peter) the modelled distribution maps of all non-volatile terrestrial mammals (Or to be precise all of which they had enough information to build the models). The models are based on Rondinini et al. 2011 (DOI: 10.1098/rstb.2011.0113).
+ The models are upscaled from the original resolution to 1 minutes by Santini. He says that “…I have resampled each 300 m pixel by sum, and multiplied it by 0.09”. Basically the cell value corresponds to amount of suitable habitat in each cell.
+ The processing is done in CSC Taito super cluster (taito.csc.fi). The original packed distribution maps can be found from /wrk/pkullber/Data/mammal\_data (Archive.zip and Archive_part2.zip).   
+ There are altogether 4149 modelled range maps. We will use only those species that also have information on dispersal distance (n=4109).
+ To compress the files (to save space as the original GeoTIFF files were uncompressed) I run a command line script `find . -name *.tif  -type f | xargs -I {} basename {} |xargs -I {} gdal_translate {} ../comp_mam_models/comp_{} -co COMPRESS=DEFLATE`. 
+ To increase the extent of all distribution maps to global, I modified the maps by running [Expand\_rasters\_TAITO.R](https://github.com/cbig/gpan-connectivity/blob/master/src/01_preprocess/Expand_rasters_TAITO.R) link to the script. Extended files are locate in /wrk/pkullber/Data/mammal\_data/exp\_mam\_models3. 
+ The original files use IUCN species IDs as names, but I renamed the files based on the scientific names. This type of naming is consistent with the previous global analyses done in the CBIG. I did the renaming with [Rename\_mammals.R](https://github.com/cbig/gpan-connectivity/blob/master/src/01_preprocess/Rename_mammals.R). Renamed files can be found from /wrk/pkullber/Data/mammal\_data/exp\_mam\_models2.
