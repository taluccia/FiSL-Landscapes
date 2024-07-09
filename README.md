# FiSL-Landscapes


# Overview

The repository host processes and code for producing combustion Landscape layers that feed into the iLand Model for FiSL. Combustion predictions are based on models produced by Stefano Potter for the ABOVEFED project. 

The process is dependent on a combination of Earth Engine (EE) Processes and R

# Process

| Step    | Script                           | plateform  | Input files        | output files                |
| ------- |:------------------------------:  | ----------:| ------------------:| ---------------------------:|
| 1       | OrganizeShpData.Rmd              |  R         | landscape, fire perimeters, ecozones| polygon shp file of landscapes|
| 2       | LandsatImageForPoints            |  EE        | Landscape polygons | Point shp file for pixel centroid|
| 3       | LandscapeBurnedArea.Rmd          |  R         | Landscapes, fires  |                             |
| 4       | RasterToPoint.Rmd                |  R         | Landsat Rasters    | Point shp by Ecozone        |
| 5       | BurnDate                         |  EE        | point shp from #4  | csv of pixel values at point |
| 6       | DoYToCalendar.Rmd                |  R         | point shp from #4  | csv of pixel values at point |
| 7       | BurnDateFWIExtract               |  EE        | point shp from #5  | csv of pixel values at point |
| 8       | LandsatVegetationIndicesExtract  |  EE        | point shp from #4  | csv of pixel values at point |
| 9       | LandsatTCTExtract                |  EE        | point shp from #4  | csv of pixel values at point |
| 10      | StaticVegetation                 |  EE        | point shp from #4  | csv of pixel values at point |
| 11      | TreeCoverExtract                 |  EE        | point shp from #4  | csv of pixel values at point |
| 12      | StaticTerrain                    |  EE        | point shp from #4  | csv of pixel values at point |
| 13      | StaticSoil                       |  EE        | point shp from #4  | csv of pixel values at point |
| 14      | StaticPFI                        |  EE        | point shp from #4  | csv of pixel values at point |
| 15      | CombineData4CombustModel.Rmd     |  R         | csv from #7-14     | csv of combined extracted data|
| 16      | CombustionModel.Rmd              |  R         | csv #15, models, training and variable importance data | above and below combustion csv |
| 16      | PredictToRaster.Rmd              |  R         |  csv from #16      | ratsers above & below combustion | 



Step 1 -- R--Run OrganizeShpData.Rmd -- combine canadian fire perimeters, filter for AK fires from all USA

Step 2 -- EE--[LandsatImageForPoints](https://code.earthengine.google.com/b41f7dc26776eefdfad611cf7b1621bb) export as equal area with 30m resolution 

Step 3 -- R--Run LandscapeBurnedArea.Rmd ---creates burned area polygon for within the landscape polygon, and selects the perimeters that over lap with the landscapes (full perimeters are needed in EE Day of Burn)

Step 4 -- R--RasterToPoint.Rmd -> input is Output from step 1 & 3 -> output is point shapefile for burned areas within Landscape

Step 5 -- EE-- [BurnDate](https://code.earthengine.google.com/11f45f5bcbd96f844d1aa5a9e8efd4eb) Calculated extracts Day of Burn

Step 6 -- R -- DoYToCalendar.Rmd

Step 7 -- EE-- [BurnDateFWIExtract](https://code.earthengine.google.com/ba536bcf867f8fa3f2104c973b7c4a4d)

Step 8 -- EE-- [LandsatVegetationIndicesExtract](https://code.earthengine.google.com/04cb069e551f842ef2969f5e8aee0f4d) Extract Used burned points to extract vegetation indices; code updated to Collection 2

Step 9 -- EE-- [LandsatTCTExtract](https://code.earthengine.google.com/cc165b119dadb6dfeaa96226494ff904) Calculated Tassel Cap and extracts to points; code updated to Collection 2

Step 10 -- EE-- [StaticVegetation](https://code.earthengine.google.com/6d261d28b8725e9237a46a33c9fa8f5c) This is species and land cover type.

Step 11 -- EE-- [TreeCoverExtract](https://code.earthengine.google.com/61ce516ecb0f345635ee378ee1680e5f) 

Step 12 -- EE-- [StaticTerrain](https://code.earthengine.google.com/dddff15fa3746a60260bb433acaf59fc) Extract Used burned points to extract static PFI soil variable 

Step 13 -- EE-- [StaticSoil](https://code.earthengine.google.com/058b3229f4aac786dca693a3d89f15bb) Extract Used burned points to extract static soil characteristics 

Step 14 -- EE-- [StaticPFI](https://code.earthengine.google.com/c29c51592040d66b4250a9434a3e8659) Extract Used burned points to extract static PFI soil variable 

Step 15 -- R -- CombineData4CombustModel.Rmd Take outputs from Steps 7-14 and combine for combustion model

Step 16 -- R -- CombustionModel.Rmd  

Step 17 -- R -- PredictToRaster.Rmd  


# Final Raster product

1. Above ground Combustion produced in `PredictToRaster.Rmd` 
2. Below ground combustion produced in `PredictToRaster.Rmd` 

#  Shapefiles data 

[Canadian Provinces](https://open.canada.ca/data/en/dataset/a883eb14-0c0e-45c4-b8c4-b54c4a819edb)
[Alaska State Boundary](https://www.sciencebase.gov/catalog/item/59d5b565e4b05fe04cc53a91)

[Alaska Fire perimeters from MTBS](https://www.mtbs.gov/direct-download)
[Canada Fire perimeters](https://cwfis.cfs.nrcan.gc.ca/datamart/download/nbac)

# Notes


# Tasseled cap Notes

For coefficients for Landsat 8 TOA see --- Muhammad Hasan Ali Baig, Lifu Zhang, Tong Shuai & Qingxi Tong (2014) Derivation of a tasselled cap transformation based on Landsat 8 at-satellite reflectance, Remote Sensing Letters, 5:5, 423-431, DOI: 10.1080/2150704X.2014.915434

[Coeffcients](https://gis.stackexchange.com/questions/156161/tasseled-cap-transformation-coefficient-and-bias-value)

## Combustion with ABoVE-FED

Three scripts from Stefano; all of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized. 
1) static_extract. This will extract all 'static variables' such as soils, topographic variables, and vegetation (we considered it static at the time).   https://code.earthengine.google.com/1e1758b0c628edf79aaa61bb36ea4448
2) landsat_extract.  This will extract NDII, NDVI, dNBR, tasseled cap, tree cover, and some other burn indices.  https://code.earthengine.google.com/7f1bf046ce88cca8f8109d8b9f3e2693
3) FWI extract.  This will extract the fire weather information for all burned pixels by using the Day of Burn information. https://code.earthengine.google.com/b9e024b906580e32cf1c8718b3b8df11


All of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized.  


# ABoVE FED Data

[SK Combustion](https://daac.ornl.gov/cgi-bin/dsviewer.pl?ds_id=1740)

[AK & Canada Combustion](https://daac.ornl.gov/cgi-bin/dsviewer.pl?ds_id=2063)
