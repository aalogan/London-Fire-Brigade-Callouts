# How Many Fire Engines Next Week?:<br /> Using Weather Data to Predict London Fire Brigade Callouts

Using data provided by the London Fire Brigade (LFB) and the Met Office, I analysed LFB callouts in relation to the weather to predict and classify callouts. :firefighter: :firefighter: :firefighter: 

## Table of Contents
* [Project Goals](#project-goals)
* [Data Sources](#data-sources)
* [Data Preparation and EDA](#data-preparation-and-eda)
* [Time Series Analysis](#time-series-analysis)
* [Regression Modelling](#regression-modelling)
* [Callout Prediction](#callout-prediction)
* [Classification Modelling](#classification-modelling)
* [Next Steps](#next-steps)


## Project Goals
The London Fire Brigade is the largest in the world and has 100,000 callouts per year. Weather has a significant effect on ambulance and police callouts (people don’t riot when it’s raining and respiratory emergencies increase with temperature). I wanted to predict callout numbers for the next week based on the current weather forecast. In addition, a first look at the data showed that over half the callouts are false alarms so I wanted to see if a classification approach could be useful to predict a false alarm. Successful prediction results will be useful for resource planning and successful classification results may be useful for helping to reduce false alarms (over half the callouts are false alarms). The map below gives an idea of the scope of the project. <br />
<img src = "Assets/images/calloutsmap1.png">

**Predictions:** Predicting number LFB callouts in the short term and likely classification of callout. <br />
**Goals:** Reveal which features have the most influence on callout numbers and callout classification. <br />

## Data Sources
* [LFB Incidents](#LFB-Incidents-)
* [London Boroughs](#London-Boroughs-)
* [Hourly Weather to 2019](#Hourly-Weather-to-2019-)
* [Hourly Weather 2020](#Hourly-Weather-2020-)
* [Sunrise and Sunset Times](#Sunrise-and-Sunset-Times-)
* [Actual and Forecast Weather](#Actual-and-Forecast-Weather-) 

#### LFB Incidents <br />
[London Datastore LFB Incidents](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records). The London Datastore is an open portal with a lot of information about London (well worth a look). 2 csv files from here listing all LFB callouts from 2013-2016 and 2017-2020. <br />

---

#### London Boroughs <br />
[London Datastore Borough Boundaries](https://data.london.gov.uk/dataset/statistical-gis-boundary-files-london). A collection of shape files from here for use with GeoPandas (and contextily) for the map above. <br />

---

#### Hourly Weather to 2019 <br />
[Met Office Archives](https://catalogue.ceda.ac.uk/uuid/dbd451271eb04662beade68da43546e1). CEDA MIDAS open datasets of hourly records from Met Office weather stations across the UK (including Heathrow). 12 csv files from here (3 per year for different readings). <br />

---

#### Hourly Weather 2020 <br />
[Meteostat](https://github.com/meteostat). A very useful python friendly archive dataset of worlwide weather readings (with a github page and easy API), needed for the 2020 weather data (and filling in gaps for the other years). The data needed some feature engineering to match the Met Office data. <br />

---

#### Sunrise and Sunset Times <br />
[Earth System Research Laboratories](https://www.esrl.noaa.gov/gmd/grad/solcalc/calcdetails.html) A spreadsheet to download to calculate the official sunrise and sunset times anywhere on earth over a calendar year. <br />

---

#### Actual and Forecast Weather <br />
[Met Office Forecast London](https://www.metoffice.gov.uk/weather/forecast/gcpvj0v07#?). This webpage (which was scraped for the data) gives the current forecast for the rest of today and the following six days for London. <br />
[Met Office Actual Heathrow](https://www.metoffice.gov.uk/weather/forecast/gcpvj0v07#?). This webpage (also scraped) gives an additional variable (air pressure) used for the callout predictions. <br />

---

## Data Preparation and EDA 
* [Preparation](#preparation-)
* [Time Series and Regression EDA](#time-series-and-regression-eda-)
* [Classification EDA](#classification-eda-)

#### Preparation <br />
The 2 LFB files have slightly different columns and format.They were joined, reformatted (inc datetime index) for all incidents 2016-2019 inclusive with non-relevant columns dropped. Weekend, holiday and lockdown columns added.
The 12 Met Office csv files (hourly weather 2016-2019) were joined and reformatted (inc datetime index) with non-relevant columns dropped. The 1 Meteostat file (2020 with features engineered to match Met Office features) joined. Weather data in GMT, LFB data in GMT/BST so weather data adjusted to suit. ‘Islight’ column added from calculated sunrise/sunset times.
LFB and weather data joined and weather data padded/backfilled appropriately. The final step at this stage was to feature engineer a column to take account of low temperatures on the assumption that a cold spell is likely to cause more callouts. This was done by creating a coumn of (air temperature-13 )**2, so that low temperatures could be positively correlated with callout numbers. <br />

---

#### Time Series and Regression EDA <br />

I wanted to predict on callout numbers per hour so I needed to aggregate the data by hour to create callout counts per hour, resulting in a dataframe with 43498 rows. In addition, a column was feature engineered showing the prior year callout counts at the same hour (also meaning that the 29th  Feb dates needed to be dropped). The next step was to look at the data in more detail. Firstly looking at the counts over time. This chart clearly shows the annual seasonality (along with interesting spikes in Feb/Mar 2018 during a cold spell and June 2018 during a heatwave). <br />
<img src = "Assets/images/allcallouts.png"> <br />

Next looking at the mean callouts per hour over the whole period, we clearly see a daily seasonality (not many calls at 4 in the morning, with a peak in the early evening). <br />
<img src = "Assets/images/dailycallouts.png" height = "300"> <br />

Looking at the heatmap of correlations between the continuous variables, we can see that there are some variables correlated with the target variable (callouts) and no significant multicollinearity between the predictor variables. <br />
<img src = "Assets/images/heatmap.png"> <br />

Seeing the daily and annual seasonality, the first step was to explore the time series aspects of the data. initially looking at the autocorrelation and partial autocorrelation plots, where we can see a strong autocorrelation with the hour of the day. <br />
<img src = "Assets/images/autocorr.png"> <br />

---

#### Classification EDA <br />

Switching to the classification task (where we are looking at 3 different classes: Fire, Special Services and False Alarm), I looked at pairplots of relevant variables, where we can see no obvious clusters. <br />
<img src = "Assets/images/pairplot.png"> <br />


