GOOGLE DATA ANALYTICS PROFESSIONAL CERTIFICATE CAPSTONE PROJECT
================

# INTRODUCTION

The case study below is a part of Google Data Analytics Professional
Certificate program. It covers all six phases of data analysis process:
ask, prepare, process, analyze, share and act. The data used in this
project were provided by course administrator – Google and contains data
and information about fictional bike-share company – Cyclistic. In this
scenario, I am a junior data analyst in Cyclistic marketing analytics
team and were assigned to a new project which should provide insights
that will drive a Cyclistics new marketing strategy.

PRZEROBIONE PRZEZ GPT:

The following case study is a component of the Google Data Analytics
Professional Certificate program. It encompasses all six phases of the
data analysis process: *ask, prepare, process, analyze, share, and act.
The data utilized for this project was furnished by the course
administrator, Google, and consists of data and information pertaining
to the fictional bike-share company, Cyclistic. In this context, I
assume the role of a Junior Data Analyst within Cyclistic’s Marketing
Analytics team, having been tasked with a new project aimed at
delivering insights to inform Cyclistic’s forthcoming marketing
strategy.*

## 1. ASK PHASE

The Cyclistic’s forthcoming marketing strategy will be guided by the
questions below:

1.  How do annual members and casual riders use Cyclistic bikes
    differently?

2.  Why would casual riders buy Cyclistic annual memberships?

3.  How can Cyclistic use digital media to influence casual riders to
    become members?

The director of marketing, Lily Moreno, has assigned me a first question
to answear.

Assigned part of the analytics project will be guided by this business
task:

<u>Pointing habit patterns and behavior differences in casual and annual
Cyclistic user groups.</u>

## 2. PREPARE PHASE

Used data comes internal from Cyclistic system. It is organized in csv
files – each file contains quantitative set of data for each month and
it covers whole year 2022. Data does not contain any personal
information about the users and can not be connected with any specific
one. One row contain data about one ride taken by user.

Since it is every ride data and it is collected automatically, it is
free of observer, sampling and confirmation bias. It also fulfills the
conditions of good data (it is reliable, original, comprehensive,
current and cited). Data’s licensing, privacy, security and
accessibility are specified in Data License Agreement available under
<https://divvybikes.com/data-license-agreement> site.

Problems with provided data that could create problems during the
analysis:

- *The amount of rides make it impossible to analyze a full year view in
  one spreadsheet.*

- *The start and end station names are not always provided.*

- *The start and end station names are not consistent. Further analysis
  showed that there is difference between number of station id’s and
  station names (same stations – by id - are sometimes named
  differently).*

- *The latitude and longitude of stations does not always have same
  accuracy (differences in decimal places) but is 100% complete.
  However, according to provided information, coordinates are not used
  in this analysis project .*

- *Because data is anonymized, it is impossible to link ride with user
  and analyze his ride history.*

## 3. PROCESS PHASE

For initial cleaning and manipulating purposes Excel spreadsheet and
Excel’s Power Query tool were used. Since the amount of data is too big
to be analyzed in one spreadsheet (5 667 711 rows), each file have been
checked, cleaned and transformed separately.

Cleaning details:

TABELA Z WORDA

Rows deleted during cleaning:

![](ROWS%20CLEANED.png)

Cleaning process reduced number of rows by 0,62%; total number of
deleted rows = 35268.

Data manipulation increased number of columns from 13 to 19. New data
columns:

- started_at_hour - hour when ride started,

- started_at_hour_as_num - hour when hour started presented as number
  data type,

- start_id_len - length of hashed ride_id,

- ride_len - length of ride,

- start_day_of_week - day of week when ride started,

- started_at_part_of_day - part of day when ride started.

After cleaning and manipulating, each file was saved as csv file (for
analysis in R Studio, Big Query SQL and Tableau visualizations) and xlsx
file. In each months xlxs file new pivot table sheet with cleaned and
manipulated data were created.

## 4. ANALYZE PHASE

Like mentioned above, the number of rows is too big to store and analyze
data in one spreadsheet, so data have been imported into R Studio.

### DATA IMPORT AND BIND

``` r
jan_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/012022_tripdata.csv", sep = ';')
feb_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/022022_tripdata.csv", sep = ';')
mar_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/032022_tripdata.csv", sep = ';')
apr_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/042022_tripdata.csv", sep = ';')
may_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/052022_tripdata.csv", sep = ';')
jun_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/062022_tripdata.csv", sep = ';')
jul_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/072022_tripdata.csv", sep = ';')
aug_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/082022_tripdata.csv", sep = ';')
sep_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/092022_tripdata.csv", sep = ';')
oct_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/102022_tripdata.csv", sep = ';')
nov_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/112022_tripdata.csv", sep = ';')
dec_2022_data <- read.csv("D:/GDAPC Case study/01_Bike_share/cleaned_in_excel/csv/122022_tripdata.csv", sep = ';')
```

