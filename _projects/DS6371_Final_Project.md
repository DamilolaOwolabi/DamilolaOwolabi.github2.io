---
layout: page
title: House Prices Analysis
description: The project aims to predict the sale prices of homes in Ames, Iowa using existing data, and programs like R and SAS.
img: assets/img/DS_6371_Final_Project/Thumbnail/DS_6371_Final_Project_Thumbnail.jpeg
importance: 2
category: work
---


Please try out my interactive Rshinyapp to experience my model’s prediction rate : [RShiny](https://oluwadamilolaowolabi.shinyapps.io/DS_6372_Project_2/)

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


## Restatement of Problem

For our first analysis our objective was to get an estimate of sale prices of houses are related to sq
footage of living area in houses for three neighborhoods which are NAmes, Edwards, and BrkSide.
Additionally, we were requested to provide the estimates if they differ based on the neighborhood and
provide confidence intervals for each of the different neighborhoods. We will also be providing evidence
that our model fits the assumptions and how we observed outliers/influential observations.


## Build and Fit the Mode

In building my model for the analysis I used a multiple linear regression model with Sale price being the response variable and GrLivArea and neighborhood being the explanatory variables. Below we see a scatterplot of living area for the 3 neighborhood vs the sale price of each home. I created this scatterplot to get an idea for the distribution of the points and see if there are any outliers, and we can see that there are two outliers from the same neighborhood Edwards. After analysis and looking at residual plots we decided to remove these two outlier points that are in the edwards neighborhood because they appear to be incorrectly assigned and the fact that they are in the same neighborhood helps us come to that conclusion as well.

test 1
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
