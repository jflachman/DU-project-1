# DU-project-1

## Project Team
 - Jeff Flachman
 - Tomas Brown
 - Pedro Zurita

## Project 1 Instructions / Directions

- Instructions for [project 1](Project_1_Overview_DU_Bootcamp.md) provided by the course instructors.

## Project Ideation: 
- Jeff - US Weather Analysis for ideal places to live
- Pedro - Using consumer data for 

## Project Files:

- [Project Report](doc/Ideal_regions_based_on_temperature_preferences.pdf)
- [Project Presentation](doc/AI_Bootcamp_Project_1_Ideal_weather.pdf)
- Resource list
    - A project resource list was created to hold all the potential data sources we researched.
        - [DU Project 1](resources.md) potential data sources
- Preliminary [optional median home value](optional_get_median_home_values.ipynb)

### Important information about the DU-Project-1 Repository

Issues with maxing out the LFS quota on github prevented storing the source_files, result_files and plotly html files on github.  In order to see these files, run the jupyter files steps 1 to 5 in order.  The jupyter files will automatically create a result_files and plotly files directory in the cloned local repo to save the files.
 - step1 now checks for the existence of result_files directory and creates it if it does not exist.  
    - All step 1-4 results will be saved in this directory as compressed parquet files.  
    - The compressed parquet format was determined to be the most efficient format to save the intermediate and final results.  See the bottom of the [resources](resources.md) file for this analysis from an article in Toward Data Science.
 - step5 now checks for teh existence of the plotly subdirectory and create if it does not exist.
    - All plotly html files (dynamic plotly maps) will be saved in the plotly subdirectory



## Project Summary

Our team is: Jeff Flachman, Pedro Zurita, Thomas Brown
People like different temperature ranges that allow them to enjoy where they live and the types of activities they do.  For this project we examined weather data to identify areas where different types of people may choose to live based on their preference for cold, hot, or temperate climates.

ChatGPT and Claude were queried to refine parameters for cold, hot and temperate climates.  These recommendations were discussed, and a range of temperatures was defined for each of the climates.  Global weather station data for the US and Canada was pulled, cleaned and organized. 

Because weather is a geographic centric statistic, we found it best to analyze and present our data visually on dynamic plotly maps.  The maps in the report contain links to dynamic plotly maps saved on our github pages at http://jflachman.github.io.


## Questions, Analysis and Summary
### Goal 

-Goal:
    - We want to answer a basic question:  Where would I like to live based on the weather in that region?

###	Question 1:  What are the different climate categories from arid to wet?
For Arid and Semi-Arid climates are characterized by 100 days of precipitation or less.  This is good for hot and temperate climates.  

For Humid Continental climates are characterized by 100-200 days of precipitation or less.  This may (a subjective answer) be better for colder climates to support winter activities.
- Arid to Semi-Arid Regions:
    - Southwester US
- Wetter or Humid Regions:
    - Northwestern US and Canada
    - US and Canada East Coast toward the Midwest
- Median Precipitation Regions
    - Midwest- Dakotas south to East Texas)
### Question 2:  For people who like cold weather, what is the ideal temperature?

25°F to 55°F was selected as the desired temperature range for people who like colder temperatures based on the initial ChatGPT and Claude responses.
- Comfort Zones for people who like it cold 25°F to 55°F:
    - Northern US and all of Canada
    - The area includes southern states in the west.

32°F to 65°F was selected as an alternate definition for the desired range for people who like colder temperatures based on the initial ChatGPT and Claude responses.  
- Comfort Zones for people who like it cold 32°F to 65°F:
    -  Mid latitudes of US and all of Canada
 
### Question 3:  For people who like hot weather, what is the ideal temperature?
75°F to 95°F was selected as the desired temperature range for people who like hotter temperatures based on the initial ChatGPT and Claude responses.
Comfort Zones for people who like it hot :
- Southeast US (FL, LA, AL, GA, SC, NC, TN)
- Central Southern CA
- South TX & AZ

### Question 4:  For people who like temperate weather, what is the ideal temperature?
60°F to 80°F was selected as the desired temperature range for people who like hotter temperatures based on the initial ChatGPT and Claude responses.

Comfort Zones for people who like temperate weather include:
Westcoast within a short range from the coastline (CA, OR WA

- Canada BC – Vancouver, Edmonton
- Mid-southeast US (VA, WV, NC, SC, TN, GA and N AL)
- Arizona at elevation (Flagstaff on to SE AZ)

### Question 5:  What temperatures would be considered extreme that people who typically like hot or cold climates would not like?

Temperatures less than 20°F were selected as the range where people who like colder temperatures will find the cold to be extreme.  Temperatures greater than 95°F were selected as the point where people who like hotter temperatures will find the heat to be extreme.

Really too cold: Days < 20°F
- Most regions above Lat: 51 (Canada, Alaska) 
    - Coastal areas in Alaska and Canada are less extreme due to the stability of the water temperatures.
- Also dipping into the northern central US (ND, SD)
- And the Central west US at altitude (WY, CO)

Really too hot: Days > 90°F
- Most regions below Lat: ~36 for Texas to the west.  Also ranges up into Kansas.
- See Brown & Cyan regions.

### Overall Conclusions
There is no definitive answer as to the best region where any individual may choose to live.  People have a variety of preferred climates.  However, given knowledge that the west coast, Hawaii and the eastern US have higher home prices it would lead to the understanding the temperate climates are preferred by most people.  

## Analysis Approach

The analysis was broken into multiple steps.


step1_clean_noaa_weather_data.ipynb
- Pulled in 37M daily weather records from 42K weather stations across the globe.
o	https://www1.ncdc.noaa.gov/pub/data/ghcn/daily/by_year/2023.csv.gz
- Reduced the data to US and Canada weather stations.
- Transformed the data by consolidating (groupby) for Precipitation & min, max and average temperature.
- Resulted in 8687 stations across US and Canada

step2_clean_ground_stations.ipynb
- Pulled in station locations from NOAA website in column separated file:
o	 https://www1.ncdc.noaa.gov/pub/data/ghcn/daily/ghcnd-stations.txt
- Cleaned station locations and column namces

step3_add_summaries.ipynb
- Continued with parquet result file from step 1.
- Binned all the data within ~20 temperature ranges for TMAX (count of days in each range)
- Eliminated stations with fewer than 300 days of data and normalized to 365 days.
o	This allowed a reasonably accurate count of days in our analysis.
o	Assumption: missing days are evenly distributed across the bins.  This is an imperfect assumption.
- Resulted in 7767 stations across US and Canada
- Performed analysis of #days > X, #days < X, and X < #days < Y for a range of temperatures X & Y and stored the results in the associated columns.

step4_combine_weather_locations.ipynb
- Added Station Lat/Lon (Merge) to binned station information

step5_plot_us_canada_pedro.ipynb
- Plotted all the result on multiple dynamic map using plotly (plotly.com)
- All the resulting plots are located on our github pages: https://jflachman.github.io/

step6_plot_your_own_ranges.ipynb
- Allows others to create custom plotly maps with the data.

# Options for the future

The section contains unorganized notes

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


## ***<u>Optional Analysis - Tabled for the future</u>***

### <u>Option 1: Summarize and plot temps per county or zip code</u>

Sources for future analysis may be found in the [Resources File](resources.md)

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

source: [US Census median housing prices](https://data.census.gov/table/ACSDT5Y2022.B25077?t=Financial%20Characteristics:Housing%20Value%20and%20Purchase%20Price&g=010XX00US$8600000&y=2022): Candidate files:  B2503, B2506, B2507 & DP04

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
