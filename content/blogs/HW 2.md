---
categories:  
- ""    #the front matter should be like the one found in, e.g., blog2.md. It cannot be like the normal Rmd we used
- ""
date: "2021-09-30"
description: Flight Operation Analysis # the title that will show up once someone gets to this page
draft: false
image: flights.jpg # save picture in \static\img\blogs. Acceptable formats= jpg, jpeg, or png . Your iPhone pics wont work

keywords: ""
slug: flight_oper # slug is the shorthand URL address... no spaces plz
title: Flight Operation Analysis
---






# Data Manipulation

## Problem 1: Use logical operators to find flights that:

```         
-   Had an arrival delay of two or more hours (\> 120 minutes)
-   Flew to Houston (IAH or HOU)
-   Were operated by United (`UA`), American (`AA`), or Delta (`DL`)
-   Departed in summer (July, August, and September)
-   Arrived more than two hours late, but didn't leave late
-   Were delayed by at least an hour, but made up over 30 minutes in flight
```


```r
# Had an arrival delay of two or more hours (> 120 minutes)
flights %>% 
  filter(arr_delay>=120)
```

```
## # A tibble: 10,200 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      811            630       101     1047            830
##  2  2013     1     1      848           1835       853     1001           1950
##  3  2013     1     1      957            733       144     1056            853
##  4  2013     1     1     1114            900       134     1447           1222
##  5  2013     1     1     1505           1310       115     1638           1431
##  6  2013     1     1     1525           1340       105     1831           1626
##  7  2013     1     1     1549           1445        64     1912           1656
##  8  2013     1     1     1558           1359       119     1718           1515
##  9  2013     1     1     1732           1630        62     2028           1825
## 10  2013     1     1     1803           1620       103     2008           1750
## # ℹ 10,190 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
# Flew to Houston (IAH or HOU)
flights %>% 
  filter(dest == "IAH" | dest == "HOU")
```

```
## # A tibble: 9,313 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      623            627        -4      933            932
##  4  2013     1     1      728            732        -4     1041           1038
##  5  2013     1     1      739            739         0     1104           1038
##  6  2013     1     1      908            908         0     1228           1219
##  7  2013     1     1     1028           1026         2     1350           1339
##  8  2013     1     1     1044           1045        -1     1352           1351
##  9  2013     1     1     1114            900       134     1447           1222
## 10  2013     1     1     1205           1200         5     1503           1505
## # ℹ 9,303 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
# Were operated by United (`UA`), American (`AA`), or Delta (`DL`)
flights %>% 
  filter(carrier %in% c("UA","AA","DL"))
```

```
## # A tibble: 139,504 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      554            600        -6      812            837
##  5  2013     1     1      554            558        -4      740            728
##  6  2013     1     1      558            600        -2      753            745
##  7  2013     1     1      558            600        -2      924            917
##  8  2013     1     1      558            600        -2      923            937
##  9  2013     1     1      559            600        -1      941            910
## 10  2013     1     1      559            600        -1      854            902
## # ℹ 139,494 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
# Departed in summer (July, August, and September)
flights %>% 
  filter(month %in% c("7","8","9"))   
```

```
## # A tibble: 86,326 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     7     1        1           2029       212      236           2359
##  2  2013     7     1        2           2359         3      344            344
##  3  2013     7     1       29           2245       104      151              1
##  4  2013     7     1       43           2130       193      322             14
##  5  2013     7     1       44           2150       174      300            100
##  6  2013     7     1       46           2051       235      304           2358
##  7  2013     7     1       48           2001       287      308           2305
##  8  2013     7     1       58           2155       183      335             43
##  9  2013     7     1      100           2146       194      327             30
## 10  2013     7     1      100           2245       135      337            135
## # ℹ 86,316 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
# Arrived more than two hours late, but didn't leave late
flights %>% 
  filter(arr_delay>120 & dep_delay<=0) 
```

