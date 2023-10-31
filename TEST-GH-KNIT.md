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

## INITIAL DATA CHECK

``` r
library(dplyr)

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
library(skimr)

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

All data was imported and binded properly and it is organized in 5 632
443 rows and 19 columns, all rows complete_rate is equal to 1 (which
indicates that it is 100% complete). Except end_station_id row
(complere_rate = 0,841), the reason of that was mentioned before.

Completion rate and consistency of min and max values of ride_id row
shows positive evaluation throughout the whole dataset. Data frame above
shows additionally that number of start_station_names/end_station_names
and start_station_ids/end_station_ids are not equal what confirms
assumption made while data cleaning.

Data check showed that started_at, ended_at and ride_len columns were
imported as characters instead of datetime and duration data types. So
it needs to be changed:

## DATA TYPES CHANGE

``` r
library(tidyverse)
library(lubridate)

all_2022_data <- all_2022_data %>% 
  mutate(
    started_at = as_datetime(started_at, format = '%d.%m.%Y %H:%M'),
    ended_at = as_datetime(ended_at, format = '%d.%m.%Y %H:%M'),
    ride_len = as.numeric(difftime(ended_at, started_at, units = 'min'))
         )

glimpse(all_2022_data)
```

    ## Rows: 5,632,443
    ## Columns: 19
    ## $ ride_id                <chr> "94696E127B0B5C2E", "2F30091C42C1A659", "FFF47E…
    ## $ rideable_type          <chr> "classic_bike", "classic_bike", "classic_bike",…
    ## $ started_at             <dttm> 2022-01-01 09:22:00, 2022-01-10 07:15:00, 2022…
    ## $ ended_at               <dttm> 2022-01-02 10:22:00, 2022-01-11 08:15:00, 2022…
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
    ## $ ride_len               <dbl> 1500, 1500, 1380, 1380, 1378, 1346, 1321, 1270,…
    ## $ start_day_of_week      <int> 6, 1, 1, 1, 7, 7, 5, 1, 4, 4, 3, 1, 7, 7, 4, 2,…
    ## $ started_at_part_of_day <chr> "Morning", "Morning", "Afternoon", "Morning", "…

As all data were merged into one file, additional month column will be
needed for analysis:

## ADDITIONAL COLUMNS FOR CALCS AND VIZ

``` r
all_2022_data <- all_2022_data %>% 
  mutate(month = substr(started_at, 6, 7)
        )

glimpse(all_2022_data)
```

    ## Rows: 5,632,443
    ## Columns: 20
    ## $ ride_id                <chr> "94696E127B0B5C2E", "2F30091C42C1A659", "FFF47E…
    ## $ rideable_type          <chr> "classic_bike", "classic_bike", "classic_bike",…
    ## $ started_at             <dttm> 2022-01-01 09:22:00, 2022-01-10 07:15:00, 2022…
    ## $ ended_at               <dttm> 2022-01-02 10:22:00, 2022-01-11 08:15:00, 2022…
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
    ## $ ride_len               <dbl> 1500, 1500, 1380, 1380, 1378, 1346, 1321, 1270,…
    ## $ start_day_of_week      <int> 6, 1, 1, 1, 7, 7, 5, 1, 4, 4, 3, 1, 7, 7, 4, 2,…
    ## $ started_at_part_of_day <chr> "Morning", "Morning", "Afternoon", "Morning", "…
    ## $ month                  <chr> "01", "01", "01", "01", "01", "01", "01", "01",…

## SUMMARY OF DATA BEFORE CALCS

``` r
all_2022_data_summary <- all_2022_data %>%
  group_by(customer_type, month, rideable_type) %>% 
  summarise(
    num_of_rides = n(),
    mean_ride_len = mean(ride_len),
    mode_of_day_of_week = median(start_day_of_week),
    mode_of_part_of_day_1 = names(sort(table(started_at_part_of_day), decreasing = TRUE)[1]),
    )

