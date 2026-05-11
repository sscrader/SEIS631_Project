# Comparison of lake chlorophyll levels to buffer zone land use using remote sensing data  

## 1. Research Question

I will be investigating how the land use (Crop Land, Developed, Tree Cover, etc..) in a 50 meter buffer around lakes in Minnesota relate to chlorophyll levels, specifically at the peak of algal growth in September in 2025.

## 2. Hypothesis

Null Hypothesis - The land use in the 50 meter buffer zone around bodies of water do not impact the mean observed chlorophyll levels, so we see no difference in mean chlorophyll when there is a differenc in the majority land use categorization of buffer zones.

Alternative Hypothesis - The land use categorization of the 50 meter buffer zone around a body of water does have an association with the chlorophyll level. For example, lakes surrounded by a majority of cropland and developed land would have significantly higher chlorophyll levels than other lakes surounded by a majority of tree cover.

## 3. Data Description

### Lake water quality data for 2017–2025 (GeoPackage for mapping, ZIP file)
This .zip is downloaded from the LakeBrowser site and is used to set lake geometries with 
https://lakes.rs.umn.edu/

This data was filtered to only include lakes with an area >10 acres, exclude lake superior, and look only at rows with chlorophyll data for the summer of 2025 and September 2025. Large chlorophyll outliers were removed by only keeping chlorophyll readings <100 µg/L. There are 10,343 lake observations that fit these parameters.

| Column | Description | 
| -------- | -------- |
| unmlknum | Updated 2010 UMN lake number and the only unique ID for all 12,193 polygons |
| RNAME_1 | Name of the water body associated with the lake polygon. |
| PolyAcres | Polygon area in acres calculated from the lake polygon. |
| geometry | Multipolygon geographic shape of each body of water |
| CL_202509 | Mean chlorophyll a (µg/L) for the year 2025 and month 9 (September). |
| CL_20250601_0930 | Mean summer chlorophyll a (µg/L) for the year 2025 and the summer period (June 1 through September 30) |

### GEMS-UMN Exchange Notebooks landcover API
This API enables exploration of land cover data from the Land Change Monitoring, Assessment, and Projection (LCMAP) for Minnesota from 2020.
https://github.com/GEMS-UMN/Exchange-Notebooks

A 50 meter buffer polygon was drawn around each body of water in the filtered water quality data. There was LCMAP land use data available for 9,906 bodies of water. The total number of pixels and the LCMAP categorization of each pixel in the buffer zone was used to determine the proportion of land use categorization for each body of water. The column majority_landuse was added to the table and listed the land use category for that lake if there was one with a greater than 50% proportion. If no use categories for a body of water reached >0.5, the majority_landuse was assigned as Mixed.

| Column | Description | 
| -------- | -------- |
| lake_id | Updated 2010 UMN lake number and the only unique ID for all 12,193 polygons |
| lake_name | Name of the water body associated with the lake polygon. |
| lcmap_year | Year of the raster data collection. |
| buffer_50m_acres | Polygon area in acres calculated from the 50 meter buffer zone polygon. |
| total_landcover_pixels | The number of 30m x 30m pixels included in the buffer zone shape. |
| NoData_count | Count of pixels without land use data |
|"landuse"_count | Count of pixels for that land use type in the buffer zone. Land use types are Developed, Cropland, TreeCover, Water, Wetland, Ice_Snow, Barren |
|"landuse"_prop | proportion of the total pixels that land use type covers. |

## 4. Methods

### Bootstrap 1 - Median Chlorophyll
The CLT doesn't apply to the Median Chlorophyll for September 2025 because median is an order based statistic and the central limit theorem is used to describe the mean of repeated samples.

To bootstrap the median chlorophyll for September 2025, the September 2025 chlorophyll values were sampled 9906 times with replacement and the median was found. This sampling and median calculation was repeated 5,000 times. The 95% confidence interval was calculated around this bootstrapped median.

### Bootstrap 2 - Mean Chlorophyll by Land Use Category
The observed mean chlorophyll for September 2025 was calculated for each buffer zone land use category. For each category, the chlorophyll data was resampled with replacement and a new mean was calculated for 5,000 repetitions. The distribution of these bootstrapped means and the resulting 95% confidence interval was plotted for each land use category.

### Permutation Test - Mean Chlorophyll by Land Use Category F-Statistic
Compare between-group variation in chlorophyll to within-group variation in chlorophyll. A large F indicates the mean chlorophyll are spread far apart relative to the noise withing each land use category and a small F indicates the land use categories look like they have the same chlorophyll distribution.

$$F = \frac{\text{between-group variability}}{\text{within-group variability}}$$

