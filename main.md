Bellabeat Wellness Device Data Analysis
================

<p align="center">
<img src="image/Bellabeat.png" alt="Bellabeat" width="700">
</p>

# Contents

1.  [Summary](#1.) 1.1. [Initializing Crypto Data Lists and Creating
    DataFrame](#1.1) 1.2. [Function: Scrape Date List from
    CoinMarketCap](#1.2)
2.  [Ask Phase](#2.)
3.  [Prepare Phase](#3.)
4.  [Process Phase](#4.)
5.  [Analyze and Share Phase](#5.)
6.  [Act Phase (Conclusion)](#6)

<a id="1."></a> \# 1. Summary

<a id="2."></a> \# 2. Ask Phase \# 2.1. Business Task

<a id="3."></a> \# 3. Prepare Phase \# 3.1. Dataset used \# 3.2.
Accessibility and privacy of data \# 3.3. Information about our dataset
\# 3.4. Data organization and verification \# 3.5. Data credibility and
integrity

<a id="4."></a> \# 4. Process Phase \# 4.1. Installing packages and
opening libraries \# 4.2. Importing datasets \# 4.3. Preview our
datasets \# 4.4. Cleaning and formatting \# 4.5. Merging datasets

<a id="5."></a> \# 5. Analyze and Share Phase \# 5.1. Type of users per
activity level \# 5.2. Steps and minutes asleep per weekday \# 5.3.
Hourly steps throughout the day \# 5.4. Correlations \# 5.5. Use of
smart device \# 5.5.1. Days used smart device \# 5.5.2. Time used smart
device per day

# 1. Load libraries

``` r
# install.packages("chron")
library(chron) # For working with times
library(tidyverse) # For data manipulation and visualization
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ lubridate::days()    masks chron::days()
    ## ✖ dplyr::filter()      masks stats::filter()
    ## ✖ lubridate::hours()   masks chron::hours()
    ## ✖ dplyr::lag()         masks stats::lag()
    ## ✖ lubridate::minutes() masks chron::minutes()
    ## ✖ lubridate::seconds() masks chron::seconds()
    ## ✖ lubridate::years()   masks chron::years()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(lubridate) # For working with dates
```

# 2. Read CSV files into data frames

``` r
dailyActivity <- read_csv("dataset/dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
sleepDay <- read_csv("dataset/sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
weightLogInfo <- read_csv("dataset/weightLogInfo_merged.csv")
```

    ## Rows: 67 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

# 3. Explore and clean the data

## 3.1. Explore the Data

``` r
# Explore data - Show the first few rows of each data frame
head(dailyActivity)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 4/12/2016         13162          8.5             8.5 
    ## 2 1503960366 4/13/2016         10735          6.97            6.97
    ## 3 1503960366 4/14/2016         10460          6.74            6.74
    ## 4 1503960366 4/15/2016          9762          6.28            6.28
    ## 5 1503960366 4/16/2016         12669          8.16            8.16
    ## 6 1503960366 4/17/2016          9705          6.48            6.48
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
head(sleepDay)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
    ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:0…                 1                327            346
    ## 2 1503960366 4/13/2016 12:0…                 2                384            407
    ## 3 1503960366 4/15/2016 12:0…                 1                412            442
    ## 4 1503960366 4/16/2016 12:0…                 2                340            367
    ## 5 1503960366 4/17/2016 12:0…                 1                700            712
    ## 6 1503960366 4/19/2016 12:0…                 1                304            320

``` r
head(weightLogInfo)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
    ## 2 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
    ## 3 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
    ## 4 2873212765 4/21/2016…     56.7         125.    NA  21.5 TRUE           1.46e12
    ## 5 2873212765 5/12/2016…     57.3         126.    NA  21.7 TRUE           1.46e12
    ## 6 4319703577 4/17/2016…     72.4         160.    25  27.5 TRUE           1.46e12

``` r
# Check for missing values
sum(is.na(dailyActivity))
```

    ## [1] 0

``` r
sum(is.na(sleepDay))
```

    ## [1] 0

``` r
sum(is.na(weightLogInfo))
```

    ## [1] 65

The “weightLogInfo” dataframe has 65 missing values.

## 3.2. Find out missing values

``` r
# Explore the weightLogInfo data frame
summary(weightLogInfo)
```

    ##        Id                Date              WeightKg       WeightPounds  
    ##  Min.   :1.504e+09   Length:67          Min.   : 52.60   Min.   :116.0  
    ##  1st Qu.:6.962e+09   Class :character   1st Qu.: 61.40   1st Qu.:135.4  
    ##  Median :6.962e+09   Mode  :character   Median : 62.50   Median :137.8  
    ##  Mean   :7.009e+09                      Mean   : 72.04   Mean   :158.8  
    ##  3rd Qu.:8.878e+09                      3rd Qu.: 85.05   3rd Qu.:187.5  
    ##  Max.   :8.878e+09                      Max.   :133.50   Max.   :294.3  
    ##                                                                         
    ##       Fat             BMI        IsManualReport      LogId          
    ##  Min.   :22.00   Min.   :21.45   Mode :logical   Min.   :1.460e+12  
    ##  1st Qu.:22.75   1st Qu.:23.96   FALSE:26        1st Qu.:1.461e+12  
    ##  Median :23.50   Median :24.39   TRUE :41        Median :1.462e+12  
    ##  Mean   :23.50   Mean   :25.19                   Mean   :1.462e+12  
    ##  3rd Qu.:24.25   3rd Qu.:25.56                   3rd Qu.:1.462e+12  
    ##  Max.   :25.00   Max.   :47.54                   Max.   :1.463e+12  
    ##  NA's   :65

``` r
# Check the structure of the data frame
str(weightLogInfo)
```

    ## spc_tbl_ [67 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id            : num [1:67] 1.50e+09 1.50e+09 1.93e+09 2.87e+09 2.87e+09 ...
    ##  $ Date          : chr [1:67] "5/2/2016 11:59:59 PM" "5/3/2016 11:59:59 PM" "4/13/2016 1:08:52 AM" "4/21/2016 11:59:59 PM" ...
    ##  $ WeightKg      : num [1:67] 52.6 52.6 133.5 56.7 57.3 ...
    ##  $ WeightPounds  : num [1:67] 116 116 294 125 126 ...
    ##  $ Fat           : num [1:67] 22 NA NA NA NA 25 NA NA NA NA ...
    ##  $ BMI           : num [1:67] 22.6 22.6 47.5 21.5 21.7 ...
    ##  $ IsManualReport: logi [1:67] TRUE TRUE FALSE TRUE TRUE TRUE ...
    ##  $ LogId         : num [1:67] 1.46e+12 1.46e+12 1.46e+12 1.46e+12 1.46e+12 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   Date = col_character(),
    ##   ..   WeightKg = col_double(),
    ##   ..   WeightPounds = col_double(),
    ##   ..   Fat = col_double(),
    ##   ..   BMI = col_double(),
    ##   ..   IsManualReport = col_logical(),
    ##   ..   LogId = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
# Count the number of missing values in each column
col_missing <- colSums(is.na(weightLogInfo))
col_missing
```

    ##             Id           Date       WeightKg   WeightPounds            Fat 
    ##              0              0              0              0             65 
    ##            BMI IsManualReport          LogId 
    ##              0              0              0

The “Fat” column of the “weightLogInfo” dataframe has 65 missing value.

## 3.3. Handle Missing Values

``` r
# Impute missing values in the "Fat" column with the mean
mean_fat <- mean(weightLogInfo$Fat, na.rm = TRUE)
weightLogInfo$Fat[is.na(weightLogInfo$Fat)] <- mean_fat
```

It replaces the missing values in the “Fat” column with the mean value
of the non-missing values in that column.

## 3.4. Data Transformations

### 3.4.1. Create the date and time columns to the datetime data type

``` r
dailyActivity$DateOnly <- as.Date(dailyActivity$ActivityDate, format = "%m/%d/%Y")

sleepDay$DateOnly <- as.POSIXct(sleepDay$SleepDay, format = "%m/%d/%Y %I:%M:%S %p")

weightLogInfo$Date <- as.POSIXct(weightLogInfo$Date, format = "%m/%d/%Y %I:%M:%S %p")
weightLogInfo$DateOnly <- as.Date(weightLogInfo$Date)
weightLogInfo$TimeOnly <- times(format(weightLogInfo$Date, format = "%H:%M:%S"))
```

# 4. Analyze the data

# 5. Address the business questions

# 6. Visualize the results

# 7. Draw conclusions and make recommendations

Add a new chunk by clicking the *Insert Chunk* button on the toolbar or
by pressing *Cmd+Option+I*.

When you save the notebook, an HTML file containing the code and output
will be saved alongside it (click the *Preview* button or press
*Cmd+Shift+K* to preview the HTML file).

The preview shows you a rendered HTML copy of the contents of the
editor. Consequently, unlike *Knit*, *Preview* does not run any R code
chunks. Instead, the output of the chunk when it was last run in the
editor is displayed.

# 6. Act Phase (Conclusion)

<a id="6"></a>
