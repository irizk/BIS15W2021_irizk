---
title: "Midterm 1"
author: "Rizk	Ibrahim"
date: "2021-02-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library(janitor)
```

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">  

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

_R is a computer language that allows for statistical analysis of data. Rstudio is a GUI for R, which increases R's ease of use. Github is an online hosting service for code, and streamlines group access to a program or code. R Markdown (.rmd files) display code written in R in a cleaner format. Knitting an R Markdown allows for the creation of data reports and documents in PDF, HTML, or Word formats. These can be stored on Github for anyone to knit. Github is what connects the individual coding contributions of R users and allows code to be shared, reused, and optimized._

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**  

_Vectors, data matrices, and data frames. In contrast to vectors and data matrices, data frames are neater, tidier and the data is treated and able to be manipulated in a more fluid and efficient way. In addition, data frames can be stored as .csv files and opened in excel._

**I suggest not putting text into code chunks. One of the benefits of markdown is allows you to produce clean, consistently formatted reports. If you put test into code chunks you lose these benefits.**  

</div>

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**  

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- select_all(elephants, tolower)
elephants
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <chr>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # … with 278 more rows
```


```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

**5. (2 points) How many male and female elephants are represented in the data?**

```r
tabyl(elephants$sex)
```

```
##  elephants$sex   n   percent
##              F 150 0.5208333
##              M 138 0.4791667
```

**6. (2 points) What is the average age all elephants in the data?**

```r
mean(elephants$age)
```

```
## [1] 10.97132
```

**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>%
  group_by(sex) %>%
  summarize(avg_age = mean(age))
```

```
## # A tibble: 2 x 2
##   sex   avg_age
## * <fct>   <dbl>
## 1 F       12.8 
## 2 M        8.95
```


```r
elephants %>%
  group_by(sex) %>%
  summarize(avg_height = mean(height))
```

```
## # A tibble: 2 x 2
##   sex   avg_height
## * <fct>      <dbl>
## 1 F           190.
## 2 M           185.
```

**Remember, you can combine these to make a cleaner output.**

```r
elephants %>%
  group_by(sex) %>%
  summarize(avg_age = mean(age),
            avg_height = mean(height)
            )
```

```
## # A tibble: 2 x 3
##   sex   avg_age avg_height
## * <fct>   <dbl>      <dbl>
## 1 F       12.8        190.
## 2 M        8.95       185.
```

**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**


```r
elephants %>%
  group_by(sex) %>%
  filter(age > 25) %>%
  summarize(avg_height = mean(height),
            min_height = min(height),
            max_height = max(height)
            )
```

