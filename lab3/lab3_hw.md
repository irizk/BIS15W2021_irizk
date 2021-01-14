---
title: "Lab 3 Homework"
author: "Ibrahim Rizk"
date: "1/13/2021"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
help.search("mammals")
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- (ggplot2::msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
summary(sleep)
```

```
##      name              genus               vore              order          
##  Length:83          Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  conservation        sleep_total      sleep_rem      sleep_cycle    
##  Length:83          Min.   : 1.90   Min.   :0.100   Min.   :0.1167  
##  Class :character   1st Qu.: 7.85   1st Qu.:0.900   1st Qu.:0.1833  
##  Mode  :character   Median :10.10   Median :1.500   Median :0.3333  
##                     Mean   :10.43   Mean   :1.875   Mean   :0.4396  
##                     3rd Qu.:13.75   3rd Qu.:2.400   3rd Qu.:0.5792  
##                     Max.   :19.90   Max.   :6.600   Max.   :1.5000  
##                                     NA's   :22      NA's   :51      
##      awake          brainwt            bodywt        
##  Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##                  NA's   :27
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- subset(sleep, bodywt<=1)

sm_omit_NA <- na.omit(small)

sm <- data.frame(sm_omit_NA)

large <- subset(sleep, bodywt >= 200)

lg_omit_NA <- na.omit(large)
lg <- data.frame(lg_omit_NA)
```

8. What is the mean weight for both the small and large mammals?

```r
mean(sm, na.rm = TRUE)
```

```
## Warning in mean.default(sm, na.rm = TRUE): argument is not numeric or logical:
## returning NA
```

```
## [1] NA
```

```r
#I'm not sure how to remove the NA values :/ I tried na.omit and na.rm = TRUE but i keep getting an error message stating "argument is not numeric or logical: returning NA[1] NA"
```


```r
mean(lg_omit_NA)
```

```
## Warning in mean.default(lg_omit_NA): argument is not numeric or logical:
## returning NA
```

```
## [1] NA
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

```r
mean(sm$sleep_total)
```

```
## [1] 12.51818
```

```r
#Small animals sleep longer on average
```


```r
mean(lg$sleep_total)
```

```
## [1] 3.766667
```

10. Which animal is the sleepiest among the entire dataframe?

```r
max(sm$sleep_total)
```

```
## [1] 19.7
```

```r
#Big Brown Bat
```


```r
max(lg$sleep_total)
```

```
## [1] 4.4
```

```r
#Brazilian Tapir
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
