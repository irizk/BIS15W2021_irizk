---
title: "Lab 8 Homework"
author: "Ibrahim Rizk"
date: "2021-03-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydney <- readr::read_csv('data/sydneybeaches.csv')
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   BeachId = col_double(),
##   Region = col_character(),
##   Council = col_character(),
##   Site = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double(),
##   Date = col_character(),
##   `Enterococci (cfu/100ml)` = col_double()
## )
```

```r
skimr::skim(sydney)
```


Table: Data summary

|                         |       |
|:------------------------|:------|
|Name                     |sydney |
|Number of rows           |3690   |
|Number of columns        |8      |
|_______________________  |       |
|Column type frequency:   |       |
|character                |4      |
|numeric                  |4      |
|________________________ |       |
|Group variables          |None   |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Region        |         0|             1|  25|  25|     0|        1|          0|
|Council       |         0|             1|  16|  16|     0|        2|          0|
|Site          |         0|             1|  11|  23|     0|       11|          0|
|Date          |         0|             1|  10|  10|     0|      344|          0|


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|   mean|     sd|     p0|    p25|    p50|    p75|    p100|hist  |
|:-----------------------|---------:|-------------:|------:|------:|------:|------:|------:|------:|-------:|:-----|
|BeachId                 |         0|          1.00|  25.87|   2.08|  22.00|  24.00|  26.00|  27.40|   29.00|▆▃▇▇▆ |
|Longitude               |         0|          1.00| 151.26|   0.01| 151.25| 151.26| 151.26| 151.27|  151.28|▅▇▂▆▂ |
|Latitude                |         0|          1.00| -33.93|   0.03| -33.98| -33.95| -33.92| -33.90|  -33.89|▆▇▁▇▇ |
|Enterococci (cfu/100ml) |        29|          0.99|  33.92| 154.92|   0.00|   1.00|   5.00|  17.00| 4900.00|▇▁▁▁▁ |

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk
```

```r
here::here()
```

```
## [1] "D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk"
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   BeachId = col_double(),
##   Region = col_character(),
##   Council = col_character(),
##   Site = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double(),
##   Date = col_character(),
##   `Enterococci (cfu/100ml)` = col_double()
## )
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

# These data are not tidy. Variables occupy multiple columns and observations do not have their own row

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- select(sydneybeaches, site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,680 more rows
```



4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(-enterococci_cfu_100ml, names_from = 'date',
             values_from = c(enterococci_cfu_100ml, contains('Beach')))
sydneybeaches_wide
```

```
## # A tibble: 11 x 345
##    site         `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013`
##    <chr>               <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1 Clovelly Be~           19            3            2           13            8
##  2 Coogee Beach           15            4           17           18           22
##  3 Gordons Bay~           NA           NA           NA           NA           NA
##  4 Little Bay ~            9            3           72            1           44
##  5 Malabar Bea~            2            4          390           15           13
##  6 Maroubra Be~            1            1           20            2           11
##  7 South Marou~            1            0           33            2           13
##  8 South Marou~           12            2          110           13          100
##  9 Bondi Beach             3            1            2            1            6
## 10 Bronte Beach            4            2           38            3           25
## 11 Tamarama Be~            1            0            7           22           23
## # ... with 339 more variables: 05/02/2013 <dbl>, 11/02/2013 <dbl>,
## #   23/02/2013 <dbl>, 07/03/2013 <dbl>, 25/03/2013 <dbl>, 02/04/2013 <dbl>,
## #   12/04/2013 <dbl>, 18/04/2013 <dbl>, 24/04/2013 <dbl>, 01/05/2013 <dbl>,
## #   20/05/2013 <dbl>, 31/05/2013 <dbl>, 06/06/2013 <dbl>, 12/06/2013 <dbl>,
## #   24/06/2013 <dbl>, 06/07/2013 <dbl>, 18/07/2013 <dbl>, 24/07/2013 <dbl>,
## #   08/08/2013 <dbl>, 22/08/2013 <dbl>, 29/08/2013 <dbl>, 24/01/2013 <dbl>,
## #   17/02/2013 <dbl>, 01/03/2013 <dbl>, 13/03/2013 <dbl>, 19/03/2013 <dbl>,
## #   06/04/2013 <dbl>, 07/05/2013 <dbl>, 14/05/2013 <dbl>, 25/05/2013 <dbl>,
## #   17/06/2013 <dbl>, 30/06/2013 <dbl>, 12/07/2013 <dbl>, 30/07/2013 <dbl>,
## #   14/08/2013 <dbl>, 16/08/2013 <dbl>, 02/09/2013 <dbl>, 28/09/2013 <dbl>,
## #   04/10/2013 <dbl>, 12/10/2013 <dbl>, 28/10/2013 <dbl>, 05/11/2013 <dbl>,
## #   29/11/2013 <dbl>, 07/12/2013 <dbl>, 10/09/2013 <dbl>, 16/09/2013 <dbl>,
## #   22/09/2013 <dbl>, 20/10/2013 <dbl>, 13/11/2013 <dbl>, 21/11/2013 <dbl>,
## #   23/12/2013 <dbl>, 13/12/2013 <dbl>, 31/12/2013 <dbl>, 08/10/2014 <dbl>,
## #   16/10/2014 <dbl>, 25/10/2014 <dbl>, 02/11/2014 <dbl>, 10/11/2014 <dbl>,
## #   18/11/2014 <dbl>, 13/12/2014 <dbl>, 19/12/2014 <dbl>, 16/01/2014 <dbl>,
## #   24/01/2014 <dbl>, 09/02/2014 <dbl>, 25/02/2014 <dbl>, 05/03/2014 <dbl>,
## #   06/04/2014 <dbl>, 06/05/2014 <dbl>, 16/05/2014 <dbl>, 22/05/2014 <dbl>,
## #   10/06/2014 <dbl>, 26/06/2014 <dbl>, 08/07/2014 <dbl>, 14/07/2014 <dbl>,
## #   24/07/2014 <dbl>, 05/08/2014 <dbl>, 21/08/2014 <dbl>, 21/10/2014 <dbl>,
## #   26/11/2014 <dbl>, 04/12/2014 <dbl>, 12/09/2014 <dbl>, 18/09/2014 <dbl>,
## #   24/09/2014 <dbl>, 28/12/2014 <dbl>, 08/01/2014 <dbl>, 01/02/2014 <dbl>,
## #   17/02/2014 <dbl>, 13/03/2014 <dbl>, 21/03/2014 <dbl>, 29/03/2014 <dbl>,
## #   22/04/2014 <dbl>, 14/04/2014 <dbl>, 30/04/2014 <dbl>, 12/05/2014 <dbl>,
## #   28/05/2014 <dbl>, 03/06/2014 <dbl>, 19/06/2014 <dbl>, 03/07/2014 <dbl>,
## #   18/07/2014 <dbl>, 01/08/2014 <dbl>, ...
```

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_longer <- sydneybeaches_wide %>%
  pivot_longer(cols = 'site', names_to = 'date', values_to = 'site')
sydneybeaches_longer
```

