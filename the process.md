#### Libraries used:

import requests # for making standard html requests <br>
from bs4 import BeautifulSoup # magical tool for parsing html data <br>
import pandas as pd # for data manipulation <br>
import numpy as np # for data manipulation <br>
import seaborn as sns # for data visualization <br>
import matplotlib.pyplot as plt # for data visualization <br>
from sklearn.metrics import mean_absolute_percentage_error # for model evaluation <br>
import statsmodels.api as sm # for data exploration <br>
import scipy.stats as stats # for statistical analysis <br>
from sklearn.linear_model import SGDClassifier # for classification <br>
from sklearn.model_selection import StratifiedKFold, RandomizedSearchCV, train_test_split # for cross validation and hyperparameter tuning <br>
from statsmodels.formula.api import glm # for classification <br>
from sklearn.preprocessing import OrdinalEncoder, StandardScaler # for data preprocessing <br>
from sklearn.pipeline import Pipeline # for data preprocessing <br>
from sklearn.metrics import classification_report, accuracy_score # for model evaluation <br>
from sklearn.tree import DecisionTreeClassifier # for classification <br>
from xgboost import XGBClassifier # for classification <br>

#### Steps followed:
- **merge** airports and runways and create airport_run using the **ident column in airports**, and the **airport_ident column in runways**. We are doing a left join i.e. all of airports columns and only the matching values from runways will be imported.
- count the number of runways each airport has. Here we will **group the rows** based on the **common airport_ident** values while keep a **count of the non-null values in id_y** which has the runway entries of a particular airport.
- create a **new dataset containing airport iata code, type of airport, elevation of airport, and the number of runways**. All of which could play a role in delays.
- **add info about the AirportFrom** values by combining the airlines and air_run datasets based on the AirportFrom and iata_code columns, this would give us the **count of runways, elevation, and iata_code of the airports from where flights take off**. Something that could be a factor in delays. 
- **add similar info about the destination airports**. This would also be a factor in flights delays. For eg: if the destination airport has only one runway, flights would have to park themselves and wait for their turn to land resulting in delays. But this time we will use combined_data instead of airlines since we have all the info from airlines dataset from previous steps. 
- As is with every profession, the amount of experience we have plays an important role in our performance. Applying that logic to airline delays, let's **extract experience info about each airline in our dataset** from "https://en.wikipedia.org/wiki/List_of_airlines_of_the_United_States" using **Beautiful Soup class from bs4 and requests lib.**
- Apart from the experience, the traffic of the airport is also a factor in delays. If your source/destination airport sees a lot of traffic, naturally the take-off and landing times will be affected. For this step, **we will extract info from** https://en.wikipedia.org/wiki/List_of_the_busiest_airports_in_the_United_States But this wikipedia page was updated recently, so I am **using a previous version of the wikipedia page of 13 April 2023** from this URL: https://en.wikipedia.org/w/index.php?title=List_of_the_busiest_airports_in_the_United_States&oldid=1149689977
- Collate the dataset into **combined_data_traffic**: ultimate dataset with all the relevant info from previous tables.
- Treat missing values:
id                                 0
Airline                            0
Flight                             0
AirportFrom                        0
AirportTo                          0
DayOfWeek                          0
Time                               0
Length                             0
Delay                              0
type_source_airport               31
elevation_ft_source_airport       31
runway_count_source_airport       31
type_dest_airport                 31
elevation_ft_dest_airport         31
runway_count_dest_airport         31
iata_code_source               83582
data_2021_source_airport       83582
iata_code_dest                 83531
data_2021_dest_airport         83531
Founded                        83601
- For type, elevation_ft, and runway_count values, we will **find the relevant info for 'CYS' i.e 'Cheyenne Regional Jerry Olson Field'** from previous datasets and add them to the combined_data_traffic dataset.
- We will **remove the iata_code columns for source and destination airport** since the AirportFrom and AirportTo columns have the same info. 
- For Founded column we can **extract the information about the founding year of airlines (US, EV, and CO) from the internet** and add the values in the missing rows. 
- For traffic data 2021 of source and destination airports, we will first **impute missing values with the median() value of the airport**. And remove the remaining left after the process.
![image](https://github.com/assr-droid/US-Airlines-Delay-Analysis-2023-/assets/75217839/98000f14-298f-4924-ad89-36baa39da227)
___________
**According to data provided, around 70% of the flights are delayed for Southwest Airlines. Visualize to compare the same for other airlines.**
- Calculate the % of delays for SouthWest Airlines and cross-check the stats in first task.
round((cdt[cdt.Airline == southwestid].Delay.sum()/
      cdt[cdt.Airline == southwestid].Delay.size)*100)
- Plot a bar chart:
-![image](https://github.com/assr-droid/US-Airlines-Delay-Analysis-2023-/assets/75217839/be793835-0a60-403a-ac7b-18e766052172)
- **As we can see in the above bar chart, SouthWest airlines has the most delayed flgihts i.e. 69.8% to be exact.**
**Mesa** is the airlines with least delays.