```
## # A tibble: 29 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1    27     1419           1420        -1     1754           1550
##  2  2013    10     7     1350           1350         0     1736           1526
##  3  2013    10     7     1357           1359        -2     1858           1654
##  4  2013    10    16      657            700        -3     1258           1056
##  5  2013    11     1      658            700        -2     1329           1015
##  6  2013     3    18     1844           1847        -3       39           2219
##  7  2013     4    17     1635           1640        -5     2049           1845
##  8  2013     4    18      558            600        -2     1149            850
##  9  2013     4    18      655            700        -5     1213            950
## 10  2013     5    22     1827           1830        -3     2217           2010
## # ℹ 19 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
# Were delayed by at least an hour, but made up over 30 minutes in flight
flights %>% 
  filter(dep_delay>=60 & dep_delay-arr_delay>30) 
```

```
## # A tibble: 1,844 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1     2205           1720       285       46           2040
##  2  2013     1     1     2326           2130       116      131             18
##  3  2013     1     3     1503           1221       162     1803           1555
##  4  2013     1     3     1839           1700        99     2056           1950
##  5  2013     1     3     1850           1745        65     2148           2120
##  6  2013     1     3     1941           1759       102     2246           2139
##  7  2013     1     3     1950           1845        65     2228           2227
##  8  2013     1     3     2015           1915        60     2135           2111
##  9  2013     1     3     2257           2000       177       45           2224
## 10  2013     1     4     1917           1700       137     2135           1950
## # ℹ 1,834 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
## Problem 2: What months had the highest and lowest proportion of cancelled flights? Interpret any seasonal patterns. To determine if a flight was cancelled use the following code
```
flights %>% 
  filter(is.na(dep_time)) 
```


```r
# What months had the highest and lowest % of cancelled flights?
cancelled_flights <- flights %>%
  group_by(month) %>%
#find portion of cancelled flights
  summarize(prop = sum(is.na(dep_time))/n()) %>% 
  arrange(desc(prop))
  
#Ans: February has the highest % of cancelled flights at 5.05% nnd October has the lowest % of cancelled flights at 0.817%. 
```

## Problem 3: What plane (specified by the `tailnum` variable) traveled the most times from New York City airports in 2013? Please `left_join()` the resulting table with the table `planes` (also included in the `nycflights13` package).

For the plane with the greatest number of flights and that had more than 50 seats, please create a table where it flew to during 2013.


```r
selected_flights <- flights %>% 
  #filter only origin from NYC and in year 2013
  filter(origin %in% c("JFK","EWR","LGA") & year == 2013) %>% 
  #filter flights that have tailnumber
  filter(!is.na(tailnum)) %>% 
  group_by(tailnum) %>% 
  #sum total flights of each tailnum
  summarise(total_flights = n()) %>% 
  arrange(desc(total_flights)) 
  top_flights <- left_join(selected_flights,planes,by="tailnum")
  
  #ANS: Plane N725MQ travelled the most time from NYC in 2013
  
  #Find plane with the greatest number of flights and that had more than 50 seats
  top_flights %>% 
  filter(seats > 50) %>% 
  arrange(desc(total_flights))
