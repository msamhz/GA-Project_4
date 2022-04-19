# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 4: Identifying and Controlling West Nile Virus Hotspots

## Background

The West Nile virus (WNV) is the leading cause of mosquito-borne disease in the continental United States. The virus is most commonly spread to people by the bite of an infected mosquito, although it can also spread through organ transplant, blood transfusions and breast milk ([CDC](https://www.cdc.gov/westnile/index.html)).

Most people infected with the WNV do not feel sick. Of those infected, 1 in 5 develop mild symptoms while 1 in 150 develop a serious, sometimes fatal, illness. Nonetheless, it is important to note that there is currently no human vaccine available. As such, the only way to reduce infection in people is by  reducing exposure to the virus ([WHO, 2017](https://www.who.int/news-room/fact-sheets/detail/west-nile-virus)).

## Problem Statement
As an analyst in the Disease And Treatment Agency in the City of Chicago, you have been tasked to predict when and where different species of mosquitos will test positive for WNV in the City of Chicago. Subsequently, a cost-benefit analysis will be conducted. This should include annual cost projections for various levels of pesticide coverage (cost) and the effect of these various levels of pesticide coverage (benefit).

This will help the City of Chicago and the Chicago Department of Public Health (CDPH) more efficiently and effectively allocate resources towards preventing transmission of this potentially deadly virus.

**Goal:**
In favour of reducing human fatalities and the possiibility of an uncontained outbreak, the team will focus on minimising false negatives over false positives. This would mean placing greater emphasise on `sensitivity` rather than `specificity` without sacrificing too much on `Accuracy`. We will also observe the `Precision` of the model to understand the quality of a positive prediction made by the model (ie. the increase in false positives for additional true positives).


## Method
**1) Data Cleaning and Exploratory Data Analysis**
- Understand the data in the train, test, spray and weather datasets
- Perform data cleaning and extract essential features for further analysis
- Plot distributions locations of WNV across the years
- Merge required data for subsequent analysis 

**2) Feature Engineering and Selection**
- Engineer features (e.g. temporal, species, weatherlag etc.) which will be required for modelling
- Perform preliminary analysis to narrow down on the features to be used for modelling and tuning
- Save datasets for modelling and Kaggle test

**3) Modelling and Tuning**
- Analyse data using a variety of algorithms such as RandomForest, Support Vector Classification, XGBoost
- Tune model hyperparameters to improve model performace.

**4) Cost Benefit Analysis**
- Understand the implications of the model chosen in terms of financial and public health costs


## Results
**Key Observatiions from Exploratory Data Analysis**
1) The number of WNV present traps across years vary greatly. Within each year there appears to be a seasonal peak in WNV in August.
2) Culex Pipiens are much more likely to carry the West Nile Virus. As such, a high number of WNV present traps correlate with a high number of Culex Pipiens.
3) There are traps which are hotspots across years, and traps which are hotspots within years or a subset of years.

Since the EDA shows that there might be deep interaction effects affecting the presence of WNV at a location^, we expect models which account for interaction effects as part of their algorithm(e.g. Random Forest and XGBoost) to perform better than a model such as Logistic Regression which does not explicitly account for such interactions.

^(ie. Certain years(weather condidions) may have more Culex Pipiens in specific times of the year at specific locations which results in higher WNV present counts)

**Modelling and Tuning**

These are the models we have used to find the best balance between sensitivity and specificity, not having huge trade off for Accuracy, ROC_AUC and Precision. 

![image](https://user-images.githubusercontent.com/98629542/163920379-40d5f6ba-0b5f-4c61-987c-b4de8638d88e.png)

## Cost Benefit Analysis


## Conclusions and Recommendations



