# python-api-challenge

### contains:
weatherpy folder:
* weatherpy.ipynb
* outputs folder:
- cities.csv (a csv of 553 randomly chosen cities throughout the world)
- .pngs of graphs of several variables vs. latitude:
    * cloudLatAllCities.png (cloudiness v. latitude for all 553 cities)
    * humLatAllCities.png (humidity v. latitude for all 553 cities)
    * northern_latitude_v_cloudiness.png (cloudiness v. latitude for cities in the northern hemisphere)
    * northern_latitude_v_max_temperature(F).png (maximum temperature (F) v. latitude in the northern hemisphere)
    * northern_latitude_v_percent_humidity.png (percent humidity v. latitude in the northern hemisphere)
    * northern_latitude_v_wind_speed.png (wind speed v. latitude in the northern hemisphere)
    * southern_latitude_v_cloudiness.png (cloudiness v. latitude for cities in the southern hemisphere)
    * southern_latitude_v_max_temperature(F).png (maximum temperature (F) v. latitude in the southern hemisphere)
    * southern_latitude_v_percent_humidity.png (percent humidity v. latitude in the southern hemisphere)
    * southern_latitude_v_wind_speed.png (wind speed v. latitude in the southern hemisphere)
    * tempLatAllCities.png (maximum temperature (F) v. latitude for all 553 cities)
    * windLatAllCities.png (wind speed v. latitude for all 553 cities)
vacationpy folder:
* vacationpy.ipynb
* images folder:
- heatmapsymbols.png (an image of the heatmap and symbol levels on the 8 ideal temperature cities)

### description:
#### weatherpy:
The weatherpy.ipynb file used citipy and the open weather map api to construct a dataframe of 553 randomly chosen cities, along with their latitudes and longitudes, and several climate variables (maximum temperature, cloudiness, wind speed, and humidity) measured on 24 December 2020. Those climate variables were graphed against each city's latitude to draw out any potential relationships. The outputs folder saves the resulting graphs, and a csv of all the data on the cities.
The following steps were taken within the weatherpy.ipynb:
* Generating the 500+ cities:
    * creating a range of latitude and longitude values for numpy.random to work on (so that non-existent latitudes/longitudes, such as 200 degrees longitude, don't get generated).
    * using numpy.random to create two lists of 1500 numbers each within the defined ranges, to create latitude/longitude pairs.
        - the lists are 1500 numbers long to account for repeats and for latitude and longitude pairs that don't successfully create cities.
    * using citipy.nearest_city to find unique cities near the randomly created latitude/longitude pairs, and appending to a list (cities).
        - the cities list is 603 cities long, down from 1500 lat/long pairs
* Obtaining climate data for the 500+ cities:
    * creating a for loop to call the open weather map api and append climate variables to lists
        - a try/except was built in, skipping cities for which data couldn't be found in the open weather api
        - checking the length of these lists shows they are 553 cities long: this is how many cities analysis will be done on
* Creating a data frame and data cleaning:
    * creating a dataframe out of the lists for analysis (weather_data_df)
    * checking to see if any humidity values need to dropped due to them exceeding 100%
    * reading the data frame to a csv (saving to the outputs folder)
    * reading the csv back in to the notebook as weather_df
        - exporting and importing the csv was done in order to create a stable data source over several working periods. If not done, every time the notebook is returned to, the data would change, due to the open weather api being called again.
* Creating scatter plots for all the data, saving them to the outputs folder:
    * temperature v. latitude
    * humidity v. latitude
    * cloudiness v. latitude
    * wind speed v. latitude
* Creating separate dataframes for the northern and southern hemispheres:
    * creating a mask for latitude > 0 or < 0 and applying it to weather_df
    * saving the results of those masks as northern_hemi_data_df and southern_hemi_data_df
* Creating scatter plots and linear regressions for the northern and southern hemisphere climate variables:
    * defining a function (scatter_linreg()) to create a scatter plot based on different variables, a linear regression based on those variables, and saving those graphs to the outputs folder.
    * calling the function for the following variable pairs, for both the northern and southern hemisphere:
        * temperature v. latitude
        * humidity v. latitude
        * cloudiness v. latitude
        * wind speed v. latitude
        - after each pair of graphs, observations are made as to what the graphs may suggest


### final considerations
This analysis contained several challenges/interesting issues. Within weatherpy, even though random numbers were generated for the latitude/longitude pairs, the final 553 cities dataframe still ended up containing more cities in the northern hemisphere. This persisted even after running the code several times. Perhaps this could be due to either citipy or open weather maps not being able to find data on as many cities in the southern hemisphere. The documentation on citipy states that only cities with over 500 inhabitants are counted; with areas with low population density such as the Amazon rainforest and central Australia in the southern hemisphere, and very high density places such as the coastal United States, western Europe, India and China in the northern hemisphere, perhaps citipy has more nearby cities for the northern hemisphere (Center for International Earth Science Information Network, 2018). Additionally, perhaps open weather maps has fewer city monitoring stations in the southern hemisphere, causing more southern hemisphere cities to be skipped when obtaining weather data for them. Whichever way the disparity occurred, it follows the analysis all the way to the end of the weatherpy notebook, potentially causing differences in the correlation coefficients in each hemisphere data graph. 
Weatherpy also shows the limits of linear regression, in that it can only show the relationship between two variables, and a linear relationship at that. As discussed within the notebook, factors that were not measured, such as seasonal variation, distance from water, altitude, and other factors that were measured, such as longitude, could all play a role in variables like cloudiness or humidity, but cannot be plotted within a linear regression. Dividing the graphs further, such as making graphs for each longitude, or weighting the scatterplot for variables such as altitude, could be ways of taking more variables into account, but that still ignores other sorts of relationships that do not fall into the y = mx + b formula, such as exponential growth or waves. Linear regression appeared most successful for the relationship between temperature and latitude, while cloudiness, humidity, and wind speed all seem to have other factors influencing them besides latitude. 
Additionally, to make claims about correlations between different variables, more than one day of measurement should be taken. The day of measurement, 24 December 2020, could have been a day of extreme variables that are not representative, as well as only representing one season per hemisphere. While the one strong linear regression correlation found between temperature and latitude appears commonsense, it cannot be asserted without further repetition of the data set. 



*References*
Center for International Earth Science Information Network - CIESIN - Columbia University (2018). _Gridded Population of the World, Version 4(GPWv4): Population Density, Revision 11._ Palisades, NY: NASA Socioeconomic Data and Applications Center (SEDAC). https://sedac.ciesin.columbia.edu/data/set/gpw-v4-population-density-rev11/maps. Accessed 3 January 2021.