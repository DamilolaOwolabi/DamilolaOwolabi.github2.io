---
layout: page
title: Frito Lay Talent Attrition Analysis
description: The project aims to analyse the attrition rate and monthly income among employees in the United States.
img: assets/img/DS_6306_Project_2/ds_6306_PROJECT_2_PIC2.0.jpeg
importance: 1
category: work
---


Please try out my interactive Rshinyapp to experience my model’s prediction rate : [RShiny](https://oluwadamilolaowolabi.shinyapps.io/DS_6372_Project_2/)


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

    ---js
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

    ---js
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
    
    
    #How many variables in the dataset ? 36
    
    #Our response variable will be based on the attrition variable
    
    ---

From the Train talent data, there are 870 random employees (rows), all ordered by IDD, and 36 variables (columns).


#### LOOKING AT THE DATASET 

    ---js
    set.seed(1234)

    # Set levels for Talent_Train
    Talent_Train$Attrition <-factor(Talent_Train$Attrition, levels=c('Yes','No')) #factoring the response variable
    Talent_Train <- Talent_Train %>% mutate(Over18_binary = ifelse(Over18 == "Y", 1, 0)) # breaking the Over18 variable into 2, because it has only one level, and keeps throwing an error.
    
    Talent_Train <- subset(Talent_Train, select = -c(Over18)) #removing the over18 column
    
    # Set levels for Talent_Test_Salary
    Talent_Test_Salary$Attrition <-factor(Talent_Test_Salary$Attrition, levels=c('No','Yes')) #factoring the response variable
    Talent_Test_Salary <- Talent_Test_Salary %>% mutate(Over18_binary = ifelse(Over18 == "Y", 1, 0)) # breaking the Over18 variable into 2, because it has only one level.
    Talent_Test_Salary <- subset(Talent_Test_Salary , select = -c(Over18)) #removing the over18 column
    
    yes_data <- Talent_Train[Talent_Train$Attrition == 'Yes',] # 4250
    no_data <- Talent_Train[Talent_Train$Attrition == 'No',] # 31918
    ---
    
The rate of Yes Attrition to no Attrition is 140: 730. Which presents the issue of an unbalanced data. This might be costly to sensitivity metric. Thankfully, i come equipped with knowledge from my Data Science professor (Prof. Bivin Sadler) on how to resolve this.


## ADDRESSING MISSING VALUES
It is important to look at missing values earlier on, as it might be affect our models

    ---js
    NaSum <- sum(is.na(Talent_Train)) #check for missing values in the Talent Dataset
    
    # Print the total count of missing values
    print(paste("Total missing values:", NaSum))
    ---
    
<[1] "Total missing values: 0"/->

From the results above, there appears to be no missing values in the Dataset (thank God!!)




# SALARY


## Looking at the Summary Statistics 

    ---js
        
    # Using a for loop to get the summary statistics
    
    # Initialize an empty list 
    numerical_count <- 0 # There are 27
    categorical_count <- 0 # There are 9
    summary_stats_numerical <- list() # Storing summary statistics for the numerical variables
    summary_stats_categorical <- list() # Storing summary statistics for the categorical variables
    summary_stats2_categorical <- list() # Storing summary statistics for the categorical variables
    categorical_variables <- list() #Storing categorical variables
    numerical_variables <- list() #Storing numerical variables
    
    # Iterate over each column in the data frame
    for (Variable in names(Talent_Train)) {
      # Calculate summary statistics for numeric variables
      if (is.numeric(Talent_Train[[Variable]])) {
        summary_stats_numerical[[Variable]] <- summary(Talent_Train[[Variable]])
        numerical_variables[[Variable]] <- Variable
        numerical_count <- numerical_count + 1
      } else {
        # For non-numeric variables, calculate frequency table
        summary_stats_categorical[[Variable]] <- table(Talent_Train[[Variable]])
        summary_stats2_categorical[[Variable]] <- summary(Talent_Train[[Variable]])
        categorical_variables[[Variable]] <- Variable
        categorical_count <- categorical_count + 1
      }
    }
    
    cat("There are  ", numerical_count, " numerical variables \n")
    cat("The numerical variables are: ")
    #Printing the numerical variables
    for (variable in names(numerical_variables)){
      print(numerical_variables[[variable]])
    }
    cat("\n")
    
    cat("There are  ", categorical_count, " categorical variables \n")
    cat("The categorical variables are: ")
    #Printing the categorical variables
    for (variable in names(categorical_variables)){
      print(categorical_variables[[variable]])
    }
    cat("\n")
    
    # Print summary statistics for each numerical variable
    for (col_name in names(summary_stats_numerical)) {
      cat("Summary statistics for", col_name, ":\n")
      print(summary_stats_numerical[[col_name]])
      cat("\n")
    }
    
    # Print summary statistics for each categorical variable
    for (col_name in names(summary_stats_categorical)) {
      cat("Summary statistics for", col_name, ":\n")
      print(summary_stats_categorical[[col_name]])
      #print(summary_stats2_categorical[[col_name]])
      cat("\n")
    }
    ---
    
From our summary Statistics, there are 28 numerical variables and 8 categorical variables. All adding to 36 variables. Some variables that swtood out were the EmployeeCount, which have a constant 1 values, and the Job Involvement, which values ranges between 1 and 4, hence providing a really low variance,.    
    
    
## VISUALIZING THE NUMERICAL VARIABLES SUMMARY STATISTICS

Since the variables are a lot, and i plan on saving time, I plan using a for loop to iterate through the variables and plot their box plots to visualize their summary statistics

    ---js
        for (Variable in names(Talent_Train)){
      # Check if the column is numeric
      if (is.numeric(Talent_Train[[Variable]])) {
        # Create a box plot for numeric variables
       plot <-  ggplot(Talent_Train, aes_string(x = , y = Variable)) +
          geom_boxplot(color = "black", fill = "lightblue") +
          labs(title = paste("Box Plot of", Variable)) + 
          geom_text(aes(label = paste("Median:", median(Talent_Train[[Variable]]), "\n", 
                                      "Mean: ", mean(Talent_Train[[Variable]]), "\n", 
                                      "SD: ", sd(Talent_Train[[Variable]]),
                                      "1st Quarter: ", summary(Talent_Train[[Variable]])[2], "\n",
                                      "3rd Quarter: ", summary(Talent_Train[[Variable]])[4], "\n",
                                      "Min: ", summary(Talent_Train[[Variable]])[1], "\n",
                                      "Max: ", summary(Talent_Train[[Variable]])[5], "\n")),
                    x = 0.4, y = max(Talent_Train[[Variable]]), hjust = 1, vjust = 1, size = 3, color = "blue") 
       
       # Print the plot
        print(plot)
      }
    }
    ---
    
<div style="overflow-x: scroll; white-space: nowrap;">
    ![Image 1](assets/img/DS_6306_Project_2/pic_1.png)
    ![Image 2](assets/img/DS_6306_Project_2/pic_2.png)
    ![Image 3](assets/img/DS_6306_Project_2/pic_3.png)
    ![Image 4](assets/img/DS_6306_Project_2/pic_4.png)
    ![Image 5](assets/img/DS_6306_Project_2/pic_5.png)
    ![Image 6](assets/img/DS_6306_Project_2/pic_6.png)
    ![Image 7](assets/img/DS_6306_Project_2/pic_7.png)
    ![Image 8](assets/img/DS_6306_Project_2/pic_8.png)
    ![Image 9](assets/img/DS_6306_Project_2/pic_9.png)
    ![Image 10](assets/img/DS_6306_Project_2/pic_10.png)
    ![Image 11](assets/img/DS_6306_Project_2/pic_11.png)
    ![Image 12](assets/img/DS_6306_Project_2/pic_12.png)
    ![Image 13](assets/img/DS_6306_Project_2/pic_13.png)
    ![Image 14](assets/img/DS_6306_Project_2/pic_14.png)
    ![Image 15](assets/img/DS_6306_Project_2/pic_15.png)
    ![Image 16](assets/img/DS_6306_Project_2/pic_16.png)
    ![Image 17](assets/img/DS_6306_Project_2/pic_17.png)
    ![Image 18](assets/img/DS_6306_Project_2/pic_18.png)
    ![Image 19](assets/img/DS_6306_Project_2/pic_19.png)
    ![Image 20](assets/img/DS_6306_Project_2/pic_20.png)
    ![Image 21](assets/img/DS_6306_Project_2/pic_21.png)
    ![Image 22](assets/img/DS_6306_Project_2/pic_22.png)
    ![Image 23](assets/img/DS_6306_Project_2/pic_23.png)
    ![Image 24](assets/img/DS_6306_Project_2/pic_24.png)
    ![Image 25](assets/img/DS_6306_Project_2/pic_25.png)
    ![Image 26](assets/img/DS_6306_Project_2/pic_26.png)
    ![Image 27](assets/img/DS_6306_Project_2/pic_27.png)
    ![Image 28](assets/img/DS_6306_Project_2/pic_28.png)
    <!-- Add more images as needed -->
</div>

New change 4

<html>
  <div class="scroll-container">
      <div><img path="assets/img/DS_6306_Project_2/pic_1.png"></div>
  		    <div><img path="assets/img/DS_6306_Project_2/pic_2.png"></div>
  		    <div><img path="assets/img/DS_6306_Project_2/pic_3.png"></div>
  </div>
  
  <style>
    div.scroll-container {
      background-color: #333;
      overflow: auto;
      white-space: nowrap;
      padding: 10px;
    }
  </style>
  
  div.scroll-container img {
    padding: 10px;
  }
  
</html>

    

        


    
