### Overview
This readme file is an executive summary of the problem statement, data dictionary, datasets used, key findings, and the conclusions/recommendations from this analysis.

---

### Problem Statement

A property consultancy has engaged you to write up an article on the key factors that affect the resale prices of HDB, to publish on their website.

This analysis aims to create a prediction model of resale prices that identifies the key features driving the prices.

---

### Data Dictionary

| Key Feature | Type | Description | 
| --- | --- | --- | 
| `floor_area_sqm` | Numerical | Floor area of the resale flat. |
| `lease_remaining` | Numerical | Number of years of lease remaining for resale flat as at transaction. |
| `Hawker_Within_2km` | Numerical | Number of hawkers within 2km of the resale flat. |
| `mrt_nearest_distance` | Numerical | Distance from the resale flat to the nearest MRT station. |
| `mid` | Numerical | Middle value of the range of levels that the resale flat is located in; used as an approximation of its level. |
| `Hawker_Nearest_Distance` | Numerical | Distance from the resale flat to the nearest hawker centre. |
| `town` | Categorical | Town that resale flat is located in. |

Data description of all features can be found at https://www.kaggle.com/competitions/dsi-sg-project-2-regression-challenge-hdb-price/data

### Datasets

There are 3 datasets included in the [`data`](./data/) folder for this project. These correponds to rainfall and traffic accident information. 

* [`train.csv`](./data/train.csv): Resale HDB transactions from March 2012 to April 2021 with 77 features and resale prices. 
* [`test.csv`](./data/test.csv): Resale HDB transactions from March 2012 to April 2021 with 77 features, without resale prices.
* [`kaggle_sub.csv`](./data/kaggle_sub.csv): Transaction ID and predicted resale prices of test dataset for Kaggle submission. 

---

### Exploratory Analysis

In this analysis, we would like to explore the following:

Are there missing data in the dataset?
1) Which features have missing data?
2) What does the missing data mean?
3) Can we replace the missing data with appropriate values?

Are there multiple features representing the same thing? If yes, can we just keep one and drop the rest?
1) Are there overlaps for features representing location?
2) Does flat type/flat model have relation with resale price? Can they be replaced by floor area?
3) Are there overlaps for features representing floor level?
4) Do we need transaction YearMonth when we already have transaction year and transaction month?

Are there any features which are over-represented by any values? If yes, should we still include them for analysis?
1) Is residential over-represented by any value?
2) Is commercial over-represented by any value?
3) Is bus interchange over-represented by any value?
4) Is mrt interchange over-represented by any value?
5) Is precinct pavilion over-represented by any value?
6) Is multistorey carpark over-represented by any value?
7) Is market hawker over-represented by any value?
8) Is pri sch affiliation over-represented by any value?
9) Is affiliation over-represented by any value?

Are there other features which are not relevant for prediction of resale prices?
1) Are rental related features relevant?
2) Are names of schools/mrt stations relevant?
3) Are geographical coordinates relevant?

Are there any new features we need to create using existing features?
1) Should we create lease remaining period, rather than using age of HDB?

Our key findings from the explorator analysis are:
1) We just need one feature for address, so keep town and remove block, street_name, address, latitude, longitude, postal, planning_area.
2) The storey range can be represented by mid (no overlaps between ranges), so keep mid and remove upper, lower, mid_storey, storey_range, max_floor_area.
3) Residential is all "Y", so we can drop it as it will not make a difference to our model.
4) Names, Latitudes and Longitudes, of mrt/bus_stops/schs are not of interest here so we can drop them. We are more interested in nearest distance of these amenities.
5) We already have Year and Month so we can drop YearMonth.
6) Drop id as it is not needed.
7) Floor_area is more important than the flat types/models so we can keep floor_area_sqm and drop flat_type, flat_model, full_flat_type, floor_area_sqft. 
8) Number of 1/2/3/4/5/exec/multigen/studio sold and rental data unlikely to be strong predictors, so we can drop them.
9) We can keep lease_remaining and drop lease_commence_date, hdb_age, year_completed

---

### Preprocessing

Before creating our prediction model, we processed our data in the following ways:
1) Replace missing data with 0 value for Mall_Within_500m/Mall_Within_1km/Mall_Within_2km/Hawker_Within_500m/Hawker_Within_1km/Hawker_Within_2km.
2) Replace missing data with median for Mall_Nearest_Distance.
3) Create new feature to represent lease remaining years.
4) Drop the features identified via exploratory analysis.
5) Dummy code categorical features.
6) Apply train/test split on our training data.
7) Scale the features.

---

### Modeling

The mean of resale prices on the train dataset was used as our baseline model. This leads us to an error of $143307 in our predictions on average.

We then created 4 regression models and compared against the baseline model to see if there were any improvements in predictions. The scores are as follows:

-------Ordinary Least Square (OLS)-------
R2 (train) : 0.861604351820485
R2 (test) : 0.8607720639769438
RMSE : $53281.68985168486
-------Ridge-------
R2 (train) : 0.8616043382424211
R2 (test) : 0.860771529350169
RMSE : $53281.79215080483
-------Lasso-------
R2 (train) : 0.8616043518199973
R2 (test) : 0.8607720570239812
RMSE : $53281.69118211324
-------ElasticNet-------
R2 (train) : 0.8189408705337543
R2 (test) : 0.8172969888516677
RMSE : $61036.222882946284

OLS, Ridge, and Lasso performed very similarly in terms of error and model accuracy. These 3 models also identified the same top features below in terms driving the resale price (based on coefficients).

1) Floor area
2) Lease remaining
3) Hawker within 2km
4) Mrt nearest distance
5) Mid 
6) Hawker nearest distance
7) Town

Next we created interaction features between the non-categorical features (1-6), and re-created the regression models using only these top features.

We found the following interactions.

Interaction between features:

1) floor_area_sqm & mid
2) lease_remaining & mrt_nearest_distance
3) mrt_nearest_distance & hawker_nearest_distance
4) lease_remaining & hawker_within_2km

Interaction within feature:

1) floor_area_sqm
2) lease_remaining
3) mrt_nearest_distance

---

### Model Evaluation

The 4 regression models were evaluated on the error in predictions (Root Mean Square Error) and the R-square score. The best model would be the one with the highest R-square and lowest RMSE.

-------OLS-------
R2 (train) : 0.8792451908590708
R2 (test) : 0.8777536566969599
RMSE : 49926.68560669114
-------Ridge-------
R2 (train) : 0.8792449354332882
R2 (test) : 0.8777512030093102
RMSE : 49927.18666003084
-------Lasso-------
R2 (train) : 0.8792451327443147
R2 (test) : 0.8777531244228806
RMSE : 49926.79429972429
-------ElasticNet-------
R2 (train) : 0.8518937322654475
R2 (test) : 0.8506011968254495
RMSE : 55193.553622659834

OLS, Ridge, and Lasso performed very similarly again in terms of error and model accuracy, and slightly better than the first round. We will use OLS as our model as it has the lowest error and highest accuracy, and also since there is no obvious improvement using ridge or lasso models, we will fall back on our basic regression model.

---

### Conclusions

Using the OLS model, we want to identify the key features which affects the resale prices. For linear regression models, the coefficient of each feature can be interpreted as the direct impact to the predicted outcome. Hence, we will identify the key features by looking at the features with the highest absolute coefficients in our prediction model. Hence, the key features that affect the resale price of a HDB are:

1) Floor area
2) Lease remaining
3) Hawker within 2km
4) Mrt nearest distance
5) Mid 
6) Hawker nearest distance
7) Town

---

