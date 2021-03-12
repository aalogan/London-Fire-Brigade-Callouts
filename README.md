# How Many Fire Engines Next Week?:<br /> Using Weather Data to Predict London Fire Brigade Callouts

Using data provided by the London Fire Brigade (LFB) and the Met Office, I analysed LFB callouts in relation to the weather to predict and classify callouts. :firefighter: :firefighter: :firefighter: 

## Table of Contents
* [Project Goals](#project-goals)
* [Data Sources](#data-sources)
* [Data Preparation](#data-preparation)
* [Time Series Analysis](#time-series-analysis)
* [Regression Modelling](#regression-modelling)
* [Callout Prediction](#callout-prediction)
* [Classification Modelling](#classification-modelling)
* [Next Steps](#next-steps)


## Project Goals
The London Fire Brigade is the largest in the world and has 100,000 callouts per year. Weather has a significant effect on ambulance and police callouts (people don’t riot when it’s raining and respiratory emergencies increase with temperature). I wanted to predict callout numbers for the next week based on the current weather forecast. In addition, a first look at the data showed that over half the callouts are false alarms so I wanted to see if a classification approach could be useful to predict a false alarm. The map below gives an idea of the scope of the project. <br />
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
[London Datastore LFB Incidents](https://data.london.gov.uk/dataset/london-fire-brigade-incident-records). The London Datastore is an open portal with a lot of information about London (well worth a look). Two csv files from here listing all LFB callouts from 2013-2016 and 2017-2020. <br />

---

#### London Boroughs <br />
[London Datastore Borough Boundaries](https://data.london.gov.uk/dataset/statistical-gis-boundary-files-london). A collection of shape files from here for use with GeoPandas (and contextily) for the map above. <br />

---

#### Hourly Weather to 2019 <br />
[Met Office Archives](https://catalogue.ceda.ac.uk/uuid/dbd451271eb04662beade68da43546e1). CEDA MIDAS open datasets of hourly records fro met office weather stations across the UK (including Heathrow). Twelve csv files from here (three per year for different readings). <br />

---

#### Hourly Weather 2020 <br />
[Meteostat](https://github.com/meteostat). A very useful python friendly archive dataset of worlwide weather readings (with a github page and easy API), needed for the 2020 weather data (and filling in gaps for the other years). The data needed some feature engineering to match the Met Office data. <br />

---

#### Sunrise and Sunset Times <br />
[Earth System Research Laboratories](https://www.esrl.noaa.gov/gmd/grad/solcalc/calcdetails.html) A spreadsheet to download to calculate the official sunrise and sunset times anywhere on earth. <br />

---

#### Actual and Forecast Weather <br />
[Met Office Forecast London](https://www.metoffice.gov.uk/weather/forecast/gcpvj0v07#?). This webpage (which was scraped for the data) gives the current forecast for the rest of today and the following six days for London. <br />
[Met Office Actual Heathrow](https://www.metoffice.gov.uk/weather/forecast/gcpvj0v07#?). This webpage (also scraped) gives an additional variable (air pressure) used for the callout predictions. <br />

---

## Data Preparation