```

```
## # A tibble: 3,200 × 10
##    tailnum total_flights  year type       manufacturer model engines seats speed
##    <chr>           <int> <int> <chr>      <chr>        <chr>   <int> <int> <int>
##  1 N328AA            393  1986 Fixed win… BOEING       767-…       2   255    NA
##  2 N338AA            388  1987 Fixed win… BOEING       767-…       2   255    NA
##  3 N327AA            387  1986 Fixed win… BOEING       767-…       2   255    NA
##  4 N335AA            385  1987 Fixed win… BOEING       767-…       2   255    NA
##  5 N323AA            357  1986 Fixed win… BOEING       767-…       2   255    NA
##  6 N319AA            354  1985 Fixed win… BOEING       767-…       2   255    NA
##  7 N336AA            353  1987 Fixed win… BOEING       767-…       2   255    NA
##  8 N329AA            344  1987 Fixed win… BOEING       767-…       2   255    NA
##  9 N789JB            332  2011 Fixed win… AIRBUS       A320…       2   200    NA
## 10 N324AA            328  1986 Fixed win… BOEING       767-…       2   255    NA
## # ℹ 3,190 more rows
## # ℹ 1 more variable: engine <chr>
```

## Problem 4: The `nycflights13` package includes a table (`weather`) that describes the weather during 2013. Use that table to answer the following questions:

```         
-   What is the distribution of temperature (`temp`) in July 2013? Identify any important outliers in terms of the `wind_speed` variable.
-   What is the relationship between `dewp` and `humid`?
-   What is the relationship between `precip` and `visib`?
```


```r
#Q:What is the distribution of temperature (`temp`) in July 2013?
weather %>% 
  filter(year == "2013" & month == 7) %>% 
  ggplot(aes(x=day,y=temp)) + 
  geom_point() + 
  labs(x="Days",y="Temp (F)") + 
  ggtitle("Temperature Distribution in July 2013")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-5-1.png" width="648" style="display: block; margin: auto;" />

```r
#Q:Identify any important outliers in terms of the `wind_speed` variable.
weather %>% 
 filter(year == "2013" & month == 7) %>% 
 ggplot(aes(x=day,y=wind_speed)) + 
  geom_point() +
  labs(x="Days",y="Wind Speed") + 
  ggtitle("Wind Speed in July 2013")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-5-2.png" width="648" style="display: block; margin: auto;" />

```r
#Q:What is the relationship between `dewp` and `humid`?
weather %>% 
 ggplot(aes(x=dewp,y=humid)) + 
  geom_point() +
  labs(x="Dew Point(F)",y="Humidity") + 
  ggtitle("Relationship of Dew Point and Humidity in July 2013")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-5-3.png" width="648" style="display: block; margin: auto;" />

```r
#Q:What is the relationship between `precip` and `visib`?
weather %>% 
  ggplot(aes(x=precip,y=visib)) + 
  geom_point() +
  labs(x="Precipitation",y="Visibility") + 
  ggtitle("Relationship of Precipitation and Visibility in July 2013")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-5-4.png" width="648" style="display: block; margin: auto;" />

## Problem 5: Use the `flights` and `planes` tables to answer the following questions:

```         
-   How many planes have a missing date of manufacture?
-   What are the five most common manufacturers?
-   Has the distribution of manufacturer changed over time as reflected by the airplanes flying from NYC in 2013? (Hint: you may need to use case_when() to recode the manufacturer name and collapse rare vendors into a category called Other.)
```


```r
#Q:How many planes have a missing date of manufacture?
planes %>% 
  filter(is.na(year)) %>% 
  summarise(count=n())
```

```
## # A tibble: 1 × 1
##   count
##   <int>
## 1    70
```

```r
  #ANS: 70 planes
  
#Q:What are the five most common manufacturers?
planes %>% 
  group_by(manufacturer) %>% 
  summarise(count=n()) %>% 
  arrange(desc(count))
```

```
## # A tibble: 35 × 2
##    manufacturer                  count
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
  #ANS: 
 #1 BOEING                         1630
 #2 AIRBUS INDUSTRIE                400
 #3 BOMBARDIER INC                  368
 #4 AIRBUS                          336
 #5 EMBRAER                         299
  
#Q:Has the distribution of manufacturer changed over time as reflected by the airplanes flying from NYC in 2013?
flights %>% 
  filter(origin %in% c("JFK","EWR","LGA") & year ==2013)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # ℹ 336,766 more rows
## # ℹ 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
  joined_table <- left_join(flights,planes,by="tailnum")

