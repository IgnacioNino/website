---
title: "Homework 1"
author: "Ignacio Nino"
date: 2023-05-19
format: 
  docx: default
  html:
    toc: true
    toc_float: true
    code-fold: true
editor: visual
---


## This is the result of the first homework of the course "E628 SUM23 Data Science For Business" at London Business School
## I analysed data of NYC flights to get a first understanding of how to use R



```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.2     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
## ✔ purrr     1.0.1     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library(nycflights13)
library(skimr)
data(flights)

#Understand the data by visualizing it and summarizing variables
view(flights)
summary(flights)
```

```
##       year          month             day           dep_time    sched_dep_time
##  Min.   :2013   Min.   : 1.000   Min.   : 1.00   Min.   :   1   Min.   : 106  
##  1st Qu.:2013   1st Qu.: 4.000   1st Qu.: 8.00   1st Qu.: 907   1st Qu.: 906  
##  Median :2013   Median : 7.000   Median :16.00   Median :1401   Median :1359  
##  Mean   :2013   Mean   : 6.549   Mean   :15.71   Mean   :1349   Mean   :1344  
##  3rd Qu.:2013   3rd Qu.:10.000   3rd Qu.:23.00   3rd Qu.:1744   3rd Qu.:1729  
##  Max.   :2013   Max.   :12.000   Max.   :31.00   Max.   :2400   Max.   :2359  
##                                                  NA's   :8255                 
##    dep_delay          arr_time    sched_arr_time   arr_delay       
##  Min.   : -43.00   Min.   :   1   Min.   :   1   Min.   : -86.000  
##  1st Qu.:  -5.00   1st Qu.:1104   1st Qu.:1124   1st Qu.: -17.000  
##  Median :  -2.00   Median :1535   Median :1556   Median :  -5.000  
##  Mean   :  12.64   Mean   :1502   Mean   :1536   Mean   :   6.895  
##  3rd Qu.:  11.00   3rd Qu.:1940   3rd Qu.:1945   3rd Qu.:  14.000  
##  Max.   :1301.00   Max.   :2400   Max.   :2359   Max.   :1272.000  
##  NA's   :8255      NA's   :8713                  NA's   :9430      
##    carrier              flight       tailnum             origin         
##  Length:336776      Min.   :   1   Length:336776      Length:336776     
##  Class :character   1st Qu.: 553   Class :character   Class :character  
##  Mode  :character   Median :1496   Mode  :character   Mode  :character  
##                     Mean   :1972                                        
##                     3rd Qu.:3465                                        
##                     Max.   :8500                                        
##                                                                         
##      dest              air_time        distance         hour      
##  Length:336776      Min.   : 20.0   Min.   :  17   Min.   : 1.00  
##  Class :character   1st Qu.: 82.0   1st Qu.: 502   1st Qu.: 9.00  
##  Mode  :character   Median :129.0   Median : 872   Median :13.00  
##                     Mean   :150.7   Mean   :1040   Mean   :13.18  
##                     3rd Qu.:192.0   3rd Qu.:1389   3rd Qu.:17.00  
##                     Max.   :695.0   Max.   :4983   Max.   :23.00  
##                     NA's   :9430                                  
##      minute        time_hour                     
##  Min.   : 0.00   Min.   :2013-01-01 05:00:00.00  
##  1st Qu.: 8.00   1st Qu.:2013-04-04 13:00:00.00  
##  Median :29.00   Median :2013-07-03 10:00:00.00  
##  Mean   :26.23   Mean   :2013-07-03 05:22:54.64  
##  3rd Qu.:44.00   3rd Qu.:2013-10-01 07:00:00.00  
##  Max.   :59.00   Max.   :2013-12-31 23:00:00.00  
## 
```

```r
skimr::skim(flights)
```


Table: <span id="tab:unnamed-chunk-1"></span>Table 1: Data summary

|                         |        |
|:------------------------|:-------|
|Name                     |flights |
|Number of rows           |336776  |
|Number of columns        |19      |
|_______________________  |        |
|Column type frequency:   |        |
|character                |4       |
|numeric                  |14      |
|POSIXct                  |1       |
|________________________ |        |
|Group variables          |None    |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|carrier       |         0|          1.00|   2|   2|     0|       16|          0|
|tailnum       |      2512|          0.99|   5|   6|     0|     4043|          0|
|origin        |         0|          1.00|   3|   3|     0|        3|          0|
|dest          |         0|          1.00|   3|   3|     0|      105|          0|


**Variable type: numeric**

