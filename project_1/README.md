### Overview
This readme file is an executive summary of the problem statement, data dictionary, datasets used, key findings, and the conclusions/recommendations from this analysis.

---

### Problem Statement

You work for the Land Transport Authority. Every year, there are road accidents leading to injuries and even fatalities, which could be due to adverse weather conditions. Factors such as slippery roads and lower visibility increases the risk of traffic incidents. 

This analysis aims to allow better planning of traffic management based on weather patterns, to reduce the number of road accidents.

---

### Data Dictionary

|Feature|Type|Dataset|Description|
|---|---|---|---|
|year_month|object|Period of reference|Year-Month|
|no_of_rainy_days|integer|rainfall-monthly-number-of-rain-days|Total number of days with rain|
|total_rainfall|float|rainfall-monthly-total|Total rainfall in mm| 
|accidents_with_fatalities|integer|road_accidents_fatal|Total number of accidents leading to fatalities|
|accidents_with_injuries|integer|road_accidents_injuries|Total number of accidents leading to injuries|
|year|object|Period of reference|Year|
|month|object|Period of reference|Month|
|average_rainfall|float|total_rainfall / no_of_rainy_days|Average rainfall per rainy day in mm|
|total_accidents|integer|accidents_with_fatalities + accidents_with_injuries|Total number of accidents|


### Datasets

There are 5 datasets included in the [`data`](./data/) folder for this project. These correponds to rainfall and traffic accident information. 

* [`rainfall-monthly-number-of-rain-days.csv`](./data/rainfall-monthly-number-of-rain-days.csv): Monthly number of rain days from 1982 to 2022. A day is considered to have “rained” if the total rainfall for that day is 0.2mm or more.
* [`rainfall-monthly-total.csv`](./data/rainfall-monthly-total.csv): Monthly total rain recorded in mm(millimeters) from 1982 to 2022
* [`road_accidents_fatal.csv`](./data/road_accidents_fatal.csv): Monthly total number of accidents leading to fatalities from 2008 to 2022 (Source:https://www.police.gov.sg/-/media/1F7F9460FD8F48928B6DEFE096414975.ashx Table 1B)
* [`road_accidents_injuries.csv`](./data/road_accidents_injuries.csv): Monthly total number of accidents leading to injuries from 2008 to 2022 (Source:https://www.police.gov.sg/-/media/1F7F9460FD8F48928B6DEFE096414975.ashx Table 1C)
* [`merge_data.csv`](./data/merge_data.csv): Merged dataset of the other 4 datasets

---

### Exploratory Analysis

In this analysis, we would like to explore if number of accidents increases during adverse weather conditions.

Adverse weather conditions could be identified through:
1) High total rainfall in the month - indicates the general weather condition of the month in terms of how much it rained
2) High number of rainy days in the month - indicates the frequemcy of rain occurences during the month
3) Higher average daily rainfall in the month - indicates the intensity of rain during the rainy days e.g. drizzle vs storm

For the first part of the analysis, we looked at general weather trends:
1) Which month have the highest and lowest total rainfall in 1990, 2000, 2010 and 2020?
2) Which year have the highest and lowest total rainfall in the date range of analysis?
3) Which month have the highest and lowest number of rainy days in 1990, 2000, 2010 and 2020?
4) Which year have the highest and lowest number of rainy days in the date range of analysis?
5) Are there any outliers months in the dataset?

For the second part of the analysis, we narrowed down to the period 2008-2021 where the traffic accidents data was available. We then aggregated the data by month, and identified months with high number of accidents, before trying to identify correlations with weather-related data.
1) Which months had highest number of accidents?
2) Which months had highest number of rainy days?
3) Which months had highest total rainfall?
4) Which months had highest number of accidents with fatalities?
5) Which months had highest number of accidents with injuries?
6) Which months had had highest average daily rainfall?
7) Is there any correlation between number of rainy days and total rainfall?
8) Is there any correlation between number of accidents with number of rainy days?
9) Is there any correlation between number of accidents with total rainfall?
10) Is there any correlation between number of accidents with average daily rainfall?

Our key findings from the explorator analysis are:
1) January and August had highest number of accidents.
2) November and December had highest number of rainy days.
3) December and November had highest total rainfall.
4) December and January had highest number of accidents with fatalities.
5) Auguest and January had highest number of accidents with injuries.
6) December and January had highest average daily rainfall.
7) There is high correlation of 0.85 between number of rainy days and total rainfall.
8) There is high correlation of 0.72 between number of accidents with fatalities and average daily rainfall.
9) The high correlation between number of accidents with fatalities and average daily rainfall could partly be due to outlier number of accidents in months of Dec.
10) A scatterplot of number of accidents with fatalities vs average daily rainfall showed no apparent pattern between the two variables.

---

### Conclusions/Recommendations

**Conclusions:** 
Although initial exploration showed potential correlation between number of accidents with fatalities and the intensity of rain, further analysis showed no conclusive link between the two.

Other independent variables also did not show any correlation.

**Recommendations:** 
As limited features were used here, we recommend to further the analysis by including more features such as visibility levels, surface temperature, humidity, etc, which would allow for more comprehensive analysis.

Although there were no conclusive findings, we still recommend to place higher emphasis on months with higher rainfall such as Nov-Jan (northeast monsoon period) as we would logically expect the risk of traffic accidents to be higher during rain, especially storms, due to reduced visibility and slippery roads.

---