joined_table %>% 
  group_by(month,manufacturer) %>% 
  summarise(sum_monthly=n()) %>% 
  #classify munufacturer
  mutate(new_manufacturer = case_when(sum_monthly<1000 ~ 'Other',TRUE ~ manufacturer)) %>% 
  arrange(desc(sum_monthly)) %>% 
  ggplot(aes(x=month,y=sum_monthly,color=new_manufacturer))+
  geom_line()+
  labs(x="Month",y="% of Total Flights") + 
  ggtitle("Distribution of Manufacturers over time in 2013")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-6-1.png" width="648" style="display: block; margin: auto;" />

## Problem 6: Use the `flights` and `planes` tables to answer the following questions:

```         
-   What is the oldest plane (specified by the tailnum variable) that flew from New York City airports in 2013?
-   How many airplanes that flew from New York City are included in the planes table?
```


```r
#Q:What is the oldest plane (specified by the tailnum variable) that flew from New York City airports in 2013?
flights %>% 
  filter(origin %in% c("JFK","EWR","LGA") & year ==2013) %>% 
  select(tailnum) %>%
  distinct() %>%
  left_join(planes, by = "tailnum") %>%
  arrange(year) %>%
  slice_head(n = 1)
```

```
## # A tibble: 1 × 9
##   tailnum  year type               manufacturer model engines seats speed engine
##   <chr>   <int> <chr>              <chr>        <chr>   <int> <int> <int> <chr> 
## 1 N381AA   1956 Fixed wing multi … DOUGLAS      DC-7…       4   102   232 Recip…
```

```r
  #ANS: Tailnum N381AA in year 1956

#Q:How many airplanes that flew from New York City are included in the planes table?
flights %>% 
  filter(origin %in% c("JFK","EWR","LGA") & year ==2013) %>% 
  group_by(tailnum) %>% 
  summarise(count = n())%>% 
  summarise(count_final=n())
```

```
## # A tibble: 1 × 1
##   count_final
##         <int>
## 1        4044
```

## Problem 7: Use the `nycflights13` to answer the following questions:

```         
-   What is the median arrival delay on a month-by-month basis in each airport?
-   For each airline, plot the median arrival delay for each month and origin airport.
```


```r
#Q:What is the median arrival delay on a month-by-month basis in each airport?
flights %>% 
  group_by(month,dest) %>% 
  summarise(median_arr_delay=median(arr_delay,na.rm=TRUE)) 
```

```
## # A tibble: 1,113 × 3
## # Groups:   month [12]
##    month dest  median_arr_delay
##    <int> <chr>            <dbl>
##  1     1 ALB                6  
##  2     1 ATL               -2  
##  3     1 AUS               -2  
##  4     1 AVL               23.5
##  5     1 BDL              -10  
##  6     1 BHM              -11  
##  7     1 BNA                1  
##  8     1 BOS              -10  
##  9     1 BQN               -5  
## 10     1 BTV               -6  
## # ℹ 1,103 more rows
```

```r
#Q:For each airline, plot the median arrival delay for each month and origin airport.
flights %>% 
  group_by(carrier,month,origin) %>%
  summarise(median_arr_delay=median(arr_delay,na.rm=TRUE)) %>% 
  ggplot(aes(x=month,y=median_arr_delay,color=origin))+
  geom_point()+
  labs(x="Month",y="Median Arrival")+
  ggtitle("Median Arrival for each carrier in each airport by month") +
  facet_wrap(~carrier)
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-8-1.png" width="648" style="display: block; margin: auto;" />

## Problem 8: Let's take a closer look at what carriers service the route to San Francisco International (SFO). Join the `flights` and `airlines` tables and count which airlines flew the most to SFO. Produce a new dataframe, `fly_into_sfo` that contains three variables: the `name` of the airline, e.g., `United Air Lines Inc.` not `UA`, the count (number) of times it flew to SFO, and the `percent` of the trips that that particular airline flew to SFO.


```r
fly_into_sfo <- flights %>% 
  #select only destination is SFO
  filter(dest=="SFO") %>% 
  left_join(airlines,by="carrier") %>% 
  group_by(name) %>% 
  summarise(count=n()) %>% 
  #find percent of trip that fly to SFO
  mutate(percent=(count/sum(count))*100) %>% 
  arrange(desc(percent)) 

