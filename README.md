# ANA-515-Assignment-2
ANA 515 Assignment 2
---
title: "ANA 515 Assignment 2"
author: "Sree Veereshwar Kumar Vallem"
date: "6/17/2022"
output:
  html_document: default
  word_document: default
  pdf_document: default
---

  
  <center>
  
  <img src="https://i.imgur.com/k0MT16s.jpeg"/>
  
  </center>
  
  ```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Description

This dataset consists of NBA data. This dataset has 72044 observations for 27 variables. It was collected from <https://projects.fivethirtyeight.com/2022-nba-predictions/>. The dataset contains links to the data behind The Complete History Of The NBA and our NBA Predictions. nba_elo.csv contains game-by-game Elo ratings and forecasts back to 1946. The dataset is a Microsoft Excel Comma Separated Values File (.csv)

Using this Data, will obtain the summary stats of Overal Golden State Warriors Home and Away game totals

## Loading Packages

```{r load-packages, echo=TRUE, message=FALSE, warning=FALSE}
library(tidyverse)  
library(dplyr)  
library(knitr)  
library(bslib)  
library(readr)
library(stringr)
library(DT)
```

## Reading the data with read.csv function from the package readr

```{r read, echo=TRUE}
nba_r <- read.csv("C:\\Users\\Veere\\Downloads\\nba_elo.csv")
```
## Cleaning/Pre-Processing Data

```{r clean, echo=TRUE, message=FALSE, warning=FALSE, paged.print=TRUE, results='hide'}
#nba_r_df <- nba_r %>%

is.na(nba_r) #Finding the missing values with NA in dataset and assign True False
colSums(is.na(nba_r)) #Sums of NA values based on SUm of True NA
which(colSums(is.na(nba_r))>67093) #Filtering the Columns which has more than 67093 NA's
names(which(colSums(is.na(nba_r))>67093)) # Names of the columns where NA's more than 67093 rows 

  #rename(away = team1) #Renaming the column team1 to away
#rename(home = team2) #Renaming the column team2 to home
nba_2022 <-filter(nba_r, season =="2022") #Filtering NBA season 2022
head (nba_2022, n=10) #look up top 10 rows
nba_2022_homeaway <- rename (nba_2022, c(away = team1, home=team2, awayscore = score1, homescore = score2))

head (nba_2022_homeaway, n=10) #look up top 10 rows

nbarating <- filter(nba_2022_homeaway, total_rating > 95)
gswnbarating <- filter(nbarating, away=="GSW" | home=="GSW")

gswnbarating %>%
  select(c(date, season, away, home, awayscore, homescore, quality,importance, total_rating))

keeps <- c("date", "season", "away", "home", "awayscore", "homescore", "quality","importance", "total_rating")

gswnbarating = nbarating[keeps]
## Hidden the output for this code chunk as it produces a large amount of data
```


```{r output, echo=TRUE}
gswnbarating['GSWTotalScore'] =  gswnbarating['awayscore'] + gswnbarating ['homescore'] #This is overall total score of Golden State Warrior NBA games in 2022 season with greater than 95 NBA Rating
```


## Characteristics of Data

```{r, echo=TRUE}

observations <- nrow(gswnbarating)
variables <- ncol(gswnbarating)
```

This data frame has `r observations` rows and `r variables` columns. The names of the columns and a brief description of each are in the table below:
  
## Table
  
```{r, echo=TRUE}
kable(str(gswnbarating))
```

## Calculating Golden State Warriors Home and Away Totals and their Summary

```{r, echo=TRUE}
GSWHomeMin <- min(gswnbarating[,9])
GSWHomeMax <- max(gswnbarating[,9])
GSWHomeMean <- mean(gswnbarating[,9])
GSWAwayMin <- min(gswnbarating[,8])
GSWAwayMax <- max(gswnbarating[,8])
GSWAwayMean <- mean(gswnbarating[,8])
      
summary(gswnbarating) #Using summary function
```
##Missing Values

```{r, echo=TRUE}
    which(is.na(gswnbarating$awayscore)) #Missing Values of Away Score
    which(is.na(gswnbarating$homescore)) #Missing Values of Home Scores
```
## Saving summary stats

```{r, echo=TRUE, results='markup'}
NBASummary <- data.frame(GSWHomeMax, GSWHomeMin, GSWHomeMean, GSWAwayMax, GSWAwayMin, GSWAwayMean)
print(NBASummary)

```
