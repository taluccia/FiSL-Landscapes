# FiSL-Landscapes


# Overview

The repository host processes and code for producing Landscape layers that feed into the iLand Model for FiSL.

The process is dependent on a combination of Earth Engine (EE) Processes and R

# Process
Step 1 -- EE--LandsatImageForPoints
Step 2 -- R--Run OrganizeShpData.Rmd --
       -- R--Get fire polygonsthat overlap landscapes -> needed in EE Day of Burn
Step 3 -- R--Run LandscapeBurnedArea.Rmd
Step 4 -- R--RasterToPoint.Rmd -> input is Output from step 1 & 3 -> output is point shapefile for burned areas within Landscape
Step 5 -- EE--Used burned points to extract static variables






*  EE Acquire DEMs
*  EE Static Variable extract
*  EE Landsat extract
*  EE Day of Burn
*  EE FWI


# Final Raster product
1. Above ground Combustion
2. Below ground combustion

#  Shapefiles data 
[Canadian Provinces](https://open.canada.ca/data/en/dataset/a883eb14-0c0e-45c4-b8c4-b54c4a819edb)
[Alaska State Boundary](https://www.sciencebase.gov/catalog/item/59d5b565e4b05fe04cc53a91)

[Alaska Fire perimeters from MTBS](https://www.mtbs.gov/direct-download)
[Canada Fire perimeters](https://cwfis.cfs.nrcan.gc.ca/datamart/download/nbac)
# Notes

## Combustion with ABoVE-FED
Three scripts from Stefano; all of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized. 
1) static_extract. This will extract all 'static variables' such as soils, topographic variables, and vegetation (we considered it static at the time).   https://code.earthengine.google.com/1e1758b0c628edf79aaa61bb36ea4448
2) landsat_extract.  This will extract NDII, NDVI, dNBR, tasseled cap, tree cover, and some other burn indices.  https://code.earthengine.google.com/7f1bf046ce88cca8f8109d8b9f3e2693
3) FWI extract.  This will extract the fire weather information for all burned pixels by using the Day of Burn information. https://code.earthengine.google.com/b9e024b906580e32cf1c8718b3b8df11



All of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized.  
