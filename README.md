
# Where is affordable housing urgently needed in San Diego?

Our final project will deal with location where more affordable (Section 8) housing can be developed. To do this, we will first look at existing Section 8 housing within San Diego county. We feel this is an important issue because of a shortage of affordable housing in San Diego. For example, in a building that was supposed to have 33 units built, only 4 were built.

For our current existing affordable housing dataset, we scraped https://www.sdhc.org/housing-opportunities/affordable-rentals/rent-from-sdhc/sdhc-owned-properties/ for their backend dataset of housing units at https://www.sdhc.org/wp-content/themes/sdhc/properties.php?type=sdhc-owned-properties%2Chousing-development-partners&query=, which returns a JSON file with all the current SDHC affordable housing units in San Diego. We then downloaded this for use as the file "properties.json".

# Background and Literature

### [A senior on California’s streets with little chance of a home](https://www.thereporter.com/2019/05/12/a-senior-on-californias-streets-with-little-chance-of-a-home/)
This identifies the purpose of the research question, homlessness is a major issue in San Diego and neighbouring Southern California Counties. With the creation of greater public transportation links comes the ability to expand the affordable housing sector in areas not previously accessible before. With one of the 22 ‘promise zones’ in San Diego, in addition to a large number of workforce coming in from the Mexico-US border, housing supply needs to keep up with an increased demand; and the need to match the country wide inflation. 

### [The Housing Crisis Is Obvious – but the Data Detailing Its Scope Is Not](https://www.voiceofsandiego.org/topics/land-use/the-housing-crisis-is-obvious-but-the-data-detailing-its-scope-is-not/)
> “A city report says just 33 units for middle-income units were built in seven years. The real number is actually four”

Expanding on the point above, the previous problem these housing units are not adequately budgeted and if they are the proposals end up getting rejected by the lobbyists pushing for welfare and safety of its current citizens. 
With the public transport expansion(planned and current) it is possible to have these units away from the city. This creates neighbourhoods and communities, increasing the trickle down by adding jobs and infrastructure to different parts of the city. 

### [SDHC - Affordable Housing Crisis Action Plan](https://www.sdhc.org/wp-content/uploads/2019/01/2016-01-04_SDHC-Housing-Affordability-Crisis-Action-Plan_web.pdf)
Action plan from the San Diego Housing Commission about the current housing crisis in San Diego and what can be done to address it. Points include having annual goals of housing production, introducing tax rebates for housing, opening more vacant land for development, and allocating more resources towards low-income housing.

### [Chicago Affordable Rental Housing](https://www.kaggle.com/chicago/chicago-affordable-rental-housing-developments)
This project was inspired from the Chicago Affordable Rental Housing project, the idea is to create a similar dataset for San Diego County by merging already available housing data and the San Diego County socio-economic and transportation patterns to add efficiency to this pre-existing model. 

# Python Libraries and ArcGIS modules used
#### Standard Python modules used
1. geopandas
2. pandas
3. Shapely
4. numpy
5. arcgis

#### GIS operations used
1. dissolve_boundaries
2. overlay_layers
3. geoenrichment
4. SpatialDataFrame & related functions
5. create_buffers
7. gis.map
9. spatial.join
10. Other functions on ArcGIS Online

This list evolved as we realized the limitations of the Zillow dataset. We had to use alternative datasets that were more detailed than the Zillow dataset. The inclusion of geopandas, pandas, Shapely, and numpy were needed in order to preprocess data into shape files and feature layers for ease of use as well as adding/dropping columns as part of data cleaning.

