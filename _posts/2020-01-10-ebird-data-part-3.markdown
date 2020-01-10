---
layout: post
title:  "Digging into your checklist data"
date:   2020-01-10 21:20:00 +1300
categories: nzatlas ebird data datasets
background: '/images/IMG_8459.JPG'
---

In this post, I want to talk about how to use the R Scripting language to process the downloaded eBird data, for the purposes of understanding checklist effort and habitat coverage, by season.

This post is going to be a little code heavy, but the upshot is that the checklist data is used to create a a map that shows what habitats still need checklists within a grid square. This map doesn't mean that the habitats with checklist have been completely counted - it's meant to demonstrate what is left, at a minimum.

The following image shows such a grid square, labeled **BM72**, showing checklist locations (as yellow dots) and habitat locations (a red-coloured, semi-transparent, overlay + labels) that have yet to have checklists collected.

![Grid Square BM72]({{site.url}}/images/nzatlas-20200106-grid-square.PNG "Grid Square")

This image is one of many, produced as a larger map book, that allows us to rapidly review effort across each grid square. For those of you familiar with QGIS, I will describe the process to build the map book in a later post.

The code that follows assumes a basic knowledge of R, but failing that, the comments within the code should provide enough signposts along the way to tell you what's going on.

The steps taken are as follows:
1. Download own checklist data from ebird (manual request process through the ebird website - generates an email with a link to a zip file). If you are collecting data with a friend or partner, you can ask them for their data at this point and then combine your checklists together. For people coordinating effort, this may have some benefit.
2. Based on checklist locations, nzatlas grid squares are selected to be overlayed with landcover data.
3. NZ Atlas grid squares are overlaid onto the landcover database (v4.1).
4. Data are saved for mapping in QGIS


```r
# R packages required
list.of.packages <- c("rgdal", "sp","spatialEco","raster","rgeos","rpostgis","dplyr","geojsonio")
new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)

require(rgdal)
require(sp)
require(spatialEco)
require(geojsonio)
require(raster)
require(rgeos)
require(rpostgis)
require(RPostgreSQL)
require(dplyr)

# Working directory
setwd("/Users/me/R/ebird")

# All spatial data will be projected to the NZTM. The string that sets the parameters follows. 
# NZTM Projection string
nztm_crs <- "+proj=tmerc +lat_0=0 +lon_0=173 +k=0.9996 +x_0=1600000 +y_0=10000000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs"

# --------------------------------------------------------
# 1. Download your checklist data

# https://ebird.org/downloadMyData

# --------------------------------------------------------
# 1a. OPTIONAL Combine multiple checklists into one dataframe

# Partners latest ebird data download
c1 <- read.csv("/Users/me/Downloads/MyEBirdData-74.csv",stringsAsFactors=FALSE)
# My latest ebird data download
c2 <- read.csv("/Users/me/Downloads/MyEBirdData-75.csv",stringsAsFactors=FALSE)

# Combine / append checklists together
# NOTE: There are a large number of duplicated checklists as a result of sharing lists from
#       the ebird app. Do not use the data.frame here to total individual species counts,
#       or to identify total species observations made.
chklst <- rbind.data.frame(c1,c2,stringsAsFactors = FALSE)
rm(c1,c2)

# Convert text to date and select data from 1-Jun-2019 for bird atlas 
chklst$txtDate <- chklst$Date
chklst$Date     <- strptime(chklst$Date,"%Y-%m-%d")
chklst <- chklst[chklst$Date>="2019-06-01",]

# Seasonality assignment
# It's important to view data seasonally in the assessment of effort
chklst$monthnum <- as.numeric(format(chklst$Date,"%m"))
chklst$season[chklst$monthnum==12 | chklst$monthnum==1 | chklst$monthnum==2] <- "summer"
chklst$season[chklst$monthnum==3 | chklst$monthnum==4 | chklst$monthnum==5] <- "autumn"
chklst$season[chklst$monthnum==6 | chklst$monthnum==7 | chklst$monthnum==8] <- "winter"
chklst$season[chklst$monthnum==9 | chklst$monthnum==10 | chklst$monthnum==11] <- "spring"

# Filtering data on State.Province - only retaining records starting with "NZ"
chklst <- chklst[grepl(pattern = "^NZ",x=chklst$State.Province), ]

# Excluding casual observations - only want stationary or traveling protocol observations
chklst <- chklst[chklst$Protocol!="eBird - Casual Observation",]

# Saving data as we go
save(chklst,file=paste("archive/ebird-checklists-",
                  format(Sys.Date(),"%Y%m%d"),".rdata",sep=""))


```

This brings us to the point where we have imported our checklists, and are now prepared to start looking at where our checklists are, and what habitats have had checklists created.

