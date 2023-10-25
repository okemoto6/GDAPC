Untitled
================

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

## Including Code

You can include R code in the document as follows:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

## Including Plots

You can also embed plots, for example:

![](TEST-GH-KNIT_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

## DATA IMPORT AND BIND

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

``` r
head(all_2022_data)
```

    ##            ride_id rideable_type       started_at         ended_at
    ## 1 94696E127B0B5C2E  classic_bike 01.01.2022 09:22 02.01.2022 10:22
    ## 2 2F30091C42C1A659  classic_bike 10.01.2022 07:15 11.01.2022 08:15
    ## 3 FFF47E9771BE076B  classic_bike 24.01.2022 12:25 25.01.2022 11:25
    ## 4 FF90D6D1A1A81046   docked_bike 10.01.2022 11:34 11.01.2022 10:34
    ## 5 89F04C89178D2898  classic_bike 30.01.2022 10:30 31.01.2022 09:28
    ## 6 ED144BF52E321C25  classic_bike 30.01.2022 11:02 31.01.2022 09:28
    ##             start_station_name start_station_id        end_station_name
    ## 1              Millennium Park            13008                        
    ## 2 Sheffield Ave & Waveland Ave     TA1307000126                        
    ## 3     University Ave & 57th St     KA1503000071 Lake Park Ave & 56th St
    ## 4    Clinton St & Roosevelt Rd           WL-008  Michigan Ave & 14th St
    ## 5  Southport Ave & Belmont Ave            13229                        
    ## 6  Southport Ave & Belmont Ave            13229                        
    ##   end_station_id start_lat start_lng  end_lat   end_lng customer_type
    ## 1                 41.88103 -87.62408 41.87000 -87.63000        member
    ## 2                 41.94940 -87.65453 41.94000 -87.66000        member
    ## 3   TA1309000063  41.79148 -87.59986 41.79324 -87.58778        member
    ## 4   TA1307000124  41.86712 -87.64109 41.86406 -87.62373        casual
    ## 5                 41.93948 -87.66375 41.94000 -87.65000        member
    ## 6                 41.93948 -87.66375 41.94000 -87.65000        member
    ##   started_at_hour started_at_hour_as_num ride_id_len ride_len start_day_of_week
    ## 1        09:22:49              0.3908449          16 24:59:55                 6
    ## 2        07:15:21              0.3023264          16 24:59:52                 1
    ## 3        12:25:04              0.5174074          16 23:00:00                 1
    ## 4        11:34:32              0.4823148          16 22:59:37                 1
    ## 5        10:30:32              0.4378704          16 22:57:29                 7
    ## 6        11:02:03              0.4597569          16 22:25:58                 7
    ##   started_at_part_of_day
    ## 1                Morning
    ## 2                Morning
    ## 3              Afternoon
    ## 4                Morning
    ## 5                Morning
    ## 6                Morning

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

## SUMMARY ABOVE SHOWS THAT ALL SUBGROUPS RIDES mode_of_part_of_day =

‘Afternoon’ \#SO I DECIDED TO ADD A NEW COLUMN WITH FULL HOUR WHEN RIDE
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
[<div class='tableauPlaceholder' id='viz1698234649937' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Da&#47;DashboardsStarterTemplate_16939014832940&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='DashboardsStarterTemplate_16939014832940&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Da&#47;DashboardsStarterTemplate_16939014832940&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1698234649937');                    var vizElement = divElement.getElementsByTagName('object')[0];                    if ( divElement.offsetWidth > 800 ) { vizElement.style.minWidth='420px';vizElement.style.maxWidth='100%';vizElement.style.minHeight='587px';vizElement.style.maxHeight=(divElement.offsetWidth*0.75)+'px';} else if ( divElement.offsetWidth > 500 ) { vizElement.style.minWidth='420px';vizElement.style.maxWidth='100%';vizElement.style.minHeight='587px';vizElement.style.maxHeight=(divElement.offsetWidth*0.75)+'px';} else { vizElement.style.width='100%';vizElement.style.height='727px';}                     var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
](https://public.tableau.com/views/DashboardsStarterTemplate_16939014832940/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)https://public.tableau.com/views/DashboardsStarterTemplate_16939014832940/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link