|skim_variable  | n_missing| complete_rate|    mean|      sd|   p0|  p25|  p50|  p75| p100|hist  |
|:--------------|---------:|-------------:|-------:|-------:|----:|----:|----:|----:|----:|:-----|
|year           |         0|          1.00| 2013.00|    0.00| 2013| 2013| 2013| 2013| 2013|▁▁▇▁▁ |
|month          |         0|          1.00|    6.55|    3.41|    1|    4|    7|   10|   12|▇▆▆▆▇ |
|day            |         0|          1.00|   15.71|    8.77|    1|    8|   16|   23|   31|▇▇▇▇▆ |
|dep_time       |      8255|          0.98| 1349.11|  488.28|    1|  907| 1401| 1744| 2400|▁▇▆▇▃ |
|sched_dep_time |         0|          1.00| 1344.25|  467.34|  106|  906| 1359| 1729| 2359|▁▇▇▇▃ |
|dep_delay      |      8255|          0.98|   12.64|   40.21|  -43|   -5|   -2|   11| 1301|▇▁▁▁▁ |
|arr_time       |      8713|          0.97| 1502.05|  533.26|    1| 1104| 1535| 1940| 2400|▁▃▇▇▇ |
|sched_arr_time |         0|          1.00| 1536.38|  497.46|    1| 1124| 1556| 1945| 2359|▁▃▇▇▇ |
|arr_delay      |      9430|          0.97|    6.90|   44.63|  -86|  -17|   -5|   14| 1272|▇▁▁▁▁ |
|flight         |         0|          1.00| 1971.92| 1632.47|    1|  553| 1496| 3465| 8500|▇▃▃▁▁ |
|air_time       |      9430|          0.97|  150.69|   93.69|   20|   82|  129|  192|  695|▇▂▂▁▁ |
|distance       |         0|          1.00| 1039.91|  733.23|   17|  502|  872| 1389| 4983|▇▃▂▁▁ |
|hour           |         0|          1.00|   13.18|    4.66|    1|    9|   13|   17|   23|▁▇▇▇▅ |
|minute         |         0|          1.00|   26.23|   19.30|    0|    8|   29|   44|   59|▇▃▆▃▅ |


**Variable type: POSIXct**

|skim_variable | n_missing| complete_rate|min                 |max                 |median              | n_unique|
|:-------------|---------:|-------------:|:-------------------|:-------------------|:-------------------|--------:|
|time_hour     |         0|             1|2013-01-01 05:00:00 |2013-12-31 23:00:00 |2013-07-03 10:00:00 |     6936|


# Data Manipulation

## Problem 1: Use logical operators to find flights that:

```         
```



```r
# 1. Had an arrival delay of two or more hours (> 120 minutes)

Problem1_1 <- flights %>% 
                filter(arr_delay > 120) %>% 
                arrange(arr_delay) #to see if the minimum value is above 120
summary(Problem1_1['arr_delay'])
```

```
##    arr_delay     
##  Min.   : 121.0  
##  1st Qu.: 138.0  
##  Median : 164.0  
##  Mean   : 185.2  
##  3rd Qu.: 208.0  
##  Max.   :1272.0
```

```r
#Summarise table only for arr_delay variable. Data seems to be ok. It filtered 10,034 rows, and the lowest arr_delay value is above 120.

# -------------------------------------------------------------------------------
# 2. Flew to Houston (IAH or HOU)
Problem1_2 <- flights %>% 
                filter(dest %in% c("IAH","HOU"))
count(Problem1_2,dest) #Make sure that the only variables in "dest" are HOU or IAH
```

```
## # A tibble: 2 × 2
##   dest      n
##   <chr> <int>
## 1 HOU    2115
## 2 IAH    7198
```

```r
# -------------------------------------------------------------------------------
# 3. Were operated by United (`UA`), American (`AA`), or Delta (`DL`)
Problem1_3 <- flights %>% 
                filter(carrier %in% c("UA","AA","DL"))
count(Problem1_3,carrier) #Make sure that the filter is correct
```

```
## # A tibble: 3 × 2
##   carrier     n
##   <chr>   <int>
## 1 AA      32729
## 2 DL      48110
## 3 UA      58665
```

```r
# -------------------------------------------------------------------------------
# 4. Departed in summer (July, August, and September)
Problem1_4 <- flights %>% 
                filter(month %in% c("7","8","9"))
count(Problem1_4,month) #Make sure that the filter is correct
```

```
## # A tibble: 3 × 2
##   month     n
##   <int> <int>
## 1     7 29425
## 2     8 29327
## 3     9 27574
```

