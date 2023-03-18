### Overview
This readme file is an executive summary of the problem statement, data dictionary, datasets used,  , and the conclusions/recommendations from this analysis.

---

### Problem Statement

Twitter needs a classification model that maximizes the recommendation of Formula 1 related tweets to users who have indicated interest in the topic, while minimizing the recommendation of other auto racing related tweets. 

This project aims to build a classifier that is able to differentiate Formula 1 related tweets from MotoGP related tweets.

---

### Data Dictionary

| Key Feature | Type | Description | 
| --- | --- | --- | 
| `date` | Object | Date of tweet |
| `tweet` | Object | Tweet |
| `text` | Object | Tweet excluding hashtags/mentions |
| `hashtag` | Object | Hashtags found in tweet |
| `mention` | Object | Mentions found in tweet |
| `username` | Categorical | User who posted tweet |
| `like_count` | Numerical | Number of likes |
| `retweet_count` | Numerical | Number of retweets |
| `tweet_length` | Numerical | Character length of tweet |
| `tweet_word_count` | Numerical | Number of words in tweet |

### Datasets

There are 3 datasets included in the [`data`](./data/) folder for this project. These correponds to rainfall and traffic accident information. 

* [`train_data.csv`](./data/train_data.csv): Tweets scrapped from Formula 1 and MotoGP Twitter accounts from 1st Jan 2022 - 28th Feb 2023.
* [`train_data_cleaned.csv`](./data/train_data_cleaned.csv): Processed tweets and additional features from `train_data.csv`.

---

### Exploratory Data Analysis

Key findings from EDA:
1. MotoGP tweets are consistently longer than F1 tweets.
2. MotoGP tweets consistently uses more words than F1 tweets.
3. F1 tweets consistently get more likes than MotoGP tweets.
4. F1 tweets consistently get more retweets than MotoGP tweets.
5. Urls are often included in the tweets.

Conclusion of EDA: 
1. We can potentially include tweet length, tweet word count, number of likes, and number of retweets as features to improve model performance, as we have seen that F1 and MotoGP tweets are consistently different in these features.
2. We should remove URLs when preprocessing our data, before building our model.

### Preprocessing

In this section, we will be preprocessing our data so that we can build our model with clean data. We will try to remove urls, stopwords, quotation marks, emoticons, and other content in the tweets that are either noise, or not useful for building our model.

We will also create new columns for hashtags, mentions, and text (which are tweets after removing hastags and mentions) to further breakdown the tweets to identify which features are useful for building our model.

After cleaning the tweets and texts, we will tokenize and lemmatize these two features to reduce each word to its base form. This will allow us to remove overlapping words and reduce the number of unique tokens to be used for our model.

Finally, we will explore the processed dataset to have a sense of the top words which appear most frequently.

---

### Modeling

In this section we will build a models using Naive Bayes and Logistic Regression techniques.

We will adopt the following approach in our model building:

1. Build a model (M1) using tweet as feature and evaluate the model.
2. Build a model (M2)  using hashtag as feature and evaluate the model.
3. If hashtag is overly dominant as a predictor, re-evaluate performance of M1 on tweets excluding hashtag/mentions.
4. Try to improve M1 by incorporating other features such as tweet length, tweet word count, number of likes, and number of retweets.
5. Determine best threshold to maximize recall and F1 scores.

Rationale behind approach:
As our data was scrapped from the official accounts of Formula 1 and MotoGP, 97% of tweets included hashtags to other official accounts such as the drivers or the constructor teams. However, our aim is to create a classifier that will recommend Formula 1 related tweets from anyone to users who have chosen to follow the topic of Formula 1. Individual users are less likely to include hashtags or mentions as they are not as strictly managed as official accounts. Hence, we want to make sure our model works sufficiently well for tweets that do not have any hashtag or mention. 

Since our aim is to accurately recommend Formula 1 related tweets to the news feeds of interested users, we will be focusing on maximizing recall, so that we will capture as many of the actual related tweets as possible. However, we do not want our model to be predicting all tweets as Formula 1 related, which would give us 100% recall but also super low precision. Hence we will try to maximize recall while maintaining a good ROC AUC score, which reflects how well the model is able to separate the classes.

---

### Model Evaluation

After running several iterations of both Naive-Bayes and Logistic Regression models, the results are shown below.

#### Naive-Bayes

|Model|Testing Accuracy|Recall|Precision|F1 Score|ROC AUC
|---|---|---|---|---|---|
|M1 (tweet-on-tweet)|0.994|0.991|0.995|0.993|---
|M1 (tweet-on-text)|0.883|0.794|0.910|0.848|---
|M2 (hashtag-on-hashtag)|0.993|0.984|1.000|0.992|---
|M3 (all-on-tweet)|0.919|0.879|0.919|0.899|0.961
|M3 (all-on-text)|0.916|0.875|0.916|0.895|0.957

#### Logistic Regression

|Model|Testing Accuracy|Recall|Precision|F1 Score|ROC AUC
|---|---|---|---|---|---|
|M1 (tweet-on-tweet)|0.992|0.980|0.999|0.990|---
|M1 (tweet-on-text)|0.772|0.453|0.980|0.620|---
|M2 (hashtag-on-hashtag)|0.991|0.978|1.000|0.989|---
|M3 (all-on-tweet, 0.5)|0.993|0.983|0.999|0.991|0.999
|M3 (all-on-text, 0.5)|0.889|0.734|0.994|0.844|0.993
|M3 (all-on-tweet, 0.4)|0.995|0.990|0.997|0.993|---
|M3 (all-on-text, 0.4)|0.917|0.805|0.987|0.888|---
|M3 (all-on-tweet, 0.3)|0.994|0.991|0.994|0.992|---
|M3 (all-on-text, 0.3)|0.940|0.871|0.981|0.923|---

---

### Conclusion

#### Recommendation

To recap our problem statement, Twitter needs a classification model that maximizes the recommendation of Formula 1 related tweets to users who have indicated interest in the topic, while minimizing the recommendation of other auto racing related tweets. This project aims to build a classifier that is able to differentiate Formula 1 related tweets from MotoGP related twxeets.

With that in mind, it is important to develop a classifier that is not only able to have high accuracy, but also high recall rates and F1-scores. This is to prioritize capturing all Formula 1 related tweets as much as possible, while avoiding spam of non Formula 1 related tweets to show on the news feeds of users who are interested in Formula 1. Considering that our training data is scraped from official twitter accounts of Formula 1 and MotoGP, where the prevalence of hashtags is high, we would also like to take into account the performance of our model on tweets without hashtag/mention, which is more common among individual user accounts.

Given this consideration, we recommend to choose the Logistic Regression (M3, 0.3 threshold), which performs well on tweets both with and without hashtags. The high ROC AUC score also implies that the model is a good fit for predicting the classification of tweets into Formula 1 vs MotoGP related.

#### Limitation/Suggested area for improvement

Although our model performed very well, one key limitation is that Tweet length, word count, number of likes, number of retweets are all features which could vary greatly among individual users, hence performance during actual deployment may drop further.

A potential area for improvement is to use tweets from individual users instead to train our model further. We could deploy our model to first predict the classification of these tweets from individual users, then cross validate manually to correct the errors in predictions. After that, we can feed the validated data into our model to refine it further. In this manner, the model will be trained using the same type of inputs we can expect when it is eventually deployed.

---