glimpse(all_2022_data_summary)
```

## SUMMARY ABOVE SHOWS THAT ALL SUBGROUPS RIDES mode_of_part_of_day =‘Afternoon’ \#SO I DECIDED TO ADD A NEW COLUMN WITH FULL HOUR WHEN RIDE

STARTED:

``` r
all_2022_data <- all_2022_data %>% 
  mutate(started_at_full_hour = substr(started_at_hour, 1, 2))
```

## FINAL DF SUMMARY OF DATA BEFORE CALCS

``` r
all_2022_data_summary <- all_2022_data %>%
  group_by(customer_type, month, rideable_type, start_day_of_week, started_at_part_of_day, started_at_full_hour) %>% 
  summarise(
    num_of_rides = n(),
    mean_ride_len = mean(ride_len)
            )

glimpse(all_2022_data_summary)
```

    ## Rows: 10,078
    ## Columns: 8
    ## Groups: customer_type, month, rideable_type, start_day_of_week, started_at_part_of_day [1,680]
    ## $ customer_type          <chr> "casual", "casual", "casual", "casual", "casual…
    ## $ month                  <chr> "01", "01", "01", "01", "01", "01", "01", "01",…
    ## $ rideable_type          <chr> "classic_bike", "classic_bike", "classic_bike",…
    ## $ start_day_of_week      <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ started_at_part_of_day <chr> "Afternoon", "Afternoon", "Afternoon", "Afterno…
    ## $ started_at_full_hour   <chr> "12", "13", "14", "15", "16", "17", "18", "19",…
    ## $ num_of_rides           <int> 55, 48, 74, 55, 92, 89, 71, 56, 25, 22, 26, 11,…
    ## $ mean_ride_len          <dbl> 27.400000, 49.333333, 47.837838, 23.145455, 19.…

## ??????????????????? PRZENIEŚĆ WYŻEJ, EKSPORT MUSI BYĆ ZROBIONY DLA WSZYSTKICH DANYCH, TRZEBA SPRAWDZIĆ CZY PRZED CZY PO MUTATE AS THIS DATA WILL BE USED FOR SOME MORE VISUAL EXPLORATION AND PRESENTATION, IT HAS TO BE SAVED AS CSV FILE:

``` r
write.csv2(all_2022_data_summary, file = 'D:/GDAPC Case study/01_Bike_share/all_2022_data_R_summary_file.csv')
```

## BASIC CALCULATIONS AND PLOTS

\#TOTAL RIDES BY CUSTOMER TYPE

``` r
all_2022_data_rides <- all_2022_data %>%
  group_by(customer_type) %>% 
  summarise(num_of_rides = n())

head(all_2022_data_rides)
```

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = num_of_rides, fill = customer_type)) + geom_col(show.legend = FALSE) + labs(x = 'Customer type', y = 'Number of rides', title = 'Numer of rides performed in 2022', subtitle = 'by casual users and members', caption = 'Data source: Cyclistic bike-share system') + theme_light() + scale_y_continuous(labels = scales::comma) + scale_x_discrete()  + annotate('text', x = 1, y = 300000, label = '2 305 885', fontface = 'italic', size = 5.5) + annotate('text', x = 2, y = 300000, label = '3 326 558', fontface = 'italic', size = 5.5)
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

\#ADDING MONTH PARAMETER dodać kable() żeby wyszła tabela

``` r
all_2022_data_rides <- all_2022_data %>%
  group_by(customer_type, month) %>% 
  summarise(num_of_rides = n()) %>% 
  arrange(-num_of_rides)
