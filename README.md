# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 4: Identifying and Controlling West Nile Virus Hotspots

## Background

The West Nile virus (WNV) is the leading cause of mosquito-borne disease in the continental United States. The virus is most commonly spread to people by the bite of an infected mosquito, although it can also spread through organ transplant, blood transfusions and breast milk ([CDC](https://www.cdc.gov/westnile/index.html)).

Most people infected with the WNV do not feel sick. Of those infected, 1 in 5 develop mild symptoms while 1 in 150 develop a serious, sometimes fatal, illness. Nonetheless, it is important to note that there is currently no human vaccine available. As such, the only way to reduce infection in people is by  reducing exposure to the virus ([WHO, 2017](https://www.who.int/news-room/fact-sheets/detail/west-nile-virus)).

## Problem Statement
As an analyst in the Disease And Treatment Agency in the City of Chicago, you have been tasked to predict when and where different species of mosquitos will test positive for WNV in the City of Chicago. Subsequently, a cost-benefit analysis will be conducted. This should include annual cost projections for various levels of pesticide coverage (cost) and the effect of these various levels of pesticide coverage (benefit).

This will help the City of Chicago and the Chicago Department of Public Health (CDPH) more efficiently and effectively allocate resources towards preventing transmission of this potentially deadly virus.

**Goal:**
In favour of reducing human fatalities and the possiibility of an uncontained outbreak, the team will focus on minimising false negatives over false positives. This would mean placing greater emphasise on `sensitivity` rather than `specificity` without sacrificing too much on `Accuracy`. We will also observe the `Precision` of the model to understand the quality of a positive prediction made by the model (ie. the increase in false positives for additional true positives).

--

## Method
**1) Data Cleaning and Exploratory Data Analysis**
- Understand the data in the train, test, spray and weather datasets
- Weather datasets were split by two weather stations, hence we calculated the euclidian distance between weather stations and traps and attributed the weather conditions to it as shown below. 

![Nearest Traps around weather stations](https://user-images.githubusercontent.com/98629542/164117440-f4c92dfa-b0b7-4ecf-a9e0-dbbbdbaa4779.png)

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

--

## Results
**Key Observatiions from Exploratory Data Analysis**
1) The number of WNV present traps across years vary greatly. Within each year there appears to be a seasonal peak in WNV in August.
2) Referring to the trend for 2007 and 2013, Culex Pipiens are much more likely to carry the West Nile Virus. As such, a high number of WNV present traps correlate with a high number of Culex Pipiens.

`In 2007 and 2013 we can see the similar trend.`

![wnvpresent_trend_2007](https://user-images.githubusercontent.com/98629542/164122669-e1f7d0f6-cbd7-47a1-bad5-f0ebcab3add7.png)
![wnvpresent_trend_2013](https://user-images.githubusercontent.com/98629542/164122693-e148fc2e-054f-45c7-bf70-a8c2667d1295.png)


`On the other hand, in 2009 and 2011 we see different but similar trend as well`

![wnvpresent_trend_2009](https://user-images.githubusercontent.com/98629542/164122685-aac0b469-651a-4db6-890e-5c0afdb2cbf7.png)
![wnvpresent_trend_2011](https://user-images.githubusercontent.com/98629542/164122689-cfbeeeeb-2b1a-46b2-affa-610fa0b599a4.png)

3) There are traps which are hotspots across years, and traps which are hotspots within years or a subset of years.

![image](https://user-images.githubusercontent.com/98629542/164116803-190eba85-9356-4f10-8c73-653916153052.png)


--

Since the EDA shows that there might be deep interaction effects affecting the presence of WNV at a location^, we expect models which account for interaction effects as part of their algorithm(e.g. Random Forest and XGBoost) to perform better than a model such as Logistic Regression which does not explicitly account for such interactions.

^(ie. Certain years(weather condidions) may have more Culex Pipiens in specific times of the year at specific locations which results in higher WNV present counts)


--
**Modelling and Tuning**

These are the models we have used to find the best balance between sensitivity and specificity, not having huge trade off for Accuracy, ROC_AUC and Precision. `XGBoost` was chosen as the final model, **achieving the best sensivity score, while maintaining superior scores for the rest of the matrix**.

![image](https://user-images.githubusercontent.com/98629542/164115280-4c040185-7d3d-4e23-8903-7969ce490d40.png)

The barplot below shows the feature importance from XGBoost, which calculates the 'gain', the relative contribution of the corresponding feature to the model calculated by taking each feature's contribution for each tree in the model. The higher value of this metric, when compared to another feature implies it is more important for generating a prediction. 

An interesting feature that was highlighted was the week 29 and 30, which was highlighted as the highest amongst the rest of the weeks. Based on our initial EDA, we can see that these two weeks were seeing a start of wnv influx across all the years as shown in "1_Data_Cleanup_Consolidated". This can be a strong signal for subsequent follow up actions needed which will be explained below.  

![XGboost_feature_importance_2](https://user-images.githubusercontent.com/98629542/164115523-5d2aa5ac-a3b0-404d-9111-adbcbceeccc1.png)



**Error Analysis** 

Focusing on Sensitivity, we will be looking at the False negatives. 
- Plotted the proportion of False negatives (FN/Total WNV) against each week 
- Found that there is higher proportion of FN at the start and end of the WNV outbreak across the weeks in all years 

![image](https://user-images.githubusercontent.com/98629542/164117155-8128f4b2-4869-459e-9a02-4939f45cda63.png)


## Cost Benefit Analysis

With the provided spray data, the average area sprayed is approximately 2864 acres. 
Using 67¢ / acre. [Source](https://www.centralmosquitocontrol.com/-/media/files/centralmosquitocontrol-na/us/resources-lit%20files/zenivex%20cost%20comparison%20fact%20sheet.pdf), the cost of spray is calculated to be $13,002 per spraying session.

From [NCBI](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/) data, out of 163 cases, 117(71.8\%) was diagonsed as the less severe West Nile Fever (WNF), while 46(28.2\%) was diagonsed as the more severe West Nile Neuroinvasive Disease (WNND). The average medical cost of WNND and WNF is estimated to be approximately \\$46,000 and \\$167 respectively.

<img src="../Pictures/XGBoost_Confusion_Matrix.png" style='float:left'>
With the assumption that the confusion matrix is the predicted data for a given year, and using the cost of spraying and the associated medical costs above, we calulated the estimated cost of 3 options.

|Options|1: Spray All WNV|2: Spray with Average Rate|3: No Spraying|
|---|---|---|---|
|Spraying Cost|\\$9,894,560.05|\\$403,063.55|\\$0|
|Medical Cost|\\$25,120.13|\\$162,705.95|\\$168,452.66|
|Total expected Cost|\\$9,919,680.18|\\$565,768.5|\\$168,452.66|

Although the expected cost of not spraying is the lowest amongst the three options. This is only looking at the monetary costs of the west nile virus infection. There are other intangible costs and benefits which we are unable to calculate, such as fatalities, discomfort of infected patients, productivity losses for infected patients.

Hence, it is not beneficial to choose to not spray.

Our recommendation currently would be to choose option 2 to spray at the average rate as we want to reduce fatalities and potential hotspot.


## Conclusions and Recommendations

Assuming the mixed species has both restuans and pipiens,  the model is expected to perform better if distinguished well. As such, we recommend taking extra time to ensure that the species are properly identified. 

While the inclusion of trap locations are sufficient leaving out the rare wnv cases, we suggest for future models to model location clusters as hotspots rather than specific traps to better capture high risk areas. 

The model currently is effective at picking out WNV presence at the peak of the wave rather than the start and of the wave. We recommend to build future models to be more sensitive to the onset of the WNV wave as that would be the best time to tackle the issue 