```
## # A tibble: 11 x 346
##    `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013` `05/02/2013`
##           <dbl>        <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1           19            3            2           13            8            7
##  2           15            4           17           18           22            2
##  3           NA           NA           NA           NA           NA           NA
##  4            9            3           72            1           44            7
##  5            2            4          390           15           13           13
##  6            1            1           20            2           11            0
##  7            1            0           33            2           13            0
##  8           12            2          110           13          100          630
##  9            3            1            2            1            6            5
## 10            4            2           38            3           25            2
## 11            1            0            7           22           23            3
## # ... with 340 more variables: 11/02/2013 <dbl>, 23/02/2013 <dbl>,
## #   07/03/2013 <dbl>, 25/03/2013 <dbl>, 02/04/2013 <dbl>, 12/04/2013 <dbl>,
## #   18/04/2013 <dbl>, 24/04/2013 <dbl>, 01/05/2013 <dbl>, 20/05/2013 <dbl>,
## #   31/05/2013 <dbl>, 06/06/2013 <dbl>, 12/06/2013 <dbl>, 24/06/2013 <dbl>,
## #   06/07/2013 <dbl>, 18/07/2013 <dbl>, 24/07/2013 <dbl>, 08/08/2013 <dbl>,
## #   22/08/2013 <dbl>, 29/08/2013 <dbl>, 24/01/2013 <dbl>, 17/02/2013 <dbl>,
## #   01/03/2013 <dbl>, 13/03/2013 <dbl>, 19/03/2013 <dbl>, 06/04/2013 <dbl>,
## #   07/05/2013 <dbl>, 14/05/2013 <dbl>, 25/05/2013 <dbl>, 17/06/2013 <dbl>,
## #   30/06/2013 <dbl>, 12/07/2013 <dbl>, 30/07/2013 <dbl>, 14/08/2013 <dbl>,
## #   16/08/2013 <dbl>, 02/09/2013 <dbl>, 28/09/2013 <dbl>, 04/10/2013 <dbl>,
## #   12/10/2013 <dbl>, 28/10/2013 <dbl>, 05/11/2013 <dbl>, 29/11/2013 <dbl>,
## #   07/12/2013 <dbl>, 10/09/2013 <dbl>, 16/09/2013 <dbl>, 22/09/2013 <dbl>,
## #   20/10/2013 <dbl>, 13/11/2013 <dbl>, 21/11/2013 <dbl>, 23/12/2013 <dbl>,
## #   13/12/2013 <dbl>, 31/12/2013 <dbl>, 08/10/2014 <dbl>, 16/10/2014 <dbl>,
## #   25/10/2014 <dbl>, 02/11/2014 <dbl>, 10/11/2014 <dbl>, 18/11/2014 <dbl>,
## #   13/12/2014 <dbl>, 19/12/2014 <dbl>, 16/01/2014 <dbl>, 24/01/2014 <dbl>,
## #   09/02/2014 <dbl>, 25/02/2014 <dbl>, 05/03/2014 <dbl>, 06/04/2014 <dbl>,
## #   06/05/2014 <dbl>, 16/05/2014 <dbl>, 22/05/2014 <dbl>, 10/06/2014 <dbl>,
## #   26/06/2014 <dbl>, 08/07/2014 <dbl>, 14/07/2014 <dbl>, 24/07/2014 <dbl>,
## #   05/08/2014 <dbl>, 21/08/2014 <dbl>, 21/10/2014 <dbl>, 26/11/2014 <dbl>,
## #   04/12/2014 <dbl>, 12/09/2014 <dbl>, 18/09/2014 <dbl>, 24/09/2014 <dbl>,
## #   28/12/2014 <dbl>, 08/01/2014 <dbl>, 01/02/2014 <dbl>, 17/02/2014 <dbl>,
## #   13/03/2014 <dbl>, 21/03/2014 <dbl>, 29/03/2014 <dbl>, 22/04/2014 <dbl>,
## #   14/04/2014 <dbl>, 30/04/2014 <dbl>, 12/05/2014 <dbl>, 28/05/2014 <dbl>,
## #   03/06/2014 <dbl>, 19/06/2014 <dbl>, 03/07/2014 <dbl>, 18/07/2014 <dbl>,
## #   01/08/2014 <dbl>, 11/08/2014 <dbl>, ...
```
</div>


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sep_dates_sydneybeaches_long <- sydneybeaches_long %>%
  separate(date, into = c('day', 'month', 'year'), sep = '/')