```

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = num_of_rides, fill = customer_type)) + geom_col() + labs(x = NULL, y = 'Number of rides', title = 'Numer of rides performed in 2022', subtitle = 'by month by casual users and members', caption = 'Data source: Cyclistic bike-share system') + theme_light() + scale_y_continuous(labels = scales::comma) + facet_grid(~month) + theme(axis.text.x = element_blank()) + guides(fill = guide_legend(title = "Customer type"))
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
\###DODAĆ WYKRES SKUMULOWANY

``` r
ggplot(all_2022_data_summary, aes(x = month, y = num_of_rides, fill = customer_type)) + geom_col(show.legend = FALSE) + labs(x = 'Month', y = 'Number of rides', title = 'Numer of rides performed in 2022', subtitle = 'by month by casual users and members', caption = 'Data source: Cyclistic bike-share system') + theme_light() + scale_y_continuous(labels = scales::comma) + facet_grid(~customer_type) + guides(fill = guide_legend(title = "Customer type"))
```

## SUMMARY ABOVE SHOWS THAT IN A YEAR VIEW, MOST POPULAR MONTH AMONG CASUAL USERS ARE: ….(…RIDES),… AND …. WHILE IN GROUP OF USERS WHO HAS ANNUAL SUBSCRIPTION MOST POPULAR ARE:

\#ANOTHER THING IS THAT IN EACH MONTH MEMBER USERS PERFORMED MORE RIDES
THAN CASUAL USERS. WHAT MORE, MEMBER USERS PERFORM AT LEAST 350 000
RIDES PER MONTH IN ALMOST HALF OF THE YEAR (THE 6TH MOST POPULAR MONTH
FOR THIS GROUP IS OCTOBER WITH 347 690 RIDES) WHILE CASUAL USERS PERFORM
OVER 350 000 RIDES ONLY IN 3 MONTHS PER YEAR

## NUM OF RIDES STARTED AT EACH PART OF DAY BY USERS

``` r
ggplot(all_2022_data_summary, aes(x = started_at_part_of_day, y = mean_ride_len, fill = customer_type)) + geom_col(show.legend = FALSE) + labs(x = 'Month', y = 'Number of rides', title = 'Numer of rides performed in 2022', subtitle = 'by month by casual users and members', caption = 'Data source: Cyclistic bike-share system') + theme_light() + scale_y_continuous(labels = scales::comma) + scale_x_discrete(limits = c("Morning", "Afternoon", "Evening", "Night")) + facet_grid(~customer_type) + guides(fill = guide_legend(title = "Customer type")) + scale_fill_brewer()
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

## NUM OF RIDES STARTED AT EACH PART OF DAY BY USERS - DO POPRAWY KOLEJNOŚĆ

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = num_of_rides, fill = customer_type)) + geom_col() + labs(x = 'Month', y = 'Number of rides', title = 'Numer of rides performed in 2022', subtitle = 'by month by casual users and members', caption = 'Data source: Cyclistic bike-share system') + theme_light() + scale_y_continuous(labels = scales::comma) + facet_grid(~started_at_part_of_day) + guides(fill = guide_legend(title = "Customer type")) + scale_fill_brewer()
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## NUM OF RIDES STARTED AT EACH DAY OF WEEK BY USERS

``` r
ggplot(all_2022_data_summary, aes(x = start_day_of_week, y = num_of_rides, fill = customer_type)) + geom_col() + facet_wrap(~customer_type) + scale_x_discrete(limits = c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")) + scale_y_continuous(labels = scales::comma) + guides(x = guide_axis(angle = 45)) + scale_fill_brewer() + theme_light()
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

## NUM OF RIDES STARTED AT EACH DAY OF WEEK BY USERS

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = num_of_rides, fill = customer_type)) + geom_col() + facet_grid(~start_day_of_week)
```

\#MEAN RIDE TIME BY CUSTOMER TYPE

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean")
```

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean") + facet_grid(~month)
```

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

``` r
ggplot(all_2022_data_summary, aes(x = mean_ride_len, y = num_of_rides, color = customer_type)) + geom_point(stat = "summary", fun = "mean") + scale_x_continuous(limits = c(0, 360), breaks = seq(0, 360, by = 60))
```

    ## Warning: Removed 2 rows containing non-finite values (`stat_summary()`).

![](TEST-GH-KNIT_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

``` r
ggplot(all_2022_data_summary, aes(x = started_at_part_of_day, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean") + facet_wrap(~customer_type)
```

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean") + facet_wrap(~started_at_part_of_day)
```

``` r
ggplot(all_2022_data_summary, aes(x = started_at_part_of_day, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean") + facet_wrap(~customer_type)
```

``` r
ggplot(all_2022_data_summary, aes(x = customer_type, y = mean_ride_len)) + geom_bar(stat = "summary", fun = "mean") + facet_wrap(~started_at_part_of_day)
```
