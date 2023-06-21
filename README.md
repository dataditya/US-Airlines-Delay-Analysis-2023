# US-Airlines-Delay-Analysis-2023-

## Description

According to air travel consumer reports, a large proportion of consumer complaints are
about frequent flight delays.
Out of all the complaints received from consumers about airline services, 32% were related to
cancellations, delays, or other deviations from the airlines’ schedules.
There are unavoidable delays that can be caused by air traffic, no passengers at the airport,
weather conditions, mechanical issues, passengers coming from delayed connecting flights ,
security clearance, and aircraft preparation.

**Objective**

The objective of this project is to identify the factors that contribute to avoidable flight delays.
You are also required to build a model to predict if the flight will be delayed
Problem statement:

Airlines.xlsx

| Column | Description |
| --- | --- |
| Airline | Different types of commercial airlines |
| Flight | Types of Aircraft |
| AirportFrom | Source Airport |
| AirportTo | Destination Airport |
| DayOfWeek | Tells you about the day of week |
| Time | departure time measured in minutes from midnight (range is 10-1439) |
| Length | duration of the flight in minutes |
| Delay | Whether the flight is delayed or not. |

Airports.xlsx

| Column | Description |
| --- | --- |
| id | Internal OurAirports integer identifier for the airport. This will stay persistent, even if the airport code changes. |
| ident | The text identifier. This will be the ICAO(International Civil Aviation Organization) code if available. Otherwise, it will be a local airport code (if no conflict), or if nothing else is available, an internally-generated code starting with the ISO2 country code, followed by a dash and a four-digit number. |
| type | The type of the airport. Allowed values are "closed_airport", "heliport", "large_airport", "medium_airport", "seaplane_base", and "small_airport". See the map legend for a definition of each type. |
| name | The official airport name, including "Airport", "Airstrip", etc. |
| latitude_deg | The airport latitude in decimal degrees (positive for north). |
| longitude_deg | The airport longitude in decimal degrees (positive for east). |
| elevation_ft | The airport elevation MSL in feet (not metres). |
| continent | The code for the continent where the airport is (primarily) located. Allowed values are "AF" (Africa), "AN" (Antarctica), "AS" (Asia), "EU" (Europe), "NA" (North America), "OC" (Oceania), or "SA" (South America). |
| iso_country | The two-character ISO 3166:1-alpha2 code for the country where the airport is (primarily) located. A handful of unofficial, non-ISO codes are also in use, such as "XK" for Kosovo. Points to the code column in countries.csv. |
| iso_region | An alphanumeric code for the high-level administrative subdivision of a country where the airport is primarily located (e.g. province, governorate), prefixed by the ISO2 country code and a hyphen. OurAirports uses ISO 3166:2 codes whenever possible, preferring higher administrative levels, but also includes some custom codes. See the documentation for regions.csv. |
| municipality | The primary municipality that the airport serves (when available). Note that this is not necessarily the municipality where the airport is physically located. |
| scheduled_service | "yes" if the airport currently has scheduled airline service; "no" otherwise. |
| gps_code | The code that an aviation GPS database (such as Jeppesen's or Garmin's) would normally use for the airport. This will always be the ICAO code if one exists. Note that, unlike the ident column, this is not guaranteed to be globally unique. |
| iata_code | The three-letter IATA code for the airport (if it has one). |
| local_code | The local country code for the airport, if different from the gps_code and iata_code fields (used mainly for US airports). |
| home_link | URL of the airport's official home page on the web, if one exists. |
| wikipedia_link | URL of the airport's page on Wikipedia, if one exists. |
| keywords | Extra keywords/phrases to assist with search, comma-separated. May include former names for the airport, alternate codes, names in other languages, nearby tourist destinations, etc. |

Runways.xlsx