fly_into_sfo %>% 
  # sort 'name' of airline by the numbers it times to flew to SFO
  mutate(name = fct_reorder(name, count)) %>% 
  ggplot() +
  aes(x = count, y = name) +
  
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

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-9-1.png" width="648" style="display: block; margin: auto;" />

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

<img src="/blogs/HW 2_files/figure-html/ggplot-flights-toSFO-1.png" width="648" style="display: block; margin: auto;" />

## Problem 9: Let's take a look at cancellations of flights to SFO. We create a new dataframe `cancellations` as follows


```r
cancellations <- flights %>% 
  
  # just filter for destination == 'SFO'
  filter(dest == 'SFO') %>% 
  
  # a cancelled flight is one with no `dep_time` 
  filter(is.na(dep_time))

# First, I will work with data to find #of cancelled flights by month, carrier, and origin. I will use group_by function to group these three parameters and then summarise cancelled flights for each carrier and origin by month.

#Secondly, It is to visualise the graph. I will use ggplot with month as x-axis and #of Cancelled flights in y axis. Then I will add geom for the bar chart. Lastly, I will use funtion facet_wrap(~carrier) to split each carrier into each graph.
```

I want you to think how we would organise our data manipulation to create the following plot. No need to write the code, just explain in words how you would go about it.

![](images/sfo-cancellations.png)

## Problem 10: On your own -- Hollywood Age Gap

The website https://hollywoodagegap.com is a record of *THE AGE DIFFERENCE IN YEARS BETWEEN MOVIE LOVE INTERESTS*. This is an informational site showing the age gap between movie love interests and the data follows certain rules:

-   The two (or more) actors play actual love interests (not just friends, coworkers, or some other non-romantic type of relationship)
-   The youngest of the two actors is at least 17 years old
-   No animated characters

The age gaps dataset includes "gender" columns, which always contain the values "man" or "woman". These values appear to indicate how the characters in each film identify and some of these values do not match how the actor identifies. We apologize if any characters are misgendered in the data!

The following is a data dictionary of the variables used

| variable            | class     | description                                                                                             |
|:---------------|:---------------|:--------------------------------------|
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

#How is `age_difference` distributed? 
age_gaps %>% 
  group_by(age_difference) %>% 
  summarise(count=n()) %>% 
  ggplot(aes(x=age_difference,y=count)) + 
  geom_point()+
  ggtitle("Age Difference Distribution")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-12-1.png" width="648" style="display: block; margin: auto;" />

```r
#What's the 'typical' `age_difference` in movies?
age_gaps %>% 
  group_by(age_difference) %>% 
  summarise(count = n()) %>% 
  arrange(desc(count))
```

```
## # A tibble: 46 × 2
##    age_difference count
##             <dbl> <int>
##  1              2    85
##  2              3    85
##  3              1    77
##  4              5    71
##  5              7    71
##  6              4    66
##  7              8    59
##  8              9    52
##  9             11    51
## 10              6    50
## # ℹ 36 more rows
```

```r
  #ANS: The most age difference is 2,3 years difference, which each has 85 movies.
  
  #The `half plus seven\` rule. Large age disparities in relationships carry certain stigmas. One popular rule of thumb is the [half-your-age-plus-seven](https://en.wikipedia.org/wiki/Age_disparity_in_sexual_relationships#The_.22half-your-age-plus-seven.22_rule) rule. This rule states you should never date anyone under half your age plus seven, establishing a minimum boundary on whom one can date. In order for a dating relationship to be acceptable under this rule, your partner's age must be:
  #How frequently does this rule apply in this dataset?
age_gaps %>% 
  mutate(lower = actor_1_age / 2 + 7,
  upper = (actor_1_age - 7) * 2,
  acceptable_age = if_else(actor_2_age >= lower & actor_2_age <=        upper, TRUE, FALSE)) %>% 
  summarise(prop = mean(acceptable_age)*n())
```