``` r
all_2022_data <- rbind(jan_2022_data, feb_2022_data, mar_2022_data, apr_2022_data,
                       may_2022_data, jun_2022_data, jul_2022_data, aug_2022_data,
                       sep_2022_data, oct_2022_data, nov_2022_data, dec_2022_data)
```

## INITIAL DATA CHECK

``` r
library(dplyr)
```

    ## 
    ## Dołączanie pakietu: 'dplyr'

    ## Następujące obiekty zostały zakryte z 'package:stats':
    ## 
    ##     filter, lag

    ## Następujące obiekty zostały zakryte z 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(skimr)

glimpse(all_2022_data)
```

    ## Rows: 5,632,443
    ## Columns: 19
    ## $ ride_id                <chr> "94696E127B0B5C2E", "2F30091C42C1A659", "FFF47E…
    ## $ rideable_type          <chr> "classic_bike", "classic_bike", "classic_bike",…
    ## $ started_at             <chr> "01.01.2022 09:22", "10.01.2022 07:15", "24.01.…
    ## $ ended_at               <chr> "02.01.2022 10:22", "11.01.2022 08:15", "25.01.…
    ## $ start_station_name     <chr> "Millennium Park", "Sheffield Ave & Waveland Av…
    ## $ start_station_id       <chr> "13008", "TA1307000126", "KA1503000071", "WL-00…
    ## $ end_station_name       <chr> "", "", "Lake Park Ave & 56th St", "Michigan Av…
    ## $ end_station_id         <chr> "", "", "TA1309000063", "TA1307000124", "", "",…
    ## $ start_lat              <dbl> 41.88103, 41.94940, 41.79148, 41.86712, 41.9394…
    ## $ start_lng              <dbl> -87.62408, -87.65453, -87.59986, -87.64109, -87…
    ## $ end_lat                <dbl> 41.87000, 41.94000, 41.79324, 41.86406, 41.9400…
    ## $ end_lng                <dbl> -87.63000, -87.66000, -87.58778, -87.62373, -87…
    ## $ customer_type          <chr> "member", "member", "member", "casual", "member…
    ## $ started_at_hour        <chr> "09:22:49", "07:15:21", "12:25:04", "11:34:32",…
    ## $ started_at_hour_as_num <dbl> 0.3908449, 0.3023264, 0.5174074, 0.4823148, 0.4…
    ## $ ride_id_len            <int> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16,…
    ## $ ride_len               <chr> "24:59:55", "24:59:52", "23:00:00", "22:59:37",…
    ## $ start_day_of_week      <int> 6, 1, 1, 1, 7, 7, 5, 1, 4, 4, 3, 1, 7, 7, 4, 2,…
    ## $ started_at_part_of_day <chr> "Morning", "Morning", "Afternoon", "Morning", "…

``` r
skim_without_charts(all_2022_data)
```

|                                                  |               |
|:-------------------------------------------------|:--------------|
| Name                                             | all_2022_data |
| Number of rows                                   | 5632443       |
| Number of columns                                | 19            |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |               |
| Column type frequency:                           |               |
| character                                        | 12            |
| numeric                                          | 7             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |               |
| Group variables                                  | None          |

Data summary

**Variable type: character**

| skim_variable          | n_missing | complete_rate | min | max |  empty | n_unique | whitespace |
|:-----------------------|----------:|--------------:|----:|----:|-------:|---------:|-----------:|
| ride_id                |         0 |          1.00 |  16 |  16 |      0 |  5632443 |          0 |
| rideable_type          |         0 |          1.00 |  11 |  13 |      0 |        3 |          0 |
| started_at             |         0 |          1.00 |  16 |  16 |      0 |   456257 |          0 |
| ended_at               |         0 |          1.00 |  16 |  16 |      0 |   456579 |          0 |
| start_station_name     |         0 |          1.00 |   0 |  64 | 826139 |     1675 |          0 |
| start_station_id       |         0 |          1.00 |   0 |  44 | 826139 |     1314 |          0 |
| end_station_name       |         0 |          1.00 |   0 |  64 | 876410 |     1691 |          0 |
| end_station_id         |    895121 |          0.84 |   0 |  44 | 668824 |     1318 |          0 |
| customer_type          |         0 |          1.00 |   6 |   6 |      0 |        2 |          0 |
| started_at_hour        |         0 |          1.00 |   8 |   8 |      0 |    86341 |          0 |
| ride_len               |         0 |          1.00 |   7 |  10 |      0 |    24423 |          0 |
| started_at_part_of_day |         0 |          1.00 |   5 |   9 |      0 |        4 |          0 |

**Variable type: numeric**

| skim_variable          | n_missing | complete_rate |   mean |   sd |     p0 |    p25 |    p50 |    p75 |   p100 |
|:-----------------------|----------:|--------------:|-------:|-----:|-------:|-------:|-------:|-------:|-------:|
| start_lat              |         0 |             1 |  41.90 | 0.05 |  41.64 |  41.88 |  41.90 |  41.93 |  45.64 |
| start_lng              |         0 |             1 | -87.65 | 0.03 | -87.84 | -87.66 | -87.64 | -87.63 | -73.80 |
| end_lat                |         0 |             1 |  41.90 | 0.05 |  41.55 |  41.88 |  41.90 |  41.93 |  42.37 |
| end_lng                |         0 |             1 | -87.65 | 0.03 | -88.14 | -87.66 | -87.64 | -87.63 | -87.30 |
| started_at_hour_as_num |         0 |             1 |   0.61 | 0.21 |   0.00 |   0.48 |   0.65 |   0.76 |   1.00 |
| ride_id_len            |         0 |             1 |  16.00 | 0.00 |  16.00 |  16.00 |  16.00 |  16.00 |  16.00 |
| start_day_of_week      |         0 |             1 |   4.06 | 1.98 |   1.00 |   2.00 |   4.00 |   6.00 |   7.00 |

## DATA TYPES CHANGE

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.4
    ## ✔ ggplot2   3.4.4     ✔ stringr   1.5.0
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2     ✔ tidyr     1.3.0
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(lubridate)

all_2022_data_test <- all_2022_data %>% 
  mutate(
    started_at = as_datetime(started_at, format = '%d.%m.%Y %H:%M'),
    ended_at = as_datetime(ended_at, format = '%d.%m.%Y %H:%M'),
    ride_len = difftime(ended_at, started_at, units = 'sec'),
  )
```