```r
# -------------------------------------------------------------------------------
# 5. Arrived more than two hours late, but didn't leave late
Problem1_5 <- flights %>% 
                filter(arr_delay > 120) %>% 
                filter(dep_delay <=0) %>% 
                arrange(arr_delay)
summary(Problem1_5[c('arr_delay', 'dep_delay')])
```

```
##    arr_delay       dep_delay      
##  Min.   :121.0   Min.   :-11.000  
##  1st Qu.:124.0   1st Qu.: -5.000  
##  Median :129.0   Median : -3.000  
##  Mean   :134.1   Mean   : -3.483  
##  3rd Qu.:140.0   3rd Qu.: -2.000  
##  Max.   :194.0   Max.   :  0.000
```

```r
#Summarise table to check filters. Both conditions are met.
# -------------------------------------------------------------------------------
# 6. Were delayed by at least an hour, but made up over 30 minutes in flight
Problem1_6 <- flights %>% 
                filter(dep_delay >= 60) %>% 
                filter(air_time >=30)
summary(Problem1_6[c('dep_delay', 'air_time')])
```

```
##    dep_delay         air_time    
##  Min.   :  60.0   Min.   : 30.0  
##  1st Qu.:  76.0   1st Qu.: 76.0  
##  Median : 100.0   Median :118.0  
##  Mean   : 120.9   Mean   :138.3  
##  3rd Qu.: 144.0   3rd Qu.:165.0  
##  Max.   :1301.0   Max.   :666.0
```

```r
#Summarise table to check filters. Both conditions are met.
```


## Problem 2: What months had the highest and lowest proportion of cancelled flights? Interpret any seasonal patterns. To determine if a flight was cancelled use the following code



```r
#Assumption: Proportion of cancelled flights is the division of cancelled flights in a particular month, over the total flights of that month.
#Group table by month, and count the total rows and the ones that has a cancelled flight

Problem2 <- flights %>% 
              group_by(month) %>% 
              summarize(
                N_flights = n(), #Count flights per month
                N_cancelled = sum(is.na(dep_time)), #Count cancelled flights per month
                Prop = N_cancelled / N_flights #Proportion
              )
Problem2 #View table
```

```
## # A tibble: 12 × 4
##    month N_flights N_cancelled    Prop
##    <int>     <int>       <int>   <dbl>
##  1     1     27004         521 0.0193 
##  2     2     24951        1261 0.0505 
##  3     3     28834         861 0.0299 
##  4     4     28330         668 0.0236 
##  5     5     28796         563 0.0196 
##  6     6     28243        1009 0.0357 
##  7     7     29425         940 0.0319 
##  8     8     29327         486 0.0166 
##  9     9     27574         452 0.0164 
## 10    10     28889         236 0.00817
## 11    11     27268         233 0.00854
## 12    12     28135        1025 0.0364
```

```r
# What months had the highest and lowest % of cancelled flights?
arrange(Problem2,desc(Prop)) #Sort table by prop
```

```
## # A tibble: 12 × 4
##    month N_flights N_cancelled    Prop
##    <int>     <int>       <int>   <dbl>
##  1     2     24951        1261 0.0505 
##  2    12     28135        1025 0.0364 
##  3     6     28243        1009 0.0357 
##  4     7     29425         940 0.0319 
##  5     3     28834         861 0.0299 
##  6     4     28330         668 0.0236 
##  7     5     28796         563 0.0196 
##  8     1     27004         521 0.0193 
##  9     8     29327         486 0.0166 
## 10     9     27574         452 0.0164 
## 11    11     27268         233 0.00854
## 12    10     28889         236 0.00817
```

```r
#Month with highest proportion of cancelled flights: February (5.05%)
#Month with lowest proportion of cancelled flights: September (1.64%)
```


## Problem 3: What plane (specified by the `tailnum` variable) traveled the most times from New York City airports in 2013? Please `left_join()` the resulting table with the table `planes` (also included in the `nycflights13` package).

For the plane with the greatest number of flights and that had more than 50 seats, please create a table where it flew to during 2013.



```r
#Filter flights from NYC and summarizing them for plane
Problem3 <- flights %>% 
              filter(origin %in% c('JFK','LGA','EWR')) %>% #Flights from NYC
              filter(!is.na(tailnum)) %>% #Filter rows with tailnum
              count(tailnum) %>% #Summarise for plane
              arrange(desc(n)) #Sort results
Problem3 #View table.
```

