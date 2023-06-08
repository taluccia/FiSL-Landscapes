# FiSL-Landscapes


# Overview

The repository host processes and code for producing Landscape layers that feed into the iLand Model for FiSL.

The process is dependent on a combination of Earth Engine (EE) Processes and R

# Process
Step 1 -- Run OrganizeShpData.Rmd --
Step 2 -- Run LandscapeBurnedArea.Rmd
Step 3 -- Run






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
# Notes

## Combustion with ABoVE-FED
Three scripts from Stefano; all of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized. 
1) static_extract. This will extract all 'static variables' such as soils, topographic variables, and vegetation (we considered it static at the time).   https://code.earthengine.google.com/1e1758b0c628edf79aaa61bb36ea4448
2) landsat_extract.  This will extract NDII, NDVI, dNBR, tasseled cap, tree cover, and some other burn indices.  https://code.earthengine.google.com/7f1bf046ce88cca8f8109d8b9f3e2693
3) FWI extract.  This will extract the fire weather information for all burned pixels by using the Day of Burn information. https://code.earthengine.google.com/b9e024b906580e32cf1c8718b3b8df11



All of these scripts take a shapefile of burned pixel centroids as the input.  The shapefile needs to have a date of burn, a unique pixel identifier and the year of burn in order to get all the scripts to work.  All the scripts will output csv files which can then be used to predict the R models on and those predictions can be joined back into the shapefile based on each pixel's unique ID and then rasterized.  