# Data Sources
1. [SDHC existing properties](https://www.sdhc.org/wp-content/themes/sdhc/properties.php?type=sdhc-owned-properties%2Chousing-development-partners&query=): JSON file traced form SDHC homepage AJAX requests that loaded pins on Google Maps for existing SDHC properties
2. [San Diego County ZIP codes](https://ucsdonline.maps.arcgis.com/home/item.html?id=81cf0db99f754a0ebc6f5ddaefd7bcad): Map of San Diego County ZIP codes
3. [ZIP Code mean and median incomes](https://www.psc.isr.umich.edu/dis/census/Features/tract2zip/): University of Michigan dataset derived from the ACS survey that details mean and median incomes by ZIP code
4. [Transit Stops GTFS](https://sdgis-sandag.opendata.arcgis.com/datasets/transit-stops-gtfs?geometry=-117.248%2C32.867%2C-117.166%2C32.88): Dataset of stop IDs and location
5. [Transit Stop Routes](https://sdgis-sandag.opendata.arcgis.com/datasets/transit-stop-routes): Stop IDs and routes that serve every stop
6. [Transit Routes GTFS](https://sdgis-sandag.opendata.arcgis.com/datasets/transit-routes-gtfs): Route geometry for SDMTS and NCTD
7. [City-owned Land](http://rdw.sandag.org/Account/GetFSFile.aspx?dir=Land+Use&Name=CITY_OWNED_LAND.zip): City-owned land for all parcels either owned by the city or leased to city.

In our project proposal, we were going to use homeless spot counts for analysis. However, we realized that a much better indicator of the need for affordable housing is to look at rent as a percentage of income as well as area median incomes, as the homeless population is very small when compared to the former. We also removed the Zillow dataset as a principal dataset because it did not give us fine enough data on the ZIP code level, which we had to use the UMich dataset as a substitute.

Many of our data sources used were from SANDAG or the open-data warehouse from the city for city resources (public transit, land). We could not obtain the existing SDHC properties through conventional means, so we had to resort to digging around on a web browser console and listening to web requests in order to gather our data.

# Data Cleaning
The first point of data cleaning we had to do was to move our existing SDHC properties data to a GIS-compatible format from just a simple JSON file. To do this, we used pandas, geopandas, and Shapely to convert the data and then save it with geopandas to a .shp file type, which we could then upload onto ArcGIS Online. We thought that most of our data would be available already in some form of a GIS warehouse, so when we were confronted with this issue we had to think of a solution we used in previous MPs for this project.

The next point in cleaning we had to do was joining the 3 transit datasets in order to get meaningful information (routes that serve stops and geometry all together). We had to fill NULL values for stop_ids as well as route_ids in order to both merge and drop them.

Finally, a last part of cleaning we had to do was using numpy in order to do calculations on an enriched dataframe to go from total in an area to a percentage.

In our project proposal, we did not expect this amount of data cleaning, which soon became apparent as our first step was to preprocess the SDHC existing properties data into a usable format.

# Descriptive Statistics 
In the development of the project analysis, we first saw clusters of affordable housing scattered around maps. As we discussed in class spatial autocorrelation can be used to find clusters of similar data (+ve Morans I). The goal in relation to the current housing locations was to have to the new housing locations further away from the current sites. This in some sense could be considered to be a variation of negative spatial autocorrelation while we look for sites in inverse value of affordable housing availability. 

For the most part, with the housing sites and the developable and vacant land plot we see random point patterns dispersed around the city based on land vacancy. 
Additionally, the results we can up with can be considered random point patterns, as the are not spatially correlated and depend heavily on city owned sites.

The description of the random point patterns in the data also explain how there is a shortage of spatial statistical descriptions. The diversity of datasets and criteria developed in the analysis do not allow for extensive statistical significance.

# Conclusions and Future Work 
### Conclusion
This project was successful on our part in considering appropriate land. The land parcels we selected in our final analysis have also recently been identified by the San Diego Housing Commission as sites suitable for developing affordable housing in the near future. 
In fact, we feel that our analysis looked at transit routes and proximity to employment opportunities more closely than the analysis put forth by the SDHC. 

While stating this project was successful in it's purpose, it is important to point out the various logistical and analytical issues in our analysis. This ranges from the lack of consideration for all types of transportation (people who need Section 8 housing may have cars, bicycles, etc) to taking a buffer of the flat distance in miles as opposed to the other diverse options available to us (e.g. driving time) through ArcGIS. 

In conclusion, we would appreciate our results to be critiqued and taken into consideration by both SDHC and SANDAG, as both are in charge of city planning and for the healthy urbanization of San Diego.
### Future Work
The project proposal and the research question we had formulated were extremely solution based. Our analysis and conclusions, as described earlier in the notebook are very precise. 

The final dataset that was incorporated into our analysis was the `developable and vacant land` dataset. This allowed us to provide exact locations that were developable while being owned by the city. The research question focused on the development of affordable housing on an urgent timeline, thus streamlining the process to looking at city-owned sites seemed the most efficient. 

Additionally, if this project was not looking at housing development on an urgent scale, a model put forward by one of the San Diego city supervisor candidates would best describe our desired future improvements. These would be to address the shortage of developable areas by not just developing housing units but also the infrastructure that came along with it. This allows the problem to be tackled at both the root and at scale. 

Below is the article referenced in the article above describing the plan of this supervisor candidate. 
https://timesofsandiego.com/politics/2019/05/28/supervisor-candidate-castellanos-offers-1-billion-affordable-housing-plan/

## Summary of Products and Result

As seen below, we have a final map layer derived from the following steps: 
1. Current units of Section 8 housing in San Diego: Scraped from the SDHC website 

2. Transit routes, transit stops and transit route stops from the MTS dataset representing the entire map of the San Diego County Transit system

3. Derived study area from zip codes representing Area Median Income (AMI). This was used to create our preliminary study area

4. `Steps 1 & 2` clipped to the study area defined in `Step 3`

5. A buffer of 1/4 mile was created around the transport (bus/trolley/rail) routes serving transit centers

6. An erase overlay of a mile around existing affordable housing units to ensure we weren't proposing new and urgent housing too close to existing units. This would not represent the diversity in demand adequately. 

7. The final derived study area with the vacant and developable land parcels dataset clipped onto it. 

8. A deep dive into the final developable plots to look at the individual parcels to identify specific sites. 

Additionally, the results are shown through the code in the notebook along with the map views with the respective layers to provide the most hollistic view of the soluition addressing the research question. 

## Discussion

The most important problem this project addresses is that of low-income populations who pay a large proportion of their paycheck on rent and where affordable housing can be built near where they live to alleviate their burden.

While this could be dealt with in a very linear manner, understanding that tackling this issue from the crux of the issue would be more efficient, giving people near or under the poverty line more access to employment and public transportation while allowing them to keep more money for saving or healthier living in their pockets every month due to rent assistance.

In the process of working on this problem we realized that there wasn't a lot of land available within the city limits that match the bounds of the criterion we had applied. 
This understanding led us to realize that the housing crisis could be solved on a short-term basis but for permenant fix that addressed the root of the issue, there has to be neighborhoods and communities developed as a whole in places that are currently underdeveloped.

This led us to analyze the potential expansion of transit lines and urban accessibility in areas far off from city centers. Building more transit centres may also be a solution to this transit problem. 

From a more technical point of view we had to make a number of informed estimations while applying buffers at two points. First, applying buffers around the transit lines was a bit tricky as we had to make sure we were considering the right type of distance parameter that would best represent the travel time. We used a flat distance in our preliminary findings and should probably use a walking time for further analysis. The second buffer we had to apply was around the current sites of affordable housing where the question was if we needed the buffer in the first place. This was because the demand for housing was so high that even sites near current Section 8 housing would serve as an asset. However, we decided on a 1 mile buffer in order to address the issue stated in our problem statement which revolved around the urgent need for housing in areas currently not served by Section 8 housing availability. 


