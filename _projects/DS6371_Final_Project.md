---
layout: page
title: House Prices Analysis
description: The project aims to predict the sale prices of homes in Ames, Iowa using existing data, and programs like R and SAS.
img: assets/img/DS_6371_Final_Project/Thumbnail/DS_6371_Final_Project_Thumbnail.jpeg
importance: 2
category: work
---


Please try out my interactive Rshinyapp to experience my model’s prediction rate : [RShiny](https://iachavez97.shinyapps.io/RealEstateApp/)

Here is the link to other files related to the project in my GitHub Repository: [GitHub](https://github.com/DamilolaOwolabi/DS-6371-Project)


# INTRODUCTION

The goal of this project is to predict the sale price of homes in Ames, Iowa using existing data. We strive
to utilize various statistical and data analytical techniques in order to derive the best predictive model for
the sale price


## DATA DESCRIPTIONS

We received this data from a study of residential homes in Ames, Iowa. The data set contains 383 rows of values with 79 different explanatory variable columns. You can find out more regarding the dataset and
how it was pulled in the kaggle for house prices - advanced regression techniques. For respect to the
analysis of question 1 the three main variables that I assessed were the Neighborhood, sales price, and GrLivArea variables. The variables used in the second analysis are SalePrice, GrLivArea, and the
OverallQual.




# Analysis Question 1


## Restatement of Problem

For our first analysis our objective was to get an estimate of sale prices of houses are related to sq
footage of living area in houses for three neighborhoods which are NAmes, Edwards, and BrkSide.
Additionally, we were requested to provide the estimates if they differ based on the neighborhood and
provide confidence intervals for each of the different neighborhoods. We will also be providing evidence
that our model fits the assumptions and how we observed outliers/influential observations.


## Build and Fit the Mode

In building my model for the analysis I used a multiple linear regression model with Sale price being the response variable and GrLivArea and neighborhood being the explanatory variables. Below we see a scatterplot of living area for the 3 neighborhood vs the sale price of each home. I created this scatterplot to get an idea for the distribution of the points and see if there are any outliers, and we can see that there are two outliers from the same neighborhood Edwards. After analysis and looking at residual plots we decided to remove these two outlier points that are in the edwards neighborhood because they appear to be incorrectly assigned and the fact that they are in the same neighborhood helps us come to that conclusion as well.

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/DS_6371_Final_Project/pictures/pic1.png" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/DS_6371_Final_Project/pictures/pic2.png" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
    Scatterplots before transformations vs. post log transformation and removal of outliers.
</div>


## Checking Assumptions

Residual Plots - The residual plots will be in the index, through the cooks d plots we were able to identify the two values that were outliers and identified them as data point 131 and 399. We removed these and they showed a much better fit for the model and a more normalized distribution of our residual plots.

#### Influential point analysis (Cook’s D and Leverage)

<div class="row justify-content-sm-center">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/DS_6371_Final_Project/pictures/pic3.png" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/DS_6371_Final_Project/pictures/pic4.png" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption">
    Original cooks D plot vs. New cooks D plot. We can see that the cooks D levels have gone down significantly.
</div>


## Make sure to address each assumption.

We can see that after our removing of outliers and transformations that the cooks d as well as our residuals look much better compared to the original model. So we are now able to move forward with this model and show how sale price is related to GrLivArea compared to each neighborhood.


## Comparing Competing Models

Adj R  - the adjusted R^2 I got for my final model is .5002
Internal CV Press - the internal CV press number I got was 1437833896.


## Parameters

#### Estimates

Here we see the summary of our model.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6371_Final_Project/pictures/pic5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Summary of the model
</div>


#### Interpretation

Above we can see the table for our multiple linear regression model. Since we ran a log-log transformation we can interpret how a percent change in the GrLivArea variable will affect our final sale price. For NeighborhoodEdwards a 1% in GrLivArea the sale price will increase by approximately .011%, for NeighborhoodNAmes a 1% increase in the GrLivArea will increase the final sale price by approximately .13%, and for NeighborhoodBrkSide a 1% increase in the GrLivArea will increase the sale price by .60%


#### Confidence Intervals

- The confidence interval for NeighborhoodEdwards is (-.077, .049)
- The confidence interval for NeighborhoodNAmes is (.072, .19)
- The confidence interval for NeighborhoodBrkSide is (.53, .66)


## Conclusion

We can see from our original graphs that the residuals and the plot had outliers so we assessed them and were able to get more normally distributed plots after assessing the outliers and performing our transformations. We can see from our graphs as well that there appears to be a positive relationship between the GrLivArea and the sales price of homes and we were able to quantify the percent increase of the homes sales price based on the GrLivArea. We can see as well that the NAmes neighborhood has the highest value homes with BrkSide being in second and Edwards being the least valuable of the three neighborhoods.


## R Shiny: Price v. Living Area Chart 

In our [Rshiny app]( https://iachavez97.shinyapps.io/RealEstateApp/) we are showing a scatterplot of the relation between sales price of homes and the living area. Additionally, it is able to be separated by each of the 3 different neighborhoods NAmes, BrkSide, and Edwards.




# Analysis Question 2


## Restatement of Problem

This analysis hopes to build the best predictive model needed to predict future sale prices of homes in Ames, Iowa. We plan on doing that by looking at different types of regression models (Simple Linear Regression, Multiple Linear Regression, and Custom Multiple Regression), choosing the best model for each regression type by analyzing the various model selections and making a final decision based on the adjusted r-squared, CVpress and Kaggle score.


## Model Selection


#### 1.   Simple Linear Regression

    ~~~ SAS
    
    data NewtrainData;
    set trainData;
    if _n_ = 1299 then delete;
    if _n_ = 524 then delete;
    run;
    
    proc print data = NewtrainData;
    run;
    
    proc reg data = NewtrainData;
    model SalePrice = GrLivArea;
    run;

    *Running the model selection with the train/test split;
    *running the forward selection;
    proc glmselect data = NewtrainData plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea /selection = Forward(stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    
    *running the Backward selection;
    proc glmselect data = NewtrainData plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea /selection = Backward(stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    
    *running the Stepwise selection;
    proc glmselect data = NewtrainData plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea /selection = Stepwise(stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    ~~~
    
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6371_Final_Project/pictures/pic6.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6371_Final_Project/pictures/pic7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6371_Final_Project/pictures/pic8.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Stepwise vs. Forward vs. Backward plots
</div>

**Decision:**  Given  that  the  adjusted R-squared for the forward model selection is higher (0.5412). We will go ahead with that instead.


#### 2.   Multiple Linear Regression

    ~~~ SAS
    *Multiple Linear Regression;
    proc corr; run;
    symbol c=blue v= dot;
    proc sgscatter data = trainData;
    matrix SalePrice GrLivArea FullBath;
    
    proc reg data = trainData;
    model SalePrice = GrLivArea FullBath;
    run;
    
    data NewtrainData2;
    set trainData;
    if _n_ = 1299 then delete;
    if _n_ = 524 then delete;
    run;
    
    proc print data = NewtrainData2;
    run;
    
    proc reg data = NewtrainData2;
    model SalePrice = GrLivArea FullBath;
    run;
    
    *running the forward selection;
    proc glmselect data = NewtrainData2 plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea FullBath /selection = Forward(select=CV choose=CV stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    
    *running the Backward selection;
    proc glmselect data = NewtrainData2 plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea FullBath /selection = Backward(stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    
    *running the Stepwise selection;
    proc glmselect data = NewtrainData2 plots = all;
    partition fraction(test= 0.2);
    model SalePrice = GrLivArea FullBath /selection = Stepwise(stop=CV) cvmethod=random(5) stats = adjrsq CVDETAILS;
    run;
    ~~~ 

