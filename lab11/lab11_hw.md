---
title: "Lab 11 Homework"
author: "Ibrahim Rizk"
date: "2021-02-23"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(paletteer)
library(viridis)
library(ggthemes)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  

```r
skimr::skim(gapminder)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |gapminder |
|Number of rows           |1704      |
|Number of columns        |6         |
|_______________________  |          |
|Column type frequency:   |          |
|factor                   |2         |
|numeric                  |4         |
|________________________ |          |
|Group variables          |None      |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts                             |
|:-------------|---------:|-------------:|:-------|--------:|:--------------------------------------|
|country       |         0|             1|FALSE   |      142|Afg: 12, Alb: 12, Alg: 12, Ang: 12     |
|continent     |         0|             1|FALSE   |        5|Afr: 624, Asi: 396, Eur: 360, Ame: 300 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|        mean|           sd|       p0|        p25|        p50|         p75|         p100|hist  |
|:-------------|---------:|-------------:|-----------:|------------:|--------:|----------:|----------:|-----------:|------------:|:-----|
|year          |         0|             1|     1979.50|        17.27|  1952.00|    1965.75|    1979.50|     1993.25|       2007.0|▇▅▅▅▇ |
|lifeExp       |         0|             1|       59.47|        12.92|    23.60|      48.20|      60.71|       70.85|         82.6|▁▆▇▇▇ |
|pop           |         0|             1| 29601212.32| 106157896.74| 60011.00| 2793664.00| 7023595.50| 19585221.75| 1318683096.0|▇▁▁▁▁ |
|gdpPercap     |         0|             1|     7215.33|      9857.45|   241.17|    1202.06|    3531.85|     9325.46|     113523.1|▇▁▁▁▁ |

```r
naniar::miss_var_summary(gapminder)
```

```
## # A tibble: 6 x 3
##   variable  n_miss pct_miss
##   <chr>      <int>    <dbl>
## 1 country        0        0
## 2 continent      0        0
## 3 year           0        0
## 4 lifeExp        0        0
## 5 pop            0        0
## 6 gdpPercap      0        0
```

```r
gapminder <- clean_names(gapminder)
```


**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

```r
gapminder %>% 
  filter(year %in% (1952:2007)) %>% 
  summarise()
```

```
## # A tibble: 1 x 0
```


**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder %>% 
  filter(year == '1952') %>% 
  ggplot(aes(x = continent, y = life_exp)) + geom_col() + labs(title = 'Life expectancy in 1952 by continent')
```

![](lab11_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
gapminder %>% 
  filter(year == '2007') %>% 
  ggplot(aes(x = continent, y = life_exp)) + geom_col() + labs(title = 'Life expectancy in 2007 by continent')
```

![](lab11_hw_files/figure-html/unnamed-chunk-5-2.png)<!-- -->


**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder %>% 
  group_by(continent) %>% 
  summarize(max_LE = max(life_exp), min_LE = min(life_exp), delta_LE = max_LE - min_LE)
```

```
## # A tibble: 5 x 4
##   continent max_LE min_LE delta_LE
## * <fct>      <dbl>  <dbl>    <dbl>
## 1 Africa      76.4   23.6     52.8
## 2 Americas    80.7   37.6     43.1
## 3 Asia        82.6   28.8     53.8
## 4 Europe      81.8   43.6     38.2
## 5 Oceania     81.2   69.1     12.1
```


**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder %>% 
  group_by(continent, life_exp) %>% 
  filter(year %in% (1952:2007)) %>% 
  ggplot(aes(x = year, y = life_exp, color = continent)) + geom_jitter() + theme(axis.text.x = element_text(angle = 30, hjust = 1)) + geom_smooth(method = lm, se = T) + labs(title = 'Change in life expectancy between 1952-2007 by continent', x = 'year', y = 'life expectancy')
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder %>% 
  ggplot(aes(x = gdp_percap, y = life_exp, color = continent)) + scale_x_log10() + geom_jitter()   + geom_smooth(method = lm, se = T) + theme_clean() + labs(title = 'Life expectancy and Per Capita GDP', x = 'GDP per Capita (scaled)', y = 'Life expectancy in years')
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
# Life expectancy and Per Capita GDP appear to be positively correlated
```



**7. Which countries have had the largest population growth since 1952?**


```r
gapminder %>% 
  filter(year %in% (1952:2021)) %>% 
  group_by(country) %>% 
  summarize(min_P = min(pop), max_P = max(pop), delta_P = max_P - min_P) %>% 
  arrange(desc(delta_P)) %>% 
  top_n(5, delta_P)
```

```
## # A tibble: 5 x 4
##   country           min_P      max_P   delta_P
##   <fct>             <int>      <int>     <int>
## 1 China         556263527 1318683096 762419569
## 2 India         372000000 1110396331 738396331
## 3 United States 157553000  301139947 143586947
## 4 Indonesia      82052000  223547000 141495000
## 5 Brazil         56602560  190010647 133408087
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder %>% 
  filter(year %in% (1952:2021)) %>% 
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>% 
  ggplot(aes(x = year, y = pop, color = country)) + geom_jitter() + geom_smooth(method = lm, se = T)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder %>% 
  filter(year %in% (1952:2021)) %>% 
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>% 
  ggplot(aes(x = year, y = gdp_percap, color = country)) + geom_jitter() + geom_smooth(method = lm, se = T) + labs(title = 'Per-Capita GDP over year', x = 'Year', y = 'Per-Capita GDP')
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


**10. Make one plot of your choice that uses faceting!**

```r
gapminder %>% 
  filter(year %in% (1952:2021)) %>% 
  filter(country == 'China' | country == 'India' | country == 'United States' | country == 'Indonesia' | country == 'Brazil') %>% 
  ggplot(aes(x = year, y = pop)) + geom_point() + facet_grid(~country) + geom_smooth(method = lm, se = T, color = 'blue') + labs(title = 'Population over year', x = 'year (1952-2021)', y = 'Population') + scale_x_discrete(breaks = c('1960', '1970', '1980', '1990', '2000', '2010', '2020'))
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
