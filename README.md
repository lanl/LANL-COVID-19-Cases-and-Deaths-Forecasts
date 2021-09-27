# LANL COVID-19 Cases and Deaths Forecasts

## Authors

* Lauren A. Castro
* Geoffrey Fairchild
* Isaac Michaud
* Dave Osthus

## Description
This repository contains an archive of the forecasts published on [https://covid-19.bsvgateway.org/](https://covid-19.bsvgateway.org/) from 2020-04-05 until 2021-09-27. Forecasts were published every Monday and Thursday unless otherwise noted in [Key Dates](#Key-Dates). Forecasts published on Mondays used observed data through Sunday; forecasts published on Thursdays used observed data through Wednesday.  

All forecasted quantities were saved as csv files and conform loosely to the standards set by [https://covid19forecasthub.org/](https://github.com/reichlab/covid19-forecast-hub/blob/master/data-processed/README.md). Each file contains historical observations starting with 2020-01-22 and ending on the last observed date and the forecasts for the next 48 days or 6 weeks.  All forecast files names are prefixed with the last observed date used in the forecast. For example, a forecast published on the website on `2021-09-20` (a Monday) would have ``2021-09-19`` in its file name.  

A metadata file (`forecast_metadata.json`) is also provided for ease of searching.  

Over the course of 18 months, we implemented two different forecasting models. We switched to Version 2 (V2) of our forecasting model on **2020-10-28**. 

## Version 1: April 5, 2020 - October 25, 2020 

### Methodology 

### Forecast Files

Five forecast types were produced using V1 of the forecasting model at the US and global levels. See [Key Dates](#Key-Dates) for exact start and end dates for the `confirmed_cases_peak_timing` and `*_incidence_*` forecasts. 

| Name   | Time Frame |  Forecast Horizon | Description |
| ------ |    ------    |   ------    |  ------    | 
| `confirmed_incidence_quantiles` | daily | 48 days | forecasted daily confirmed cases      |
| `confirmed_quantiles`           | daily | 48 days | forecasted cumulative confirmed cases |
| `deaths_incidence_quantiles`    | daily | 48 days | forecasted daily deaths               |
| `deaths_quantiles`              | daily | 48 days | forecasted cumulative deaths          |
| `confirmed_cases_peak_timing`   | weekly | 6 weeks | cumulative probability of peak occuring in week of `weekdate`  |


The full names of the output CSVs from V1 are in the following format:
-- **fcstdate** is the forecast date (e.g., 2020-10-27)
-- **target** can be `cases` or `deaths`
-- **type** can be `incidence` or blank for cumulative
-- **quantiles** indicates the forecast is summarized into quantiles
-- **geography** can be `us` or `weekly`
-- **website**  indicates the forecast was published. Note, early forecast CSVs are missing this tag. 


### Output Column Descriptions

There are 23 quantiles forecasted for each quantity. The quantile columns are names "q.\*" where \* is the quantile. All of the names are:
```r
 [1] "q.01"  "q.025" "q.05"  "q.10"  "q.15"  "q.20"  "q.25"  "q.30"  "q.35"  "q.40"  "q.45"  "q.50"  "q.55"  "q.60"  "q.65"
[16] "q.70"  "q.75"  "q.80"  "q.85"  "q.90"  "q.95"  "q.975" "q.99"
```

V1 files have additional columns which identify the locale and date of either the forecast or historical observation.

| Column name | Dtype  | Description | Range or Example | Notes  |
| ------      | ------ |  ------     | ------           | ------ |
| `dates` | string | last observed date | "2020-04-01" through current | All observations on and before fcst_date are used for make forecasts|
| `simple_state` | string | unique id for each locale | e.g. "newmexico" | Lower case state or country name with spaces and punctuation removed |
| `state` | string | Name of locale as reported by JHU CSSE | e.g. "New Mexico" | Includes capitalization, spaces, and punctuation |
| `obs` | bool | indicator of whether the entry was observed | 0,1 | All entries dated on or before fcst_date are observed   | 
| `truth_confirmed` | integer | reported number of confirmed cases | [0,∞) | Applies to cumulative and incident records. Forecasted entries have NA. |  
| `truth_deaths`    | integer | reported number of deaths | [0,∞) | Applies to cumulative and incident records. Forecasted entries have NA. | 
| `fcst_date` | string | last observed date | "2020-04-01" through current | All observations on and before fcst_date are used for make forecasts|
| `big_group`  | string | WHO global regions| e.g. "NORTHERN AFRICA AND WESTERN ASIA" | Only occures in global forecasts. Not in US state forecast output files. | 


## Version 2: October 28, 2020 -- September 27, 2021

### Methodology 

A description of V2 of the forecasting model, COVID-19 Forecasts using Fast Evaluations and Estimation (COFFEE) can be accessed on arXiv.  
### Forecast Files
Eight types of forecasts were produced using V2 of the forecasting model. Daily forecasts were made for the 48 days following the last observed day. Weekly forecasts were made for the 6 following epiweeks after the current observed epiweek. Because the COVID-19 Forecast Hub requests forecasts for the first full epiweek (except for Mondays which include data observed the previous Sunday) any forecast made with data ending on any day other than Saturday or Sunday will skip the current epiweek and aggregate daily forecasts beginning the following unobserved Sunday. **This skip results in a missing epiweek entry in the weekly aggregation output files for the current epiweek.** Also note that because of this definition of epiweek, the COVID-19 Forecast Hub are one week ahead of the CDC epiweek defined in the `lubridate` package.

| Name   | Time Frame |  Forecast Horizon | Description 
| ------ |    ------    |   ------    |  ------   
| `incidence_daily_cases` | daily | 48 days | forecasted daily confirmed cases     
| `cumulative_daily_cases`  | daily | 48 days | forecasted cumulative confirmed cases 
| `incidence_daily_deaths`    | daily | 48 days | forecasted daily deaths               
| `cumulative_daily_deaths`              | daily | 48 days | forecasted cumulative deaths          
| `incidence_weekly_cases` | weekly | 6 weeks | forecasted weekly confirmed cases            |
| `cumulative_weekly_cases`           | weekly | 6 weeks | forecasted weekly cumulative confirmed cases |
| `incidence_weekly_deaths`    | weekly | 6 weeks | forecasted weekly deaths                     |
| `cumulative_weekly_deaths`              | weekly | 6 weeks | forecasted weekly cumulative deaths          |

The full names of the output CSVs from V2 are in the following format: 
- **fcstdate** is the forecast date (e.g., `2020-10-27`)
- **geography** can be  `us` or `global`
- **type** can be `incidence` or `cumulative`
- **resolution** can be `daily` or `weekly`
- **target** can be `cases` or `deaths`
- **website** indicates the forecast was published

### Output Columns

As with V1, there are 23 quantiles forecasted for each quantiy. The quantile columns are names "q.\*" where \* is the quantile. All of the names are:
```r
 [1] "q.01"  "q.025" "q.05"  "q.10"  "q.15"  "q.20"  "q.25"  "q.30"  "q.35"  "q.40"  "q.45"  "q.50"  "q.55"  "q.60"  "q.65"
[16] "q.70"  "q.75"  "q.80"  "q.85"  "q.90"  "q.95"  "q.975" "q.99"
```
The remaining columns identify the locale and date of either the forecast or historical observation. Not all columns appear in all output files. For example, instead of a `date` column weekly_confirmed_incidence_quantiles and weekly_deaths_incidence_quantiles have a `start_date` and `end_date` fields that correspond to the beginning and end dates of the observed data used to compute the observed incidence for historical data and to the beginning and end date of the forecasted days used to create the incident forecast. **The CDC epiweek of these date ranges is different than the COVID-19 Forecast Hub defined epiweek.** For weekly_confirmed_quantiles and weekly_deaths_quantiles, instead of a `date` column they have an `end_date` field which describes the last date of the COVID-19 Forecast Hub epiweek (which is always a Saturday) or the forecasted cumulative quantity according to the week-ahead rule used by the COVID-19 Forecast Hub.

| Column name | Dtype  | Description | Range or Example | Notes  |
| ------      | ------ |  ------     | ------           | ------ |
| `fcst_date` | string | last observed date | "2020-04-01" through current | All observations on and before fcst_date are used for make forecasts|
| `date` | string | date of the observation or forecast | "2020-01-22" through fcst_date + 48 days |   |
| `key` | string | unique id for each locale | e.g. "newmexico" | Lower case state or country name with spaces and punctuation removed |
| `epiyear`          | integer | CDC defined epiyear | \{2020,2021,...\} | The first couple of days of the new year are in the previous epiyear |
| `fcst_hub_epiweek` | integer | COVID-19 Forecast Hub defined epiweek | [5,52] | Epiweeks reset on the first Sunday of the new year|
| `start_date` | string | first day of epiweek observation or forecast | "2020-04-01" through fcst_date + 48 days | Always a Sunday   |
| `end_date`   | string | last  day of epiweek observation or forecast | "2020-04-01" through fcst_date + 48 days | Always a Saturday |
| `week_ahead` | integer | number of epiweeks ahead of fcst_date of forecast | \{1,2,3,4,5,6\} |    | 
| `name` | string | Name of locale as reported by JHU CSSE | e.g. "New Mexico" | Includes capitalization, spaces, and punctuation |
| `obs` | bool | indicator of whether the entry was observed | 0,1 | All entries dated on or before fcst_date are observed   | 
| `truth_confirmed` | integer | reported number of confirmed cases | [0,∞) | Applies to cumulative and incident records. Forecasted entries have NA. |  
| `truth_deaths`    | integer | reported number of deaths | [0,∞) | Applies to cumulative and incident records. Forecasted entries have NA. | 
| `big_group`  | string | WHO global regions| e.g. "NORTHERN AFRICA AND WESTERN ASIA" | Only occures in global forecasts. Not in US state forecast output files. | 


## Key Dates
Below we note significant changes to the format or type of output ("Output Change"), or dates when we did not adhere to the normal Monday/Thursday schedule of publishing forecasts due to model or data issues. 

| Forecast Date   | Type |  Description | Version 
| ------ | ------ |    ------    |    ------    |  
| `2020-04-05` | Output Change | First US daily cumulative forecasts published | V1
| `2020-04-15` | Output Change | First US daily incidence foreasts published |V1
| `2020-04-26` | Output Change | First global daily cumulative and incidence forecasts published | V1
| `2020-06-14` | Model issue | Published US/global output from 2020-06-13 | V1
| `2020-06-24` | Data issue  | Published US/global output from 2020-06-23 | V1
| `2020-06-24` | Output Change | Peak forecast terminated | V1
| `2020-10-28` | Output Change | V2 COFFEE implemented for US and global forecasts| V2
| `2020-12-23` -- `2021-01-02` | Output Change | Pause for winter closure - no forecasts published| V2
| `2021-01-06` | Model issue  | Published output from 2021-01-05 | V2
| `2021-03-17` | Model issue  | No forecasts published | V2
| `2021-05-30` | Model issue  | No forecasts published | V2
| `2021-06-10` | Output Change | Thursday forecasts terminated | V2
| `2021-09-27` | Output Change | Last US/global forecasts | V2


## LA-UR

This repository is approved for public release and is assigned number LA-UR-20-22749.