## ADDITIONAL COLUMNS FOR CALCS AND VIZ

``` r
all_2022_data_test <- all_2022_data_test %>% 
  mutate(month = substr(started_at, 6, 7)
        )
```

## SUMMARY OF DATA BEFORE CALCS

``` r
all_2022_data_summary <- all_2022_data_test %>%
  group_by(customer_type, month, rideable_type) %>% 
  summarise(
    num_of_rides = n(),
    mean_ride_len = mean(ride_len),
    mode_of_day_of_week = median(start_day_of_week),
    mode_of_part_of_day_1 = names(sort(table(started_at_part_of_day), decreasing = TRUE)[1]),
    )
```

    ## `summarise()` has grouped output by 'customer_type', 'month'. You can override
    ## using the `.groups` argument.

``` r
View(all_2022_data_summary)
```

## SUMMARY ABOVE SHOWS THAT ALL SUBGROUPS RIDES mode_of_part_of_day =‘Afternoon’ \#SO I DECIDED TO ADD A NEW COLUMN WITH FULL HOUR WHEN RIDE

STARTED:

``` r
all_2022_data_test <- all_2022_data_test %>% 
  mutate(started_at_full_hour = substr(started_at_hour, 1, 2))
```

## FINAL DF SUMMARY OF DATA BEFORE CALCS

``` r
all_2022_data_summary <- all_2022_data_test %>%
  group_by(customer_type, month, rideable_type, started_at_full_hour) %>% 
  summarise(
    num_of_rides = n(),
    mean_ride_len = mean(ride_len),
    mode_of_day_of_week = median(start_day_of_week)
    )
```

    ## `summarise()` has grouped output by 'customer_type', 'month', 'rideable_type'.
    ## You can override using the `.groups` argument.

## BASIC CALCULATIONS \#TOTAL RIDES BY CUSTOMER TYPE

``` r
all_2022_data_test_rides <- all_2022_data_test %>%
  group_by(customer_type, month) %>% 
  summarise(num_of_rides = n())
```

    ## `summarise()` has grouped output by 'customer_type'. You can override using the
    ## `.groups` argument.

``` r
View(all_2022_data_test_rides)
```

\#PLOT FOR RIDES BY CUSTOMER TYPE library(ggplot2)
ggplot(all_2022_data_summary, aes(x = customer_type, y =
num_of_rides)) + geom_col() \#PLOT FOR RIDES BY CUSTOMER TYPE BY MONTH
ggplot(all_2022_data_summary, aes(x = customer_type, y =
num_of_rides)) + geom_col() + facet_wrap(~month)

\#MEAN RIDE TIME BY CUSTOMER TYPE

all_2022_data_test_time \<- all_2022_data_test %\>%
group_by(customer_type, month) %\>% summarise(mean_ride_len =
mean(ride_len))
