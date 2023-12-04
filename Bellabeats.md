Bellabeat Case Study - Work in Progress
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

Loading all CSV files with health data into a list.

``` r
file_list <- list.files(path = "Fitabase Data 4.12.16-5.12.16/", full.names = TRUE)
datasets <- lapply(file_list, fread)
names(datasets) <- gsub('.{11}$', '', basename(file_list))
```

Each file had been appended with “\_merged.csv”, which has been removed
to ease referencing.

In order to begin analyzing the data, I need to be able to understand
what I’m working with. I will be creating a summary of the data files
showing the number of columns and rows, how many N/A fields, distinct
IDs, and duplicates are found in each.

``` r
ds_summary <- data.frame(
  sapply(datasets, nrow),
  sapply(datasets, ncol),
  sapply(datasets, FUN = function(x) {sum(is.na(x))}),
  sapply(datasets, FUN = function(x) {n_distinct(x[[1]])}),
  sapply(datasets, FUN = function(x) {sum(duplicated(x))})
)
names(ds_summary) <- c("# Rows", "# Columns", "# NA", "# Distinct ID", "# Duplicated")

ds_summary
```

    ##                          # Rows # Columns # NA # Distinct ID # Duplicated
    ## dailyActivity               940        15    0            33            0
    ## dailyCalories               940         3    0            33            0
    ## dailyIntensities            940        10    0            33            0
    ## dailySteps                  940         3    0            33            0
    ## heartrate_seconds       2483658         3    0            14            0
    ## hourlyCalories            22099         3    0            33            0
    ## hourlyIntensities         22099         4    0            33            0
    ## hourlySteps               22099         3    0            33            0
    ## minuteCaloriesNarrow    1325580         3    0            33            0
    ## minuteCaloriesWide        21645        62    0            33            0
    ## minuteIntensitiesNarrow 1325580         3    0            33            0
    ## minuteIntensitiesWide     21645        62    0            33            0
    ## minuteMETsNarrow        1325580         3    0            33            0
    ## minuteSleep              188521         4    0            24          543
    ## minuteStepsNarrow       1325580         3    0            33            0
    ## minuteStepsWide           21645        62    0            33            0
    ## sleepDay                    413         5    0            24            3
    ## weightLogInfo                67         8   65             8            0

-The summary indicates that most data sets have 33 Distinct IDs, other
than *minuteSleep*, *heartrate_seconds*, *sleepDay*, and
*weightLogInfo*.

-There are also 2 data sets that have duplicated data: *minuteSleep*,
and *sleepDay*.

-weightLogInfo is showing 65 NAs in the dataset.

``` r
sapply(datasets$weightLogInfo, FUN = function(x) {sum(is.na(x))})
```

    ##             Id           Date       WeightKg   WeightPounds            Fat 
    ##              0              0              0              0             65 
    ##            BMI IsManualReport          LogId 
    ##              0              0              0

-Upon further inspection, the NAs were all located within the column
*Fat*. As there are a total of 67 rows in this data set, this column
will not be useful with that much missing data.

#### Next Steps

-Remove duplicates from *minuteSleep* and *sleepDay*

-Drop the *Fat* column from *weightLogInfo*

``` r
datasets$weightLogInfo <- select(datasets$weightLogInfo, -Fat)
datasets$sleepDay <- unique(datasets$sleepDay)
datasets$minuteSleep <- unique(datasets$minuteSleep)

ds_summary <- data.frame(
  sapply(datasets, nrow),
  sapply(datasets, ncol),
  sapply(datasets, FUN = function(x) {sum(is.na(x))}),
  sapply(datasets, FUN = function(x) {n_distinct(x[[1]])}),
  sapply(datasets, FUN = function(x) {sum(duplicated(x))})
)
names(ds_summary) <- c("# Rows", "# Columns", "# NA", "# Distinct ID", "# Duplicated")
ds_summary
```

    ##                          # Rows # Columns # NA # Distinct ID # Duplicated
    ## dailyActivity               940        15    0            33            0
    ## dailyCalories               940         3    0            33            0
    ## dailyIntensities            940        10    0            33            0
    ## dailySteps                  940         3    0            33            0
    ## heartrate_seconds       2483658         3    0            14            0
    ## hourlyCalories            22099         3    0            33            0
    ## hourlyIntensities         22099         4    0            33            0
    ## hourlySteps               22099         3    0            33            0
    ## minuteCaloriesNarrow    1325580         3    0            33            0
    ## minuteCaloriesWide        21645        62    0            33            0
    ## minuteIntensitiesNarrow 1325580         3    0            33            0
    ## minuteIntensitiesWide     21645        62    0            33            0
    ## minuteMETsNarrow        1325580         3    0            33            0
    ## minuteSleep              187978         4    0            24            0
    ## minuteStepsNarrow       1325580         3    0            33            0
    ## minuteStepsWide           21645        62    0            33            0
    ## sleepDay                    410         5    0            24            0
    ## weightLogInfo                67         7    0             8            0

## Analyzing the Data

``` r
ggplot(datasets$dailyActivity, mapping = aes(x = Calories, y = TotalSteps, color = Calories)) +
  geom_point() +
  geom_smooth(method = "loess") +
  labs(title = "Total Steps vs Calories Burned", x = "Calories Burned") +
  scale_color_gradient(low = "#3182bd", high = "#fc8d59")
```

![](Bellabeat-Case-Study-Github_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
