# Soil grids data access package

This package makes working with data from the [ISRIC Soil Grids](https://soilgrids.org/#!/?layer=ORCDRC_M_sl2_250m&vector=1) data set easier. One Acre Fund collects soil data for analysis in our own lab however we lack the coverage to describe soils in our program area broadly or in new places where we don't yet work.

This package simplifies extracting soil values for given GPS points from soil raster layers from Soil Grids.

# How to install

To install this package, please do the following:

1. You'll need to create a personal access token (PAT) on github to download the package from github.
  + Go to the [tokens](https://github.com/settings/tokens) page on github to create a token. Be sure to select the `repo` scope so that it works!
  + Then, you'll need to edit the `.Renviron` file on your computer to make that PAT available to R when it tries to download the package.
  + To do this, type `usethis::edit_r_environ()` in R to edit the `.Renviron` file.
  + In the `.Renviron` file type `GITHUB_PAT = "longstringoflettersandnumbersyougetfromgithub"`. (What goes in the quotes is the PAT you got from GitHub). Then save and close that file. You might have to restart R for the changes to take effect.
2. Open R script and download the package using `devtools::install_github(one-acre-fund/arc2weather)`. (You can check that the GITHUB_PAT is working as it should by first typing `Sys.getenv("GITHUB_PAT")` in the R console.)
3. The package should now be available for your use!


# How to use

The primary function will be `get_soil_data()` which takes four inputs:

* `spdf` - a SpatialPointsDataFrame that includes the GPS points of interest
* `country_iso` - the country ISO code for the country in which the points are located. This allows the package to trim down the data and simplify the amount of data we're working with. Right now you can only access points from one country at a time but future releases will expand on this. 
* `soil_layers` - The soil data you're interested in have. Right now you can access any of ph, carbon, soil_texture, sand, silt, clay, or CEC. I'll adding more soil chemistry and texture layers soon.
* `raw_data_directory` - the file location on your local computer of the raw soil data downloaded from Soil Grids. This component is engineered for 1AF's current set up and will likely be a point of improvement in future versions.
* `country_polygon_directory` - the file location with the country polygons. These polygons are used to crop the soil data so that we're passing smaller datasets behind the scenes before extracting the data for the GPS points.

Finally, the package currently assumes you have all the right data downloaded locally. This works for 1AF since we can share Google Drive folders with the [raw data](https://drive.google.com/open?id=1piqHGLXffirXQAa4oSFTUcjieQY8tud8) and the [country polygons](https://drive.google.com/open?id=1bXO74V5c4URUqtkPVeyABywjpfmFW2Mx).

Here's an example call of the main function to extract data for Kenya GPS points:

~~~~

# first, make sure you install the `soilgrids` package following the instructions above.
library(soilgrids)

soil_data <- get_soil_data(spdf = siteSp, 
                          country_iso = "KEN", 
                          soil_layers = c("ph", "carbon"), 
                          raw_data_directory = "/Users/mlowes/Google Drive/analyses/soil_grids_raw_data/", 
                          country_polygon_directory = "/Users/mlowes/Google Drive/analyses/soil_grids_data/")

# where siteSp is a SpatialPointsDataFrame of GPS and associated location information. 

~~~~
