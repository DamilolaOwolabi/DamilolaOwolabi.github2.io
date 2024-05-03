---
layout: page
title: Budweiser EDA
description: The purpose of the project is to analyse the IBU and ABV % of different beers and breweries from different US state using R.
img: assets/img/DS_6306_Project1/Thunbnails/budweiser.jpeg
importance: 3
category: school
---


Please try out my interactive Rshinyapp to experience my model’s prediction rate : [RShiny](https://github.com/DamilolaOwolabi/DS-6306-PROJECT-1)

Here is the link to other files related to the project in my GitHub Repository: [GitHub](https://github.com/DamilolaOwolabi/DS-6371-Project)


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <iframe width="560" height="315" src="https://www.youtube.com/embed/5oXWlK6q3AU?si=THH2m7MM1Voio0PE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    </div>
</div>

<div class="caption">
    Here is the Youtube Video of my presentation.
</div>




# Introduction

Good afternoon Mr. Whitworth and Mr. Tennenbaum. We are here today to present the findings from the questions presented to our team of Data Scientist. We will answer the nine questions provided ahead of time with our analysis and visualuations, as well as highlighting any key observations that came up during our review. With that, let us get started as we’re excited to share with you the breakdown of our findings.


#### Libraries/Packages used

      --- R
      #install.packages("tidyverse")
      #install.packages("ggplot2")
      #install.packages("GGally")
      #install.packages("dplyr")
      #install.packages("maps")
      #install.packages("mapsproj")
      library(ggplot2)
      library(tidyverse)
      library(GGally)
      library(dplyr)
      library(class)
      library(caret)
      library(e1071)
      library(maps)
      library(mapproj)
      ---


#### Agenda 1a: Distribution by State

Loaded files and confirm, how many breweries are located in each state?

      --- R
    #summary(Breweries) #check the type
    
    mean_count <- mean(Breweries$n) #create a column mean_count for the reference line
        
    Breweries %>% group_by(State) %>% summarize(n = n()) %>% ggplot(aes(x = State, y = n, fill = State)) +
      geom_bar(stat = "identity") + theme(legend.position = "none") + geom_text(aes(label = n, y = n +1.5)) +
      ggtitle("Count of Breweries by State") + ylab("Count of Breweries") + xlab("State") + 
      theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
      geom_hline(yintercept = mean_count, linetype = "dotted", color = "red") # to show the mean.
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Plot of Count of Breweries by State
</div>

**Analysis: ** We provided a bar chart showing the count of breweries by state. The state with the highest count of breweries is Colorado with 47 breweries. There is a four way tie between DC, North Dakota, South Dakota, and West Virginia for the lowest with one brewery each. The mean count of breweries per state is eleven.


#### Agenda 1b: Distribution by State

Load files and confirm, how many breweries are located in each state?

      --- R
      lookup = data.frame(State = state.abb, State_Name = state.name) #makes a dataframe with State na me and state abb. 
      Breweries2 = inner_join(Breweries,lookup, by = "State")
      BreweriesMapData = Breweries2 %>% group_by(State) %>% count(State_Name) #head(BreweriesMapData)
      colnames(BreweriesMapData)[3] = "Breweries" #change 'n' to 'Breweries' BreweriesMapData$region <- tolower(BreweriesMapData$State_Name) #head(BreweriesMapData)
      BreweriesMapData2 = BreweriesMapData[-1] #drop the first column states <- map_data("state")
      map.df <- merge(states, BreweriesMapData2, by = "region", all.x = TRUE) head(map.df)
      ---
   
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Map of Breweries by State
</div>
      
**Analysis: ** In addition to the bar chart, a heat map showing the distribution of breweries was included. This was added as a “knock their socks off” addition as it enhanced the answer to their question visually.


#### Question 2 - Merge beer data with the breweries data.

Print the first 6 observations and the last six observations to check the merged file.

      ---
      merged_df = merge(Beers,Breweries, by.x = "Brewery_id", by.y = "Brew_ID") #joining when keys do not match
      head(merged_df,6) #to show the first 6 observations
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic19.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Merged dataset
</div>

**Analysis: ** Using merge() we brought in the Breweries csv file against the Beers csv file. The method used was when the two keys were not identical


#### Agenda 2: Missing Values

Address the missing values in each column

      ---
      #merged_df %>% is.na() #check for missing values , ABV and IBU
      #Cleaning the data
      merged_df <- merged_df %>% na.omit() 
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic20.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Proof of Missing Values
</div>

**Analysis: ** There were 62 missing values from column ABV, approximately three percent of the total. There were 1,005 missing values from column IBU. The data was review and we could not locate a relationship amongst the missing data and the qualifiers. Example, there did not appear to be an observable difference between data missing across the State data, or Brewery data. Overall, it appeared that the data was missing completely at random with no way of back tracking into an answer for the missing values.


#### Agenda 3: Mean ABV %

mean of ABV by State and bar chart

      ---
      #create a new df to add Mean ABV % 
    merged_df1 <- merged_df %>% group_by(State) %>% summarize(meanABV = mean(ABV, na.rm = TRUE)) #ad d column meanABV with the mean of ABV. 
    #confirm meanABV has been added 
    #merged_df1
    #plot to show the meanABV % by state, update labels, remove legend. 
    ggplot(merged_df1, aes(x = State, y = meanABV, fill = State)) + 
    theme(legend.position = "none") + #removed the legend
    geom_bar(stat = "identity") + geom_text(aes(label = round(meanABV, 2), y = meanABV + .01), siz e = 2) + #using round for cleaner looking labels
    ggtitle("ABV % mean by State") + xlab("State") + ylab("Mean ABV %") + ylim(0.0,0.1) + #zoom in on the y-axis for small %s
    theme(axis.text.x = element_text(angle = 90, hjust = 1)
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Plot of ABV% mean by state
</div>

**Analysis: ** New Mexico has the highest mean Alcohol by Volumne % (ABV %) and Utah has the lowest ABV %. Each state was calculated and a label was added to show each. To prevent overlapping, the size of the mean labels were reduced


#### Agenda 4: Mean IBU 

mean of IBU by State and bar chart

      ---
      #repeat with IBU
    #create a new dataframe with 
    merged_df2 <- merged_df %>% group_by(State) %>% summarize(meanIBU = mean(IBU, na.rm = TRUE)) #ad d column meanABV with the mean of IBU. 
    #confirm meanIBU has been added 
    #plot to show the meanIBU by state, update labels, remove legend.
    ggplot(merged_df2, aes(x = State, y = meanIBU, fill = State)) + 
    theme(legend.position = "none") + #removed the legend
    geom_bar(stat = "identity") + geom_text(aes(label = round(meanIBU, 1), y = meanIBU + 2), size = 2) + #using round for cleaner looking labels
    ggtitle("IBU mean by State") + xlab("State") + ylab("Mean IBU") + ylim(0,60) + 
    theme(axis.text.x = element_text(angle = 90, hjust = 1))
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Plot of IBU mean by state
</div>

**Analysis: ** West Virginia has the highest mean International Bitterness Units (IBU). Due to missing values, South Dakota had no data represented. The remaining states with data were calculated and a label was added to show each. To prevent overlapping, the size of the mean labels were reduced.


#### Agenda 5 - Finding the states with the most alcoholic beer and the most bitter beer.

Which state has the maximum alcoholic (ABV) beer

      ---
      merged_df <- merged_df %>%  filter(!is.na(ABV)) 
      merged_df %>% group_by(State) %>% ggplot(aes(x = State, y = ABV, fill = State)) + theme(legend.p osition = "none") + geom_boxplot()  + ggtitle("Stem and Box plot of the ABV Values for each stat e")
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Stem and Box Plot of the ABV values for each state
</div>

Which state has the most bitter (IBU) beer?

      ---
      merged_df %>% group_by(State) %>% ggplot(aes(x = State, y = IBU, fill = State)) + theme(legend.p osition = "none") + geom_boxplot()  + ggtitle("Stem and Box plot of the IBU Values for each stat e")
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic6.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Stem and Box Plot of the IBU values for each state
</div>

**Analysis: ** The state of Kentucky possesses the most alcoholic beverage with an ABV of 12.5%, while the state of Oregon, possesses the most biter beer with an IBU of 138 units


#### Agenda 6: Investigating the summary statistics of the Alcohol By Volume Variable.

Getting a  box plot for the summary

      ---
      merged_df %>% group_by(ABV) %>% ggplot(aes(y = ABV)) + geom_boxplot(color = 'orange') + ggtitle ("The summary statistics of the ABV variable") + theme(legend.position = "none")
      ---
      
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DS_6306_Project1/pic7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    The Summary of the ABV variable
</div>

** Visualizing the distribution based on the states using a box plot **

Getting the boxplot for the state with the highest ABV (Kentucky)