``` r
# Reviewing effort needs to season to be set, so that effort "so-far" can be viewed.
# Set the number of seasons you want to look at - must be one or more.
season <- c("winter","spring","summer")

# For each season...
for(i in 1:length(season)){

  #Filter the checklist so that you are only looking at a seasons worth of data
  checklists <- chklst[chklst$season==season[i],]
  
  # Remove duplicated checklists (based on txtDate, Time, Scientific.Name)
  checklists <- dplyr::distinct(checklists,txtDate,Time,Scientific.Name,.keep_all=TRUE)
  
  # Create a unique list of checklist locations
  locations <-  dplyr::distinct(checklists,Location.ID,Location,Latitude,Longitude,.keep_all=FALSE)
  
  # Create spatial object for ebird checklists
  checklist_pts <- unique(checklists[,c(1,8,10:13,15,24:26)])
  sp::coordinates(checklist_pts)=~Longitude+Latitude
  sp::proj4string(checklist_pts)<- CRS("+proj=longlat +datum=WGS84")
  
  # shapefile step - reproject to NZTM first
  pts <- sp::spTransform(checklist_pts,
                         CRS(nztm_crs))
  raster::shapefile(pts, paste("archive/ebird-checklists-",season[i],"-",
                               format(Sys.Date(),"%Y%m%d"),".shp",sep=""),overwrite=TRUE)
  # This is a copy of the previous file so that when a mapping tool is looking for the data,
  # the most recent copy always has a suffix of latest. The previous file is used to look at 
  # state of observations at a specific date
  raster::shapefile(pts, paste("mapdata/ebird-checklists-",season[i],"-",
                               "latest.shp",sep=""),overwrite=TRUE)
  
  # --------------------------------------------------------
  #  2. Select nz atlas grids that contain your checklists
  
  #    a. Connect to postgres db
  #       All the reference data is held in a publically accessable postgres database.
  instance <- "database-1.cpdimobmwqf9.ap-southeast-2.rds.amazonaws.com"
  # port:     5432
  # Connect to PostgreSQL
   conn <- RPostgreSQL::dbConnect(drv = "PostgreSQL", host = instance, dbname = "postgres", user = "appl", password = "eBird")
  tbl <- RPostgreSQL::dbReadTable(conn,"nzatlas_grid")
    
  #    b. Load nzatlas grid
  if(!exists("nzatlas.grid")){
    nzatlas.grid <- rpostgis::pgGetGeom(conn, "nzatlas_grid")
  }

  #    c. Intersect checklist point with nz atlas grids and add 
  #         nz atlas grid attributes to points
  pts.poly <-  spatialEco::point.in.poly(pts, nzatlas.grid)
  
  #    d. Return a vector containing the unique nz atlas grid squares that have checklists
  sampled.grids <- unique(pts.poly@data$name)
  
  #    e. Create shapefile of nz atlas grid squares containing checklists. This
  #         will serve as an "atlas" layer in QGIS or other GIS tools, that will
  #         allow reporting to generate for each grid square with a checklist
  #       Filter nzatlas.grid by the sampled.grids vector above

  # Retrieving data as Spatial*-class, and creating a view in the Postgres database
  clause <- paste("WHERE name IN(",paste("'",sampled.grids,"'",collapse=",",sep=""),")",sep="")
  mygrids<-rpostgis::pgGetGeom(conn, name = "nzatlas_grid", geom="geom", clauses=clause)
  
  raster::shapefile(mygrids, paste("archive/nzatlas-grid-",season[i],"-",
                                   format(Sys.Date(),"%Y%m%d"),".shp",sep=""),overwrite=TRUE)
  # This is a copy of the previous file so that when a mapping tool is looking for the data,
  # the most recent copy always has a suffix of latest. The previous file is used to look at 
  # state of observations at a specific date
  raster::shapefile(mygrids, paste("mapdata/nzatlas-grid-",season[i],"-",
                                   "latest.shp",sep=""),overwrite=TRUE)
  
  # --------------------------------------------------------
  #  3. Select lcdb-nzatlas features = nzatlas grid square "Names"
  #    a. Load the dissolved nzatlas and landcover data features from postgres db, where 
  #       the grid names match sampled.grids derived above
  
  # Retrieving data as Spatial*-class, and creating a view in the Postgres database
  clause <- paste("WHERE name IN(",paste("'",sampled.grids,"'",collapse=",",sep=""),")",sep="")
  pol<-rpostgis::pgGetGeom(conn, name = "nzatlas_lcdb41_nz_dissolve", geom="geom", clauses=clause)
  RPostgreSQL::dbDisconnect(conn)
    
  # --------------------------------------------------------
  #  4. Output lcdb-nzatlas polygons with, and without, checklists
  
  # Subset to include polygons that intersect the points i.e. values that are not "NA"
  pol.sub <- pol[!is.na(sp::over(pol, sp::geometry(pts))), ] 
  raster::shapefile(pol.sub, paste("archive/lcdb-nzatlas-checklists-",season[i],"-",
                                   format(Sys.Date(),"%Y%m%d"),".shp",sep=""),overwrite=TRUE)
  # This is a copy of the previous file so that when a mapping tool is looking for the data,
  # the most recent copy always has a suffix of latest. The previous file is used to look at 
  # state of observations at a specific date
  raster::shapefile(pol.sub, paste("mapdata/lcdb-nzatlas-checklists-",season[i],"-",
                                   "latest.shp",sep=""),overwrite=TRUE)
  
  
    
  # Now, do the inverse of the above to output poly's with no checklists. i.e. the "NA's"
  pol.sub <- pol[is.na(sp::over(pol, sp::geometry(pts))), ] 
  pol.sub.data <- pol.sub@data
  ftable(xtabs(~name_2012+name,pol.sub.data))
  
  raster::shapefile(pol.sub, paste("archive/lcdb-nzatlas-no-checklists-",season[i],"-",
                                   format(Sys.Date(),"%Y%m%d"),".shp",sep=""),overwrite=TRUE)
  # This is a copy of the previous file so that when a mapping tool is looking for the data,
  # the most recent copy always has a suffix of latest. The previous file is used to look at 
  # state of observations at a specific date
  raster::shapefile(pol.sub, paste("mapdata/lcdb-nzatlas-no-checklists-",season[i],"-",
                                   "latest.shp",sep=""),overwrite=TRUE)
  
}

```