| Column | Description |
| --- | --- |
| id | Internal OurAirports integer identifier for the runway. This will stay persistent, even if the runway numbering changes. |
| airport_ref | Internal integer foreign key matching the id column for the associated airport in airports.csv. (airport_ident is a better alternative.) |
| airport_ident | Externally-visible string foreign key matching the ident column for the associated airport in airports.csv. |
| length_ft | Length of the full runway surface (including displaced thresholds, overrun areas, etc) in feet. |
| width_ft | Width of the runway surface in feet. |
| surface | Code for the runway surface type. This is not yet a controlled vocabulary, but probably will be soon. Some common values include "ASP" (asphalt), "TURF" (turf), "CON" (concrete), "GRS" (grass), "GRE" (gravel), "WATER" (water), and "UNK" (unknown). |
| lighted | 1 if the surface is lighted at night, 0 otherwise. (Note that this is inconsistent with airports.csv, which uses "yes" and "no" instead of 1 and 0.) |
| closed | 1 if the runway surface is currently closed, 0 otherwise. |
| le_ident | Identifier for the low-numbered end of the runway. |
| le_latitude_deg | Latitude of the centre of the low-numbered end of the runway, in decimal degrees (positive is north), if available. |
| le_longitude_deg | Longitude of the centre of the low-numbered end of the runway, in decimal degrees (positive is east), if available. |
| le_elevation_ft | Elevation above MSL of the low-numbered end of the runway in feet. |
| le_heading_degT | Heading of the low-numbered end of the runway in degrees true (not magnetic). |
| le_displaced_threshold_ft | Length of the displaced threshold (if any) for the low-numbered end of the runway, in feet. |
| he_ident | Identifier for the high-numbered end of the runway. |
| he_latitude_deg | Latitude of the centre of the high-numbered end of the runway, in decimal degrees (positive is north), if available. |
| he_longitude_deg | Longitude of the centre of the high-numbered end of the runway, in decimal degrees (positive is east), if available. |
| he_elevation_ft | Elevation above MSL of the high-numbered end of the runway in feet. |
| he_heading_degT | Heading of the high-numbered end of the runway in degrees true (not magnetic). |
| he_displaced_threshold_ft | Length of the displaced threshold (if any) for the high-numbered end of the runway, in feet. |

**Applied Data Science With Python**

1. Import and aggregate data:
a. Collect information related to flights, airports (e.g., type of airport and elevation), and runways (e.g., length_ft , width_ft , surface , and number of runways). Gather all fields you believe might cause avoidable delays in one dataset.
Hint: In this case, you would have to determine the keys to join the tables. A data description will be useful.
b. When it comes to on time arrivals, different airlines perform differently based on the amount of experience they have. The major airlines in this field include US Airways Express (founded in
1967) Continental Airlines (founded in 1934), and Express Jet (founded in 19860. Pull such
information specific to various airlines from the Wikipedia page link given below.
https://en.wikipedia.org/wiki/List_of_airlines_of_the_United_States.
Hint: Here, you should use web scraping to learn how long an airline has been operating.
    
    c. You should then get all the information gathered so far in one place.
    d. The total passenger traffic may also contribute to flight delays. The term hub refers to
    busy commercial airports. Large hubs are airports that account for at least 1 percent of the total passenger enplanements in the United States. Airports that account for 0.25 percent to 1 percent of total passenger enplanements are considered medium hubs. Pull passenger traffic data from the Wikipedia page given below using web scraping and collate it in a table.
    https://en.wikipedia.org/wiki/List_of_the_busiest_airports_in_the_United_States
    
2. You should then examine the missing values in each field, perform missing value
treatment, and justify your actions.
3. Perform data visualization and share your insights on the following points:
a. According to the data provided, approximately 70% of Southwest Airlines flights are delayed. Visualize it to compare it with the data of other airlines.
b. Flights were delayed on various weekdays. Which day of the week is the safest for travel?
c. Which airlines should be recommended for short --, medium --, and long distance travel?
d. Do you notice any patterns in the departure times of long duration flights?
4. How many flights were delayed at large hubs compared to medium hubs? Use
appropriate visualization to represent your findings.
5. Use hypothesis testing strategies to discover:
a. If the airport's altitude has anything to do with flight delays for incoming and
departing flights
b. If the number of runways at an airport affects flight delays
c. If the duration of a flight (length) affects flight delays
Hint: Test this from the perspective of both the source and destination airports
6. Find the correlation matrix between the flight delay predictors, create a heatmap to
visualize this, and share your findings

**Machine Learning**

1. Use OneHotEncoder and OrdinalEncoder to deal with categorical variables
2. Perform the following model building steps:
a. Apply logistic regression (use stochastic gradient descent optimizer) and decision tree models
b. Use the stratified five fold method to build and validate the models
Note : Make sure you use standardization effectively, ensuring no data leakage and leverage pipelines to have a cleaner code
c. Use RandomizedSearchCV for hyperparameter tuning, and use k fold for cross validation
d. Keep a few data points (10%) for prediction purposes to evaluate how you would make the final prediction, and do not use t his data for testing or validation
Note: The final prediction will be based on the voting (majority class by 5 models created using the stratified 5 fold method)
g. Compare the results of logistic regression and decision tree classifier
3. Use the stratified five fold method to build and validate the models using the XGB classifier, compare all methods, and share your findings