```
## # A tibble: 4,043 × 2
##    tailnum     n
##    <chr>   <int>
##  1 N725MQ    575
##  2 N722MQ    513
##  3 N723MQ    507
##  4 N711MQ    486
##  5 N713MQ    483
##  6 N258JB    427
##  7 N298JB    407
##  8 N353JB    404
##  9 N351JB    402
## 10 N735MQ    396
## # ℹ 4,033 more rows
```

```r
#Join tables to gather more information about planes
Problem3 <- left_join(Problem3,planes, by="tailnum")
Problem3 #View table
```

```
## # A tibble: 4,043 × 10
##    tailnum     n  year type        manufacturer model engines seats speed engine
##    <chr>   <int> <int> <chr>       <chr>        <chr>   <int> <int> <int> <chr> 
##  1 N725MQ    575    NA <NA>        <NA>         <NA>       NA    NA    NA <NA>  
##  2 N722MQ    513    NA <NA>        <NA>         <NA>       NA    NA    NA <NA>  
##  3 N723MQ    507    NA <NA>        <NA>         <NA>       NA    NA    NA <NA>  
##  4 N711MQ    486  1976 Fixed wing… GULFSTREAM … G115…       2    22    NA Turbo…
##  5 N713MQ    483    NA <NA>        <NA>         <NA>       NA    NA    NA <NA>  
##  6 N258JB    427  2006 Fixed wing… EMBRAER      ERJ …       2    20    NA Turbo…
##  7 N298JB    407  2009 Fixed wing… EMBRAER      ERJ …       2    20    NA Turbo…
##  8 N353JB    404  2012 Fixed wing… EMBRAER      ERJ …       2    20    NA Turbo…
##  9 N351JB    402  2012 Fixed wing… EMBRAER      ERJ …       2    20    NA Turbo…
## 10 N735MQ    396    NA <NA>        <NA>         <NA>       NA    NA    NA <NA>  
## # ℹ 4,033 more rows
```

```r
#Identify plane that traveled the most
Problem3_plane = filter(Problem3,n==max(n))
Problem3_plane #Visualize results
```

```
## # A tibble: 1 × 10
##   tailnum     n  year type  manufacturer model engines seats speed engine
##   <chr>   <int> <int> <chr> <chr>        <chr>   <int> <int> <int> <chr> 
## 1 N725MQ    575    NA <NA>  <NA>         <NA>       NA    NA    NA <NA>
```

```r
##Unfortunately, there's no data for the plane that traveled the most from NYC in 2013. The plane is the "N725MQ", but there's no information about the model, manufacturer, etc.
##Given that the instructions ask for the plane with more than 50 seats, and the plane identified doesn't have seats information, I will run the code again to filter by seats

Problem3 <- Problem3 %>% 
              filter(seats>=50) #Filter planes with more than 50 seats
Problem3 #View table
```

```
## # A tibble: 3,200 × 10
##    tailnum     n  year type        manufacturer model engines seats speed engine
##    <chr>   <int> <int> <chr>       <chr>        <chr>   <int> <int> <int> <chr> 
##  1 N328AA    393  1986 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  2 N338AA    388  1987 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  3 N327AA    387  1986 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  4 N335AA    385  1987 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  5 N323AA    357  1986 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  6 N319AA    354  1985 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  7 N336AA    353  1987 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  8 N329AA    344  1987 Fixed wing… BOEING       767-…       2   255    NA Turbo…
##  9 N789JB    332  2011 Fixed wing… AIRBUS       A320…       2   200    NA Turbo…
## 10 N324AA    328  1986 Fixed wing… BOEING       767-…       2   255    NA Turbo…
## # ℹ 3,190 more rows
```

```r
Problem3_plane = filter(Problem3,n==max(n)) #Store the information of the plane
glimpse(Problem3_plane) #Print information of the plane
```

```
## Rows: 1
## Columns: 10
## $ tailnum      <chr> "N328AA"
## $ n            <int> 393
## $ year         <int> 1986
## $ type         <chr> "Fixed wing multi engine"
## $ manufacturer <chr> "BOEING"
## $ model        <chr> "767-223"
## $ engines      <int> 2
## $ seats        <int> 255
## $ speed        <int> NA
## $ engine       <chr> "Turbo-fan"
```

```r
#Destinations table for the plane
Problem3_dest <- flights %>% 
                  right_join(Problem3_plane,flights, by="tailnum") %>%  
                  count(dest) %>% #Summarize results by destination
                  arrange(desc(n)) #Sort results
Problem3_dest #Show data
```

```
## # A tibble: 6 × 2
##   dest      n
##   <chr> <int>
## 1 LAX     313
## 2 SFO      52
## 3 MIA      25
## 4 BOS       1
## 5 MCO       1
## 6 SJU       1
```