```
## # A tibble: 1 × 1
##    prop
##   <dbl>
## 1   829
```

```r
#Which actors/ actresses have the greatest number of love interests in this dataset?
age_gaps %>% 
    group_by(movie_name) %>% 
    summarise(num = n_distinct(couple_number)) %>%
    filter(num == max(num))
```

```
## # A tibble: 1 × 2
##   movie_name      num
##   <chr>         <int>
## 1 Love Actually     7
```

```r
#Which actors/ actresses have the greatest number of love interests in this dataset?
age_gaps %>% 
    group_by(actor_1_name) %>% 
    summarise(num = n_distinct(couple_number)) %>%
    arrange(desc(num))
```

```
## # A tibble: 567 × 2
##    actor_1_name      num
##    <chr>           <int>
##  1 Pierce Brosnan      4
##  2 Roger Moore         4
##  3 Bradley Cooper      3
##  4 Christian Bale      3
##  5 Colin Firth         3
##  6 Dermot Mulroney     3
##  7 Heath Ledger        3
##  8 Jesse Eisenberg     3
##  9 John Cusack         3
## 10 Julia Roberts       3
## # ℹ 557 more rows
```

```r
age_gaps %>% 
    group_by(actor_2_name) %>% 
    summarise(num = n_distinct(couple_number)) %>%
    arrange(desc(num))
```

```
## # A tibble: 647 × 2
##    actor_2_name           num
##    <chr>                <int>
##  1 Diane Keaton             4
##  2 Keira Knightley          4
##  3 Ali Wong                 3
##  4 Amy Adams                3
##  5 Angelina Jolie           3
##  6 Annette Bening           3
##  7 Blake Lively             3
##  8 Cameron Diaz             3
##  9 Cate Blanchett           3
## 10 Helena Bonham Carter     3
## # ℹ 637 more rows
```

```r
#Is the mean/median age difference staying constant over the years (1935 - 2022)?
age_gaps %>%
  group_by(release_year) %>%
  summarise(mean_age_difference = mean(age_difference), median_age_difference = median(age_difference)) %>%
  ggplot(aes(x = release_year, y = mean_age_difference)) +
  geom_line(aes(y = mean_age_difference), color = "blue") +
  geom_line(aes(y = median_age_difference), color = "red") +
  labs(x = "Release Year", y = "Age Difference")
```

<img src="/blogs/HW 2_files/figure-html/unnamed-chunk-12-2.png" width="648" style="display: block; margin: auto;" />

```r
#How frequently does Hollywood depict same-gender love interests?
age_gaps %>%
  filter(character_1_gender == character_2_gender) %>% 
  summarise(count = n())
```

```
## # A tibble: 1 × 1
##   count
##   <int>
## 1    23
```

```r
#ANS: 23 movies
```

How would you explore this data set? Here are some ideas of tables/ graphs to help you with your analysis

-   How is `age_difference` distributed? What's the 'typical' `age_difference` in movies?

-   The `half plus seven\` rule. Large age disparities in relationships carry certain stigmas. One popular rule of thumb is the [half-your-age-plus-seven](https://en.wikipedia.org/wiki/Age_disparity_in_sexual_relationships#The_.22half-your-age-plus-seven.22_rule) rule. This rule states you should never date anyone under half your age plus seven, establishing a minimum boundary on whom one can date. In order for a dating relationship to be acceptable under this rule, your partner's age must be:

`$$\frac{\text{Your age}}{2} + 7 < \text{Partner Age} < (\text{Your age} - 7) * 2$$` How frequently does this rule apply in this dataset?

-   Which movie has the greatest number of love interests?
-   Which actors/ actresses have the greatest number of love interests in this dataset?
-   Is the mean/median age difference staying constant over the years (1935 - 2022)?
-   How frequently does Hollywood depict same-gender love interests?