```
## # A tibble: 2 x 4
##   sex   avg_height min_height max_height
##   <fct>      <dbl>      <dbl>      <dbl>
## 1 F           233.       206.       278.
## 2 M           273.       237.       304.
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**  

```r
vertebrate <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```


```r
vertebrate$HuntCat <- as.factor(vertebrate$HuntCat)
vertebrate$LandUse <- as.factor(vertebrate$LandUse)
```


```r
class(vertebrate$HuntCat)
```

```
## [1] "factor"
```

```r
class(vertebrate$LandUse)
```

```
## [1] "factor"
```


```r
names(vertebrate)
```

```
##  [1] "TransectID"              "Distance"               
##  [3] "HuntCat"                 "NumHouseholds"          
##  [5] "LandUse"                 "Veg_Rich"               
##  [7] "Veg_Stems"               "Veg_liana"              
##  [9] "Veg_DBH"                 "Veg_Canopy"             
## [11] "Veg_Understory"          "RA_Apes"                
## [13] "RA_Birds"                "RA_Elephant"            
## [15] "RA_Monkeys"              "RA_Rodent"              
## [17] "RA_Ungulate"             "Rich_AllSpecies"        
## [19] "Evenness_AllSpecies"     "Diversity_AllSpecies"   
## [21] "Rich_BirdSpecies"        "Evenness_BirdSpecies"   
## [23] "Diversity_BirdSpecies"   "Rich_MammalSpecies"     
## [25] "Evenness_MammalSpecies"  "Diversity_MammalSpecies"
```

```r
str(vertebrate)
```

```
## spec_tbl_df [24 × 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ TransectID             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ Distance               : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ HuntCat                : Factor w/ 3 levels "High","Moderate",..: 2 3 3 3 3 3 3 3 1 1 ...
##  $ NumHouseholds          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ LandUse                : Factor w/ 3 levels "Logging","Neither",..: 3 3 3 1 3 3 3 1 2 1 ...
##  $ Veg_Rich               : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ Veg_Stems              : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ Veg_liana              : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ Veg_DBH                : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ Veg_Canopy             : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ Veg_Understory         : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ RA_Apes                : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ RA_Birds               : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ RA_Elephant            : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ RA_Monkeys             : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ RA_Rodent              : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ RA_Ungulate            : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ Rich_AllSpecies        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ Evenness_AllSpecies    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ Diversity_AllSpecies   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ Rich_BirdSpecies       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ Evenness_BirdSpecies   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ Diversity_BirdSpecies  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ Rich_MammalSpecies     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ Evenness_MammalSpecies : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ Diversity_MammalSpecies: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
```

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**  

```r
x <- select(vertebrate, HuntCat, Diversity_BirdSpecies, Diversity_MammalSpecies)
```

**These are already data frames, so there is no need to convert them by using data.frame()**

```r
High <- data.frame(filter(x, HuntCat == "High"))
Moderate <- data.frame(filter(x, HuntCat == "Moderate"))
```


```r
mean(High$Diversity_BirdSpecies)
```

```
## [1] 1.660857
```

```r
mean(High$Diversity_MammalSpecies)
```

```
## [1] 1.737
```

```r
#The value of average diversity of birds tends to be lower than the value of average diversity of mammals
```


```r
mean(Moderate$Diversity_BirdSpecies)
```

```
## [1] 1.62125
```

```r
mean(Moderate$Diversity_MammalSpecies)
```

```
## [1] 1.68375
```

```r
#The value of average diversity of both birds and mammals tends to be higher in areas with High hunting intensity than of those with Moderate hunting intensity.

#I am unsure which diversity index is used to calculate species diversity, so I am unable to interpret whether a higher value for species diversity corresponds to lower or higher species diversity.
```

_You should have a look over the key and see where the problems are in this chunk. The numbers appear to be correct, but some of your coding has mistakes and can be improved._
</div>

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**


```r
vertebrate %>%
  group_by(RA_Apes, RA_Birds, RA_Elephant, RA_Monkeys, RA_Rodent, RA_Ungulate, Distance) %>%
  filter(Distance < 5 | Distance > 20) %>%
  summarize(Distance, across(starts_with("RA")))
```

```
## `summarise()` has grouped output by 'RA_Apes', 'RA_Birds', 'RA_Elephant', 'RA_Monkeys', 'RA_Rodent', 'RA_Ungulate'. You can override using the `.groups` argument.
```

```
## # A tibble: 6 x 7
## # Groups:   RA_Apes, RA_Birds, RA_Elephant, RA_Monkeys, RA_Rodent, RA_Ungulate
## #   [6]
##   RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate Distance
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>    <dbl>
## 1    0        57.8       0          37.8       3.19        1.04     3.83
## 2    0        85.0       0.290       9.09      3.74        1.86     2.7 
## 3    0.24     68.2       0          25.6       4.05        1.88     2.92
## 4    3.78     42.7       1.11       46.2       3.1         3.1     24.1 
## 5    4.91     31.6       0          54.1       1.29        8.12    26.8 
## 6   12.9      59.3       0.56       19.8       3.66        3.71    20.8
```
</div>

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**


```r
pull(vertebrate, Distance)
```

```
##  [1]  7.14 17.31 18.32 20.85 15.95 17.47 24.06 19.81  5.78  5.13  6.61  8.23
## [13]  2.70  6.10  3.83 18.85 13.96  6.61  5.14  5.33 26.76 15.02 11.21  2.92
```

```r
n_distinct(vertebrate$Distance)
```

```
## [1] 23
```
</div>


```r
#I apologize for the late submission! my apartment just had internet restored today after being out since tuesday. I emailed Dr. Ledford earlier this week and he has instructed me to submit whenever I am able to. 

#Thank you for grading, and all the effort you put into running this class! I am greatly enjoying BIS 015L :)
```

_Hi Rizk, No problem on the submission. I took some time going through your exam and there are several places where you should look over the key and see where things can be improved. Some of this is formatting, but in some cases you have some coding errors. Let me know if you have questions._