```r
#Probably right_join wasn't the best way to do it, but it combines the plane that traveled the most, with the flights
##The table works, the destination where this plane flew the most was LAX
```


## Problem 4: The `nycflights13` package includes a table (`weather`) that describes the weather during 2013. Use that table to answer the following questions:

```         
-   What is the distribution of temperature (`temp`) in July 2013? Identify any important outliers in terms of the `wind_speed` variable.
-   What is the relationship between `dewp` and `humid`?
-   What is the relationship between `precip` and `visib`?
```



```r
view (weather) #Understand the table
P4 <- weather %>% 
              filter(month == 7) %>% #July flights
              group_by(day) %>% #Group the info by day
              summarise(P4_avg_weather = mean(temp),
                          P4_avg_ws = mean(wind_speed))
P4 #View table.
```

```
## # A tibble: 31 × 3
##      day P4_avg_weather P4_avg_ws
##    <int>          <dbl>     <dbl>
##  1     1           74.6      9.69
##  2     2           76.3     10.9 
##  3     3           77.4      9.32
##  4     4           81.0     NA   
##  5     5           82.7     10.4 
##  6     6           85.7     10.8 
##  7     7           85.9     10.9 
##  8     8           81.6     11.2 
##  9     9           81.2      8.41
## 10    10           80.6      9.97
## # ℹ 21 more rows
```

```r
#At an aggregate level, I can't visualice any month that act as an outlier. To analyze it at a more detailed level, I will plot the results and not averaging them

#Distribution of temperatures in July
P4_2 <- weather %>% 
          filter(month==7) #July flights

P4Plot_temp <- ggplot(data=P4_2,
                      aes(x=day,
                          y=temp))+
                  geom_point()
P4Plot_temp
```

<img src="/blogs/homework1_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
##Temperatures follows a pattern. The only day where it behave different was on July 25th, where temperatures were unsually low

#Distribution of Wind speed in July
P4Plot_ws <- ggplot(data=P4_2,
                      aes(x=day,
                          y=wind_speed))+
                  geom_point()
P4Plot_ws
```

```
## Warning: Removed 2 rows containing missing values (`geom_point()`).
```

<img src="/blogs/homework1_files/figure-html/unnamed-chunk-5-2.png" width="672" />

```r
##There are some outliers in the data. Mostly days where wind_speed was 0 and >25

#Relationship between `dewp` and `humid`
P4Plot_dewp_humid <- ggplot(data=P4_2,
                      aes(x=dewp,
                          y=humid))+
                  geom_point()
P4Plot_dewp_humid
```

<img src="/blogs/homework1_files/figure-html/unnamed-chunk-5-3.png" width="672" />

```r
## It appears that the variables have a positive correlation. My guess is that here there are at least 3 clusters (you can see three big groups that could have an exponential relationship). I guess that if we filter with another variable we could see that relationship.

#Relationship between `precip` and `visib`

P4Plot_precip_visib <- ggplot(data=P4_2,
                              aes(x=precip,
                                  y=visib))+
                        geom_point()
P4Plot_precip_visib
```

<img src="/blogs/homework1_files/figure-html/unnamed-chunk-5-4.png" width="672" />

```r
#There's not likely any correlation between these variables
```


## Problem 5: Use the `flights` and `planes` tables to answer the following questions:

```         
-   How many planes have a missing date of manufacture?
-   What are the five most common manufacturers?
-   Has the distribution of manufacturer changed over time as reflected by the airplanes flying from NYC in 2013? (Hint: you may need to use case_when() to recode the manufacturer name and collapse rare vendors into a category called Other.)
```



```r
#Planes with missing date of manufacture

view(planes) #Show table
## Looking at the table, there's no plane without a date of manufacture

#Five most common manufacturers
P5_2 <- planes %>% #Use planes table to summarize it for manufacturers
        filter(!is.na(manufacturer)) %>% #Filter rows with manufacturers
        count(manufacturer) %>% #Summarise for every manufacturer
        arrange(desc(n)) #Sort results
P5_2 #View table.
```

```
## # A tibble: 35 × 2
##    manufacturer                      n
##    <chr>                         <int>
##  1 BOEING                         1630
##  2 AIRBUS INDUSTRIE                400
##  3 BOMBARDIER INC                  368
##  4 AIRBUS                          336
##  5 EMBRAER                         299
##  6 MCDONNELL DOUGLAS               120
##  7 MCDONNELL DOUGLAS AIRCRAFT CO   103
##  8 MCDONNELL DOUGLAS CORPORATION    14
##  9 CANADAIR                          9
## 10 CESSNA                            9
## # ℹ 25 more rows
```

```r
## The 5 most common manufacturers are BOEING, AIRBUS INDUSTRIE, BOMBARDIER INC, AIRBUS, and EMBRAER

