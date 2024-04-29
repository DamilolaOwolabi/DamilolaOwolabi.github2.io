---
layout: page
title: Frito Lay Talent Attrition Analysis
description: The project aims to analyse the attrition rate and monthly income among employees in the United States.
img: assets/img/ds_6306_PROJECT_2_PIC2.0.jpeg
importance: 1
category: work
---


Please try out my Rshinyapp in order to experience my model’s prediction rate : [RShiny](https://oluwadamilolaowolabi.shinyapps.io/DS_6372_Project_2/)


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <iframe width="560" height="315" src="https://www.youtube.com/embed/hwyr3ZPVgIQ?si=OlWzfU2HNfsL_NWf" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    </div>
</div>
<div class="caption">
    Here is the Youtube Video of my presentation.
</div>




# INTRODUCTION

Good afternoon Mr. Steven Williams – CEO and Mrs. Christy Jacoby - CFO. I am here today to present my findings on my analysis on the factors affecting the rate of attrition among employees within a company. I am an employee of DDSAnalytivs, and i am here to present how my analysis can help you retain talented employees within Frito Lays.

Our data set was extracted from the company's AWS bucket. Where we interviewed 870 random employees, question related to their possibility of attrition.We got 36 answers from each employee. 34 of those factors (answers) were used for our dependent variables. hle 2 of them were used for our predictions: The monthly income for our Regression prediction, and the Attrition for our Classification prediction.

My aim is to provide valuable insight on what factors affects attrition rate and Monthly Income


## LOADING LIBRARIES

    ---
    library(ggplot2) #For data visualization
    library(dplyr)  # For data manipulation
    library(tidyverse)
    #install.packages('aws.s3')
    library(aws.s3) #to access the aws s3 package
    library(caret) # Load the caret package
    library(pROC) # for the ROC curve
    library(class) #calling the knn function
    library(e1071)  # for naiveBayes function
    ---
    
    
##  GETTING THE CSV FILE FROM AWS USING AWS.S3 PACKAGES

    ---
    Sys.setenv("AWS_ACCESS_KEY_ID" = "MY_ACCESS_ID",
               "AWS_SECRET_ACCESS_KEY" = MY_SECRET_ACCESS_KEY",
               "AWS_DEFAULT_REGION" = "MY_DEFAULT_REGION")
    
    
    # Using aws.s3
    aws.s3::bucketlist()
    aws.s3::get_bucket("msdsds6306")
    
    
    # read and write from ojbect
    
    #Read in the train data 
    Talent_Train = s3read_using(FUN = read.csv,
                        bucket = "msdsds6306",
                        object = "CaseStudy2-data.csv")
    
    #Reading in  the Test Data for Attrition
    Talent_Test_Attrition = s3read_using(FUN = read.csv,
                        bucket = "msdsds6306",
                        object = "CaseStudy2CompSet No Attrition.csv")
    
    #Reading in  the Test Data for monthly income
    Talent_Test_Salary = s3read_using(FUN = read.csv,
                        bucket = "msdsds6306",
                        object = "CaseStudy2CompSet No Salary.csv")
    
    #Reading in  an example of prediction Regeression
    Example = s3read_using(FUN = read.csv,
                        bucket = "msdsds6306",
                        object = "Case2PredictionsRegressEXAMPLE.csv")
    
    #Reading in  an example of prediction classification
    Example2 = s3read_using(FUN = read.csv,
                        bucket = "msdsds6306",
                        object = "Case2PredictionsClassifyEXAMPLE.csv")
    
    
    #Talent_Train; Talent_Test_Attrition; Talent_Test_Salary
    
    names(Talent_Train) # looking at the variables in the dataset
    
    #How many variables in the dataset ? 36
    
    #Our response variable will be based on the attrition variable
    
    head(Example) #Looking at our example regression
    
    head(Example2) #Looking at our example regression
    ---

    
    
