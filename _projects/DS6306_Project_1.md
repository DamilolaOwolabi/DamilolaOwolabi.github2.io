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
    Original plot of the linear model before transformations or removing of outliers
</div>


We provided a bar chart showing the count of breweries by state. The state with the highest count of breweries is Colorado with 47 breweries. There is a four way tie between DC, North Dakota, South Dakota, and West Virginia for the lowest with one brewery each. The mean count of breweries per state is eleven.



_Project1