#Distribution of manufacturers for planes flying from NYC

P5_3 <- left_join(flights,planes, by="tailnum") %>%  #Join tables
          filter(origin %in% c('JFK','LGA','EWR')) %>%  #Flights from NYC
          group_by(month,manufacturer) %>% #See the evolution for every month
          summarise(count=n())
```

```
## `summarise()` has grouped output by 'month'. You can override using the
## `.groups` argument.
```

```r
P5_3 #View table
```

```
## # A tibble: 372 × 3
## # Groups:   month [12]
##    month manufacturer          count
##    <int> <chr>                 <int>
##  1     1 AGUSTA SPA                3
##  2     1 AIRBUS                 3916
##  3     1 AIRBUS INDUSTRIE       3367
##  4     1 AMERICAN AIRCRAFT INC     8
##  5     1 AVIAT AIRCRAFT INC        5
##  6     1 BARKER JACK L            26
##  7     1 BEECH                     7
##  8     1 BELL                      3
##  9     1 BOEING                 6623
## 10     1 BOMBARDIER INC         1925
## # ℹ 362 more rows
```


## Problem 6: Use the `flights` and `planes` tables to answer the following questions:

```         
-   What is the oldest plane (specified by the tailnum variable) that flew from New York City airports in 2013?
-   How many airplanes that flew from New York City are included in the planes table?
```



```r
#Oldest plane
P6_1 <- left_join(flights,planes, by="tailnum") %>%  #Join tables
          filter(origin %in% c('JFK','LGA','EWR')) %>%  #Flights from NYC
          filter(year.y==min(year.y,na.rm=TRUE)) %>% #Filter rows with the oldest plane
          distinct(tailnum) #One row per plane
          
P6_1 #View table
```

```
## # A tibble: 1 × 1
##   tailnum
##   <chr>  
## 1 N381AA
```

```r
##The oldest plane is N381AA

#Planes from NYC included in the table
P6_2 <- left_join(flights,planes, by="tailnum") %>%  #Join tables
          filter(origin %in% c('JFK','LGA','EWR')) %>%  #Flights from NYC
          filter(!is.na(tailnum)) %>% #Filter rows with data in tailnum
          summarise(n_distinct(tailnum))
P6_2 #View table
```

```
## # A tibble: 1 × 1
##   `n_distinct(tailnum)`
##                   <int>
## 1                  4043
```

```r
#There are 4043 different planes that flew from NYC in 2013
```


## Problem 7: Use the `nycflights13` to answer the following questions:

```         
-   What is the median arrival delay on a month-by-month basis in each airport?
-   For each airline, plot the median arrival delay for each month and origin airport.
```



```r
#Monthly median arrival delay in each airport
P7_1 <- flights %>% 
        group_by(origin,month) %>% #Group by airport and month
        summarise(median_ad=median(arr_delay,na.rm=TRUE)) #Calculate median
```

```
## `summarise()` has grouped output by 'origin'. You can override using the
## `.groups` argument.
```

```r
P7_1 #View table
```

```
## # A tibble: 36 × 3
## # Groups:   origin [3]
##    origin month median_ad
##    <chr>  <int>     <dbl>
##  1 EWR        1         0
##  2 EWR        2        -2
##  3 EWR        3        -4
##  4 EWR        4        -1
##  5 EWR        5        -6
##  6 EWR        6        -1
##  7 EWR        7        -2
##  8 EWR        8        -5
##  9 EWR        9       -13
## 10 EWR       10        -6
## # ℹ 26 more rows
```

```r
#Plot median arrival rate for each AIRLINE, MONTH, and ORIGIN
P7_2 <- flights %>% 
        group_by(carrier,month,origin) %>% #Group by variables
        summarise(median_ad2=median(arr_delay,na.rm=TRUE)) #Calculate median
```

```
## `summarise()` has grouped output by 'carrier', 'month'. You can override using
## the `.groups` argument.
```

```r
P7_2
```

```
## # A tibble: 399 × 4
## # Groups:   carrier, month [185]
##    carrier month origin median_ad2
##    <chr>   <int> <chr>       <dbl>
##  1 9E          1 EWR          -1  
##  2 9E          1 JFK          -4  
##  3 9E          1 LGA           0  
##  4 9E          2 EWR          -8  
##  5 9E          2 JFK          -6  
##  6 9E          2 LGA          -1  
##  7 9E          3 EWR         -14  
##  8 9E          3 JFK         -11  
##  9 9E          3 LGA         -11  
## 10 9E          4 EWR         -15.5
## # ℹ 389 more rows
```

```r
P7_2_PLOT <- ggplot(data=P7_2,
                    aes(x=month,
                        y=median_ad2))+
          geom_point()+
          facet_wrap(~carrier) #Group by carrier