sep_dates_sydneybeaches_long
```

```
## # A tibble: 3,690 x 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ... with 3,680 more rows
```



7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
avg_enterococci <- sep_dates_sydneybeaches_long %>%
  group_by(site, year) %>%
  summarise(mean_enterococci_cfu_100ml = mean(enterococci_cfu_100ml, na.rm = T), .groups = 'keep')
avg_enterococci
```

```
## # A tibble: 66 x 3
## # Groups:   site, year [66]
##    site         year  mean_enterococci_cfu_100ml
##    <chr>        <chr>                      <dbl>
##  1 Bondi Beach  2013                        32.2
##  2 Bondi Beach  2014                        11.1
##  3 Bondi Beach  2015                        14.3
##  4 Bondi Beach  2016                        19.4
##  5 Bondi Beach  2017                        13.2
##  6 Bondi Beach  2018                        22.9
##  7 Bronte Beach 2013                        26.8
##  8 Bronte Beach 2014                        17.5
##  9 Bronte Beach 2015                        23.6
## 10 Bronte Beach 2016                        61.3
## # ... with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.


```r
avg_enterococci %>%
  pivot_wider(names_from = 'site', values_from = mean_enterococci_cfu_100ml)
```

```
## # A tibble: 6 x 12
## # Groups:   year [6]
##   year  `Bondi Beach` `Bronte Beach` `Clovelly Beach` `Coogee Beach`
##   <chr>         <dbl>          <dbl>            <dbl>          <dbl>
## 1 2013           32.2           26.8             9.28           39.7
## 2 2014           11.1           17.5            13.8            52.6
## 3 2015           14.3           23.6             8.82           40.3
## 4 2016           19.4           61.3            11.3            59.5
## 5 2017           13.2           16.8             7.93           20.7
## 6 2018           22.9           43.4            10.6            21.6
## # ... with 7 more variables: Gordons Bay (East) <dbl>, Little Bay Beach <dbl>,
## #   Malabar Beach <dbl>, Maroubra Beach <dbl>, South Maroubra Beach <dbl>,
## #   South Maroubra Rockpool <dbl>, Tamarama Beach <dbl>
```



9. What was the most polluted beach in 2018?

```r
avg_enterococci %>%
  pivot_wider(names_from = 'site', values_from = mean_enterococci_cfu_100ml) %>%
  filter(year == '2018')
```

```
## # A tibble: 1 x 12
## # Groups:   year [1]
##   year  `Bondi Beach` `Bronte Beach` `Clovelly Beach` `Coogee Beach`
##   <chr>         <dbl>          <dbl>            <dbl>          <dbl>
## 1 2018           22.9           43.4             10.6           21.6
## # ... with 7 more variables: Gordons Bay (East) <dbl>, Little Bay Beach <dbl>,
## #   Malabar Beach <dbl>, Maroubra Beach <dbl>, South Maroubra Beach <dbl>,
## #   South Maroubra Rockpool <dbl>, Tamarama Beach <dbl>
```

```r
#South Maroubra Rockpool was the most polluted beach in 2018
```


10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
