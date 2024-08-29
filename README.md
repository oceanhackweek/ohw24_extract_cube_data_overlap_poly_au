# ohw24_proj_extract_cube_data_overlap_poly_au

## Project Name
Extracting data using overlapping polygons from a data cube using Python

## One-line Description
How can we make extracting data from a publicly available zarr file using hundreds of overlapping polygons?

## Collaborators
- [Clement Astruc Delor](https://github.com/clemasde)  
- [Bryan Hally](https://github.com/janomecopter)  
- [Denisse Fierro Arcos](https://github.com/lidefi87)  

## Background
The [Data Management System (DMS) for the Great Barrier Reef (GBR)](https://imos.org.au/data/access-ocean-data/great-barrier-reef-data-management-system) has a wide collection of publicly available datasets describing various aspects of the GBR. The DMS aims to be a “one stop shop” where researchers and policy makers can access data about the physical, ecological, and social components of the GBR.  
  
The DMS holds some very large gridded datasets, including satellite products and model outputs of different shapes. There is a general interest in extracting long time series using individual reef footprints. Depending on the size of the cells of the gridded data, multiple overlapping polygons are possible. Masking the raster with a single polygon is the easiest way to extract data under it, but looping over more than 6,000 reefs makes the process extremely inefficient. The challenge is to find a solution that will be able to efficiently extract a full-time series from gridded data and aggregate the output under the footprint of every individual reef and return a table stored as a geoParquet file. This product is extremely useful for the day-to-day decisions about managing the GBR.

## Goals
Improving the efficiency of the data extraction workflow when using overlapping polygons. If this project is of interest, currently script extracting data will be shared here.

## Datasets
We can use any dataset to improve this workflow, but the three datasets below are the most used when extracting data with overlapping polygons.  
- eReefs hydrodynamical model output at 1km cell resolution: s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-1km-daily/data.zarr  
- eReefs hydrodynamical model output at 1km cell resolution: s3://gbr-dms-data-public/aims-ereefs-agg-hydrodynamic-4km-daily/data.zarr  
- GBRMPA - Complete GBR Features: s3://gbr-dms-data-public/gbrmpa-complete-gbr-features/data.parquet  

## Workflow/Roadmap
1. **Load the GBRMPA shapefile and dissolve all polygons into one.** 
2. **Load the gridded dataset and use the dissolved GBRMPA shapefile to extract data where polygons are present.** This step reduces the size of the gridded dataset used in the data extraction at a reef level.  
3. **Rechunk cropped dataset.** Chunk size of the dataset was changed to maximise use of resources when cropping data for each reef in the GBRMPA polygon. However, rechunking works best when the rechunked data is saved to disk and loaded into memory.  
4. **Extract data at a reef level using the cropped and rechunk gridded dataset.**  
5. **Save extracted data for each reef as a csv file.** We should note that this step takes a long time if the rechunked data is not saved to disk.  

## References
- E. Lawrey, M. Stewart, "Mapping the Torres Strait Reef and Island Features: Extending the GBR Features (GBRMPA) dataset,"  (Report to the National Environmental Science Programme, Cairns, 2016).  
- M. Herzfeld et al., "eReefs Marine Modelling: Final Report,"  (https://research.csiro.au/ereefs/?ddownload=2836, 2016).  
- A. D. L. Steven et al., eReefs: An operational information system for managing the Great Barrier Reef. Journal of Operational Oceanography 12, S12-S28 (2019).  