P7_2_PLOT
```

<img src="/blogs/homework1_files/figure-html/unnamed-chunk-8-1.png" width="672" />


## Problem 8: Let's take a closer look at what carriers service the route to San Francisco International (SFO). Join the `flights` and `airlines` tables and count which airlines flew the most to SFO. Produce a new dataframe, `fly_into_sfo` that contains three variables: the `name` of the airline, e.g., `United Air Lines Inc.` not `UA`, the count (number) of times it flew to SFO, and the `percent` of the trips that that particular airline flew to SFO.



```r
#Airlines that flew the most to SFO
view(airlines) #Explore data
P8 <- left_join(flights,airlines,by="carrier") %>%  #Join tables
      filter(dest=="SFO") %>% #Flights to SFO
      count(name) %>% #Summarise by number of flights
      arrange(desc(n)) #Sort
      
P8
```

```
## # A tibble: 5 × 2
##   name                       n
##   <chr>                  <int>
## 1 United Air Lines Inc.   6819
## 2 Virgin America          2197
## 3 Delta Air Lines Inc.    1858
## 4 American Airlines Inc.  1422
## 5 JetBlue Airways         1035
```

```r
##There are 5 airlines that flew to SFO: United Air Lines Inc.,Virgin America, Delta Air Lines Inc., American Airlines Inc., and JetBlue Airways

#Fly_into_sfo

fly_into_sfo <- left_join(flights,airlines,by="carrier") %>%  #Join tables
                group_by(name) %>% 
                summarize(
                  N_flights = n(), #Count flights per airline
                  count = sum(dest=="SFO", na.rm=TRUE), #Count flights to SFO
                  percent = sum(dest=="SFO", na.rm=TRUE)/n() #Proportion
                ) %>% 
                arrange(desc(percent))
fly_into_sfo #View table
```

```
## # A tibble: 16 × 4
##    name                        N_flights count percent
##    <chr>                           <int> <int>   <dbl>
##  1 Virgin America                   5162  2197  0.426 
##  2 United Air Lines Inc.           58665  6819  0.116 
##  3 American Airlines Inc.          32729  1422  0.0434
##  4 Delta Air Lines Inc.            48110  1858  0.0386
##  5 JetBlue Airways                 54635  1035  0.0189
##  6 AirTran Airways Corporation      3260     0  0     
##  7 Alaska Airlines Inc.              714     0  0     
##  8 Endeavor Air Inc.               18460     0  0     
##  9 Envoy Air                       26397     0  0     
## 10 ExpressJet Airlines Inc.        54173     0  0     
## 11 Frontier Airlines Inc.            685     0  0     
## 12 Hawaiian Airlines Inc.            342     0  0     
## 13 Mesa Airlines Inc.                601     0  0     
## 14 SkyWest Airlines Inc.              32     0  0     
## 15 Southwest Airlines Co.          12275     0  0     
## 16 US Airways Inc.                 20536     0  0
```

```r
#Table seems to work.It makes sense because only shows 5 airlines with flights to SFO, which is true.
```


And here is some bonus ggplot code to plot your dataframe



```r
fly_into_sfo %>% 
  
  # sort 'name' of airline by the numbers it times to flew to SFO
  mutate(name = fct_reorder(name, count)) %>% 
  
  ggplot() +
  
  aes(x = count, 
      y = name) +
  
  # a simple bar/column plot
  geom_col() +
  
  # add labels, so each bar shows the % of total flights 
  geom_text(aes(label = percent),
             hjust = 1, 
             colour = "white", 
             size = 5)+
  
  # add labels to help our audience  
  labs(title="Which airline dominates the NYC to SFO route?", 
       subtitle = "as % of total flights in 2013",
       x= "Number of flights",
       y= NULL) +
  
  theme_minimal() + 
  
  # change the theme-- i just googled those , but you can use the ggThemeAssist add-in
  # https://cran.r-project.org/web/packages/ggThemeAssist/index.html
  
  theme(#
    # so title is left-aligned
    plot.title.position = "plot",
    
    # text in axes appears larger        
    axis.text = element_text(size=12),
    
    # title text is bigger
    plot.title = element_text(size=18)
      ) +

  # add one final layer of NULL, so if you comment out any lines
  # you never end up with a hanging `+` that awaits another ggplot layer
  NULL
