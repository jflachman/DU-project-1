# DU-project-1

## Project Team
 - Jeff Flachman
 - Tomas Brown
 - Pedro Zurita

## Project 1 Directions

- Instructions for [project 1](Project_1_Overview_DU_Bootcamp.md)

## Project Ideation: US Weather Analysis

### Project Resource list

- [DU Project 1](resource.md) potential data sources

### Questions to be answered

1. What areas of the country have ideal temperature for a particular individual?
    1. Some individuals like hot, others like cool, etc.
2. For people who like it hot, what are the ideal areas
3. For people who like cold winters, what are the ideal areas
4. For people who like very temperate weather, what are the ideal areas
5. What areas of the country have less 

### Analysis

**Analysis results:**  What is the typical weather and weather extremes across the U.S.  

Pull in weather data for a year and determine the number of days in each temperature range.  Candidate temperature ranges would be bins (-30..120 in 5 degree increments).  Collate and plot the results.

**Optional**:  After analyzing data sources it is difficult to find a publicly available source to convert station lat/lon to zip-code.  FCC will convert to county.  Google can convert to zip code for use with google maps, but not with other mapping destination.  Two options:
1. **zip codes:**  use google API and google maps.  This will involve using modified KML file for zip code boundaries.
2. **counties:** use FCC API and plotly

#### Step 1: Pull in and organize weather data by weather station

- Import file (2023.csv)
- drop extra columns
- drop all non-US,CA stations
- drop all rows not using TMAX, TMIN, TAVG or PRCP data
- Combine all station information for each day and station
- Fix all NaN values in TMAX, TMIN, TAVG and PRCP columns
- Save results (all stations for each day of the year) in parquet zipped file

#### Step 2: Create station summary information
Goal: How many days of year in each temperature range

- Create bins for highs (50-55, 55-6, …. 95-100, 100-105) etc
- Create bins for lows
- <or> Alternate: may also try bins for days > 80, days > 90, days < 30 etc.
- Save results

Result should look like:
| Station ID | #days < 10 | #days < 20 | ... | #days < 65 | #days > 60 | ... | #days > 95 | # days > 100 |
|----|----|----|----|----|----|----|----|----|
| US001 | 0 | 0 | ... | 300 | 365 | ... | 45 | 20 |
| US002 | 5 | 15 | ... | 360 | 280 | ... | 5 | 0 |

#### Step 3: Clean ground station information

source: [NOAA Ground Stations](https://www1.ncdc.noaa.gov/pub/data/ghcn/daily/ghcnd-stations.txt)

- read ground stations and address NaN values.  Limit to US and Canada stations.
- write dataframe to file for future use.

#### Step 4: Plot summary information on plotly or google maps
Create maps for each temperature bin or category

    - Option 1: plotly
        - create a map for a temperature bin with small circle around each station
        - circle should be colored based on the temperature bin value
    - Option 1: google maps
        - create a map for a temperature bin with small circle around each station
        - circle should be colored based on the temperature bin value

Consider creating a movie showing the progression of temps across maps

- Create movie or possibly a presentation with each map slide transitioning to the next every few seconds.


## ***<u>Optional Analysis</u>***

### <u>Option 1: Summarize and plot temps per county or zip code</u>

Use station information and combine for zip code or county.  This will require determining the zipcode or county for each station.  Then determining the min, max and average temp and precipitation for all stations in each region.

**Note** zip code may provide finer granularity for plotting.  Consider creating and saving two datasets (stations aggregated by county and stations aggregated by zipcode)

Reverse geocode ground station lat / lon to find zipcode or county.  See options in [resources](resources.md).  FCC API can provide county information.  But zipcode will may require local install of openstreetmaps database and docker install of Nominatim application.

- Read station lat lon
- perform fcc query for county
- combine all station info (max, min, avg, precip) for all stations in each county
- combine into new df
- Query for county or zip code 
- Add to combined df
- Process all counties or zip and combine / average all data (high, low, precip)
- Save results

#### Visualization of zip or county results

Determine how to use shape files from US census, color code and import into mapping program
- Options:  
    1. [google maps]
    2. [plotly maps](https://plotly.com/python/county-choropleth/#the-entire-usa)


### <u>Option 2: Calculate median house prices for counties</u>

source: [US CEnsus median housing prices](https://data.census.gov/table/ACSDT5Y2022.B25077?t=Financial%20Characteristics:Housing%20Value%20and%20Purchase%20Price&g=010XX00US$8600000&y=2022): Candidate files:  B2503, B2506, B2507 & DP04

cleaned file should look like the following.

| County name | median price |
| ------------ | ----------- |
| Araphaoe | 612000 |
| Douglas | 630000 |

#### Visualization of zip or county results

Determine how to use shape files from US census, color code and import into mapping program
- Options:  
    1. [google maps]
    2. [plotly maps](https://plotly.com/python/county-choropleth/#the-entire-usa)
        - Use county based ploty map
        - Color code each county by median price

### <u>Option 3: Create Jupyter file to provide custom analysis</u>

Example: All stations where temperature is between 40 & 80 340 days of the year) and overlay on median housing data

- make min temp (40) a variable
- make max temp (80) a variable
- make #days in range (340) a variable
- create map
