# Lake Chlorophyll Levels and Buffer Zone Land Use Association Through Remote Sensing Data 

## 1. Research Question

I will be investigating how the land use (Crop Land, Developed, Tree Cover, etc..) in a 50 meter buffer around lakes in Minnesota relate to chlorophyll levels, specifically at the peak of algal growth in September in 2025.

## 2. Hypothesis

Null Hypothesis - The land use in the 50 meter buffer zone around bodies of water do not impact the mean observed chlorophyll levels, so we see no difference in mean chlorophyll when there is a differenc in the majority land use categorization of buffer zones.

Alternative Hypothesis - The land use categorization of the 50 meter buffer zone around a body of water does have an association with the chlorophyll level. For example, lakes surrounded by a majority of cropland and developed land would have significantly higher chlorophyll levels than other lakes surounded by a majority of tree cover.

## 3. Data Description

### Lake water quality data for 2017–2025 (GeoPackage for mapping, ZIP file)
This .zip is downloaded from the LakeBrowser site and is used to set lake geometries with 
https://lakes.rs.umn.edu/

This data was filtered to only include lakes with an area >10 acres, exclude lake superior, and look only at rows with chlorophyll data for the summer of 2025 and September 2025. There are 10,343 lake observations that fit these parameters.

| Column | Description | 
| -------- | -------- |
| unmlknum | Updated 2010 UMN lake number and the only unique ID for all 12,193
polygons |
| RNAME_1 | Name of the water body associated with the lake polygon. |
| PolyAcres | Polygon area in acres calculated from the lake polygon. |
| geometry | Multipolygon geographic shape of each body of water |
| CL_202509 | Mean chlorophyll a (µg/L) for the year 2025 and month 9 (September). |
| CL_20250601_0930 | Mean summer chlorophyll a (µg/L) for the year 2025 and the summer period (June 1 through September 30) |

### GEMS-UMN Exchange Notebooks landcover API
This API enables exploration of land cover data from the Land Change Monitoring, Assessment, and Projection (LCMAP) for Minnesota from 2020.
https://github.com/GEMS-UMN/Exchange-Notebooks

There was land use data available for 9,906 bodies of water.

| Column | Description | 
| -------- | -------- |
| lake_id | Updated 2010 UMN lake number and the only unique ID for all 12,193
polygons |
| lake_name | Name of the water body associated with the lake polygon. |
| lcmap_year | Year of the raster data collection. |
| buffer_50m_acres | Polygon area in acres calculated from the 50 meter buffer zone polygon. |
| total_landcover_pixels | The number of 30m x 30m pixels included in the buffer zone shape. |
| NoData_count | Count of pixels without land use data |
|"landuse"_count | Count of pixels for that land use type in the buffer zone. Land use types are Developed, Cropland, TreeCover, Water, Wetland, Ice_Snow, Barren |
|"landuse"_prop | proportion of the total pixels that land use type covers. |


## 4. Methods

Summarize how you analyzed the data:

* The test statistic for your permutation test
* How you simulated or resampled under the null hypothesis
* The metric(s) for which you created bootstrap confidence intervals
* Why the CLT does not apply to at least one metric

## 5. Results

Present your main findings:

* Key summary statistics and visualizations
* Observed test statistic and p-value (if applicable)
* Bootstrap confidence intervals for relevant metrics

## 6. Uncertainty Estimation

Discuss your resampling results:

* How many resamples you used
* What the bootstrap or randomization distributions looked like
* How you interpret the interval estimates

## 7. Limitations

Briefly note any limitations in data, assumptions, or methods, including sources of bias or missing data.

## 8. References

List all datasets, tools, libraries, or papers you cited.

---

**Reminder:** Your README should be clear enough that someone unfamiliar with your work could understand what you studied, how you analyzed it, and what you found.