```

<img src="/blogs/homework1_files/figure-html/ggplot-flights-toSFO-1.png" width="672" />


## Problem 9: Let's take a look at cancellations of flights to SFO. We create a new dataframe `cancellations` as follows



```r
cancellations <- flights %>% 
  
  # just filter for destination == 'SFO'
  filter(dest == 'SFO') %>% 
  
  # a cancelled flight is one with no `dep_time` 
  filter(is.na(dep_time))

#I want you to think how we would organise our data manipulation to create the following plot. No need to write the code, just explain in words how you would go about it.

##ANSWER

#First, we need to join the data with airlines table to obtain the name of the airline

#Then, we need to summarize the data with the following fields:
## name (airline)
## month
## dep_time (the value we will plot)
## origin (to differentiate graphs for destination to EWR or JFK)
# To summarize, we need to count the times a flight was cancelled (and plot that value)

#Finally, plot the graph with all of these variables mentioned above
```


## Problem 10: On your own -- Hollywood Age Gap

The website <https://hollywoodagegap.com> is a record of *THE AGE DIFFERENCE IN YEARS BETWEEN MOVIE LOVE INTERESTS*. This is an informational site showing the age gap between movie love interests and the data follows certain rules:

-   The two (or more) actors play actual love interests (not just friends, coworkers, or some other non-romantic type of relationship)
-   The youngest of the two actors is at least 17 years old
-   No animated characters

The age gaps dataset includes "gender" columns, which always contain the values "man" or "woman". These values appear to indicate how the characters in each film identify and some of these values do not match how the actor identifies. We apologize if any characters are misgendered in the data!

The following is a data dictionary of the variables used

| variable            | class     | description                                                                                             |
|:----------------|:----------------|:-------------------------------------|
| movie_name          | character | Name of the film                                                                                        |
| release_year        | integer   | Release year                                                                                            |
| director            | character | Director of the film                                                                                    |
| age_difference      | integer   | Age difference between the characters in whole years                                                    |
| couple_number       | integer   | An identifier for the couple in case multiple couples are listed for this film                          |
| actor_1\_name       | character | The name of the older actor in this couple                                                              |
| actor_2\_name       | character | The name of the younger actor in this couple                                                            |
| character_1\_gender | character | The gender of the older character, as identified by the person who submitted the data for this couple   |
| character_2\_gender | character | The gender of the younger character, as identified by the person who submitted the data for this couple |
| actor_1\_birthdate  | date      | The birthdate of the older member of the couple                                                         |
| actor_2\_birthdate  | date      | The birthdate of the younger member of the couple                                                       |
| actor_1\_age        | integer   | The age of the older actor when the film was released                                                   |
| actor_2\_age        | integer   | The age of the younger actor when the film was released                                                 |



```r
age_gaps <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2023/2023-02-14/age_gaps.csv')
```

```
## Rows: 1155 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (6): movie_name, director, actor_1_name, actor_2_name, character_1_gend...
## dbl  (5): release_year, age_difference, couple_number, actor_1_age, actor_2_age
## date (2): actor_1_birthdate, actor_2_birthdate
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


How would you explore this data set? Here are some ideas of tables/ graphs to help you with your analysis

-   How is `age_difference` distributed? What's the 'typical' `age_difference` in movies?

-   The `half plus seven\` rule. Large age disparities in relationships carry certain stigmas. One popular rule of thumb is the [half-your-age-plus-seven](https://en.wikipedia.org/wiki/Age_disparity_in_sexual_relationships#The_.22half-your-age-plus-seven.22_rule) rule. This rule states you should never date anyone under half your age plus seven, establishing a minimum boundary on whom one can date. In order for a dating relationship to be acceptable under this rule, your partner's age must be:

`$$\frac{\text{Your age}}{2} + 7 \\\< \text{Partner Age} \\\< (\text{Your age} - 7) \\\* 2$$`
How frequently does this rule apply in this dataset?

-   Which movie has the greatest number of love interests?
-   Which actors/ actresses have the greatest number of love interests in this dataset?
-   Is the mean/median age difference staying constant over the years (1935 - 2022)?
-   How frequently does Hollywood depict same-gender love interests?

# Details

-   Who did you collaborate with: No one.
-   Approximately how much time did you spend on this problem set: Don't remember, but I believe between 6-8 hours
-   What, if anything, gave you the most trouble: GIT :(

> As a true test to yourself, do you understand the code you submitted and are you able to explain it to someone else? Sure!

