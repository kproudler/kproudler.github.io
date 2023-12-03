Bellabeat Case Study
================

## About

Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that
manufactures health-focused smart products. Collecting data on activity,
sleep, stress, and reproductive health has allowed Bellabeat to empower
women with knowledge about their own health and habits. Sršen knows that
an analysis of Bellabeat’s available consumer data would reveal more
opportunities for growth. She has asked the marketing analytics team to
focus on a Bellabeat product and analyze smart device usage data in
order to gain insight into how people are already using their smart
devices. Then, using this information, she would like high-level
recommendations for how these trends can inform Bellabeat marketing
strategy.

## Ask

Sršen asks you to analyze smart device usage data in order to gain
insight into how consumer use non-Bellabeat smart devices.

1.  What are some trends in smart device usage?
2.  How could these trends apply to Bellabeat customers?
3.  How could these trends help influence Bellabeat marketing strategy?

To provide an answer to these questions, I will be taking a look at
[FitBit Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit).
This is a Kaggle data set that contains personal fitness tracker data,
including physical activity, heart rate, weight, and sleep. Our analysis
will be geared towards one, or a selection of the following Bellabeat
products:

\-**Bellabeat app**: Provides users with health data related to their
activity, sleep, stress, menstrual cycle, and mindfulness habits. This
data can help users better understand their habits and make healthy
decisions.

\-**Leaf**: Wellness tracker that can be worn as a bracelet, necklace,
or clip. Tracks activity, sleep, and stress.

\-**Time**: A smart watch that tracks user activity, sleep, and stress.

\-**Spring**: Water bottle that tracks daily water intake.

\-**Bellabeat Membership**: Subscription-based membership program for
users that gives guidance on nutrition, activity, sleep, health and
beauty, and mindfulness.

## Data Source

The data provided has been licensed by CC0: Public Domain, dataset made
available through [Mobius](https://www.kaggle.com/arashnic). The dataset
contains 18 csv files (15 long, 3 wide). This dataset has been generated
by respondents to a distributed survey between 03/12/2016 - 05/12/2016.
Thirty users submitted their personal tracker data including physical
activity, heart rate, and sleep monitoring.

### Issues

-User gender is not revealed in the data. As Bellabeat is a company
focusing on creating health and wellness products for women, this may be
an issue. Other personal data of the users that aren’t included are
their age, ethnicity, or location. This may be an issue when it comes to
specificity of the analysis for Bellabeat’s target market.

-The sample size is of 30 users, over a short period of time. This
shortcoming will make it difficult to ascertain long-term insights in to
the users and their behavior.

## Data Cleaning

All data sets from [FitBit Fitness Tracker
Data](https://www.kaggle.com/arashnic/fitbit) have been downloaded and
will be analyzed.

In preparation for the analysis, the following packages will be loaded.

``` r
library(ggplot2)
library(tidyverse)
library(dplyr)
library(tidyr)
library(lubridate)
library(data.table)
library(janitor)
library(skimr)
```

## Including Plots