The land use labels will be shuffled and the f-statistic will be re-calculated for this permutation test 5,000 times. The repetition of this suffling will create the null distribution that the observed F-statistic can be compared to.

## 5. Results

### Bootstrap 1 - Median Chlorophyll
The bootstrapped median for September 2025 Chlorophyll across all bodies of water was 6.33 µg/L with a 95% confidence interval of [6.15, 6.51]. This means that if the september 2025 chlorophyll data were repeatedly resampled and the median calculated, 95% of the time that median would be between 6.15-6.51. When considering the entire range of possible Chlorophyll values from 0 to 100, this is a pretty narrow range. 

### Bootstrap 2 - Mean Chlorophyll by Land Use Category
The bootstrapped means for chlorophyll in september 2025 split into each land use category and their 95% condfidence intervals were calculated. I thought this series of bootstrapping illustrated the CLT well because the land use category with the smalles bodies of water Grass_Shrub (n=3) had the widest CI around the mean of 31 [5, 56] and TreeCover was the second largest category (n=3337) and the narrowest CI [7.38, 8.06]. They all showed normal distribution of bootstrapped means, though they each took on a different shape depending on their sample size.

### Permutation Test - Mean Chlorophyll by Land Use Category F-Statistic
The mean square (variance) between groups is much higher than the mean square (vairance) within groups which gave us a very large F-statistic of 179. When this p-value is calculated for this observation against the f-statistics found under the null hypothesis, we get a value that is approaching 0. This indicates that it's highly improbable that the variation in mean chlorophyll among land use groups is due to chance - we'd reject the null hypothesis in favor of the alternative hypothesis that the mean chlorophyll and buffer zone land use is correlated.

## 6. Uncertainty Estimation

Discuss your resampling results:

* How many resamples you used
* What the bootstrap or randomization distributions looked like
* How you interpret the interval estimates

### Bootstrap 1 - Median Chlorophyll
The bootstrapping was repeated 5,000 times, which produced a normal distribution of medians centered around 6.33 µg/L and spreading between about 6-6.7 µg/L.

### Bootstrap 2 - Mean Chlorophyll by Land Use Category
These bootstraps were each repeated 5,000 times, producing normal distribution of means, though the 95% confidence intervals varied greatly depending on the category's sample size. For example the Grass_Shrub category only contained 3 lakes, so the confidence interval was very wide - almost undescriptive.

### Permutation Test - Mean Chlorophyll by Land Use Category F-Statistic
The permutations and re-calculation of the F-Statistic were repeated 5,000 times, which produced pleanty of data under the null huypothesis. This null curve has a right tail. When plotted with the observed F-Statistic, this curve becomes a skinny spike due to the distance of the observed stat from the null curve.

## 7. Limitations

### Spatial and Temporal Scope
This analysis is limited to lakes within Minnesota, so couldn’t be generalized to regions with different climates, watershed structures, or land use patterns.
The data reviewed was only for 2025, so would not capture longer term ecological trends or even seasonal dynamics.
### Buffer and Land Use Simplification
The 50-meter buffer zone is very narrow when considering the land use resolution is 30m by 30m blocks and by not be representative of the land that’s further away in the watershed that may still have an impact on chlorophyll levels.
### Remote Sensing Data Limitations
Land cover categorization from LCMAP is subject to classification uncertainty which may affect the accuracy of estimated buffer zone land use.
Chlorophyll measurements are similarly subject to variability based on the condition of the sensing data.
### Assumption of Independence
The bootstrapping and permutation testing analysis assume that the chlorophyll observations for each lake are independent from each other, even though the clustering of lakes near each other may introduce correlation between sites that are close together. 


## 8. References

* Minnesota Lakes and Rivers Data PortalUniversity of Minnesota. (n.d.). Minnesota lakes and rivers data portal. https://lakes.rs.umn.edu/ 
* Minnesota Lake BrowserUniversity of Minnesota. (n.d.). Minnesota lake browser. https://water.rs.umn.edu/lakebrowser 
* GEMS-UMN Exchange Notebooks GitHub RepositoryGEMS-UMN. (n.d.). Exchange notebooks [GitHub repository]. GitHub. https://github.com/GEMS-UMN/Exchange-Notebooks 
* GeoPandas User GuideGeoPandas developers. (n.d.). GeoPandas user guide. https://geopandas.org/en/stable/docs/user_guide.html 
* University of Minnesota Water Resources Center PresentationsUniversity of Minnesota. (n.d.). Presentations. https://water.rs.umn.edu/present 
* NASA Earth Observation Data BasicsNational Aeronautics and Space Administration. (n.d.). Earth observation data basics. https://www.earthdata.nasa.gov/learn/earth-observation-data-basics
