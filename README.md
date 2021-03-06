# Churn Prediction with PySpark

## Project Overview
Churn Prediction is a very common problem in nowadays business, and has been applied widely. 
In this project I’m about to build an end-to-end machine learning model to predict churn customer based on a sample dataset of Sparkify users data and the Apache Spark Machine Learning framework. 
The model is capable of predict which users is likely to churn the music application service.

The dataset contains logs of user interaction. Using the user logs, we need to identify customers with propensity to churn so they can be offered promotions. We can identify factors which are significant indicators for churn.
In Part I we only use a subset of data (~128MB) to train our churn prediction models locally to save cost. 
In PART II we will train our predictive model on a large dataset (~12GB) of customer’s activities on a cloud service to take full advantage of Spark’s distributed computing framework.

PySpark is used to clean, wrangle and process data and perform modeling and tuning to build a churn prediction model.

## Part I: Exploratory

This part will serve as an exploration of how to make a churn-prediction model using PySpark, with the following steps included:

1. Data Preprocessing
* Load raw dataset from JSON
* Assess missing values

2. Exploratory Data Analysis (EDA)
* Define churn Cancellation Confirmation event 
* Transform data
* Overview of numerical and categorical columns
* Compare behavior of churn vs. non-churn users in terms of:
    * User interaction at different hours of the day
    * User interartion at different days of a week
    * User level (free vs. paid)
    * Event types (e.g. add a friend, thumbs up)
    
3. Engineer relevant features for our problem
* Create features on per user basis:
    * Latest user level
    * User lifetime (time since registration)
    * Gender of user
    * Number of sessions that user has engaged
    * Count of page each event type
* Remove strongly correlated features
* Transform features to have distributions closer to normal

4. Modeling
* Split dataset into training and testing sets
* Train binary classifier models, and evaluate models performance (Metric: F1 Score)
* The classifiers presented in this project are as the following:
    * Logistic Regression
    * Gradient-Boosted Trees Classifier
    * Random Forest Classifier
 
5. Select best model and features to fine-tune the final model with K-fold Cross-Validation

### Results
* Model performance on testing set:

| classifier          | F1 score    | Time |
| ------------------- | ----------- |------|
| Logistic Regression | 0.8033      | 50s  |
| GBTClassifier       | 0.8500      | 199s |
| Random Forest       | 0.8401      | 35s  |

*The model performance could be further improved by exploring more helpful features.

Explore feature importances:

![](./feature_importance.png)
* Churns relate to users who have received more advertisements, disliked songs more often than liked, and registered more recently.

## Part II: Fine-tune hyperameters on AWS EMR cloud cluster
The goal of this part is to fine-tune parameters of the final prediction model on AWS EMR cluster with config:
```commandline
Release: emr-5.20.0
Applications: Spark: Spark 2.4.0 on Hadoop 2.8.5
Instance type: m4.xlarge
Number of instance: 3
```
In this part we are going to use insights from exploratory Part I. Therefore the following steps have been applied:
* Load and clean data
* Engineer important features which are used to train the final model
* Split dataset into training and testing sets
* Train the final model with K-fold cross-validation
* Improve the model to get higher results and Evaluate

### Results
* Model performance on testing set after tuning hyperparameters:

| classifier          | F1 score    | Time |
| ------------------- | ----------- |------|
| Random Forest       | 0.8441      | 35s  |

*The model performance could be further improved by tuning broader ranges of hyperparameters.
