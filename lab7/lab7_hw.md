---
title: "Lab 7 Homework"
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
library(skimr)
library(here)
library(naniar)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- read_csv(here('lab7', 'data', 'amniota.csv'))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   common_name = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
amniota
```

```
## # A tibble: 21,322 x 36
##    class order   family  genus  species subspecies common_name  female_maturity~
##    <chr> <chr>   <chr>   <chr>  <chr>        <dbl> <chr>                   <dbl>
##  1 Aves  Accipi~ Accipi~ Accip~ albogu~       -999 Pied Goshawk            -999 
##  2 Aves  Accipi~ Accipi~ Accip~ badius        -999 Shikra                   363.
##  3 Aves  Accipi~ Accipi~ Accip~ bicolor       -999 Bicolored H~            -999 
##  4 Aves  Accipi~ Accipi~ Accip~ brachy~       -999 New Britain~            -999 
##  5 Aves  Accipi~ Accipi~ Accip~ brevip~       -999 Levant Spar~             363.
##  6 Aves  Accipi~ Accipi~ Accip~ castan~       -999 Chestnut-fl~            -999 
##  7 Aves  Accipi~ Accipi~ Accip~ chilen~       -999 Chilean Hawk            -999 
##  8 Aves  Accipi~ Accipi~ Accip~ chiono~       -999 White-breas~             548.
##  9 Aves  Accipi~ Accipi~ Accip~ cirroc~       -999 Collared Sp~            -999 
## 10 Aves  Accipi~ Accipi~ Accip~ cooper~       -999 Cooper's Ha~             730 
## # ... with 21,312 more rows, and 28 more variables:
## #   litter_or_clutch_size_n <dbl>, litters_or_clutches_per_y <dbl>,
## #   adult_body_mass_g <dbl>, maximum_longevity_y <dbl>, gestation_d <dbl>,
## #   weaning_d <dbl>, birth_or_hatching_weight_g <dbl>, weaning_weight_g <dbl>,
## #   egg_mass_g <dbl>, incubation_d <dbl>, fledging_age_d <dbl>,
## #   longevity_y <dbl>, male_maturity_d <dbl>,
## #   inter_litter_or_interbirth_interval_y <dbl>, female_body_mass_g <dbl>,
## #   male_body_mass_g <dbl>, no_sex_body_mass_g <dbl>, egg_width_mm <dbl>,
## #   egg_length_mm <dbl>, fledging_mass_g <dbl>, adult_svl_cm <dbl>,
## #   male_svl_cm <dbl>, female_svl_cm <dbl>, birth_or_hatching_svl_cm <dbl>,
## #   female_svl_at_maturity_cm <dbl>, female_body_mass_at_maturity_g <dbl>,
## #   no_sex_svl_cm <dbl>, no_sex_maturity_d <dbl>
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- read_csv(here('lab7', 'data', 'amphibio.csv'))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   id = col_character(),
##   Order = col_character(),
##   Family = col_character(),
##   Genus = col_character(),
##   Species = col_character(),
##   Seeds = col_logical(),
##   OBS = col_logical()
## )
## i Use `spec()` for the full column specifications.
```

```
## Warning: 125 parsing failures.
##  row col           expected                                                           actual                                                                                 file
## 1410 OBS 1/0/T/F/TRUE/FALSE Identified as P. appendiculata in Boquimpani-Freitas et al. 2002 'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'
## 1416 OBS 1/0/T/F/TRUE/FALSE Identified as T. miliaris in Giaretta and Facure 2004            'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'
## 1447 OBS 1/0/T/F/TRUE/FALSE Considered endangered by Soto-Azat et al. 2013                   'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'
## 1448 OBS 1/0/T/F/TRUE/FALSE Considered extinct by Soto-Azat et al. 2013                      'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'
## 1471 OBS 1/0/T/F/TRUE/FALSE nomem dubitum                                                    'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'
## .... ... .................. ................................................................ ....................................................................................
## See problems(...) for more details.
```

```r
amphibio
```

```
## # A tibble: 6,776 x 38
##    id    Order Family Genus Species   Fos   Ter   Aqu   Arb Leaves Flowers Seeds
##    <chr> <chr> <chr>  <chr> <chr>   <dbl> <dbl> <dbl> <dbl>  <dbl>   <dbl> <lgl>
##  1 Anf0~ Anura Allop~ Allo~ Alloph~    NA     1     1     1     NA      NA NA   
##  2 Anf0~ Anura Alyti~ Alyt~ Alytes~    NA     1     1     1     NA      NA NA   
##  3 Anf0~ Anura Alyti~ Alyt~ Alytes~    NA     1     1     1     NA      NA NA   
##  4 Anf0~ Anura Alyti~ Alyt~ Alytes~    NA     1     1     1     NA      NA NA   
##  5 Anf0~ Anura Alyti~ Alyt~ Alytes~    NA     1    NA     1     NA      NA NA   
##  6 Anf0~ Anura Alyti~ Alyt~ Alytes~     1     1     1     1     NA      NA NA   
##  7 Anf0~ Anura Alyti~ Disc~ Discog~     1     1     1    NA     NA      NA NA   
##  8 Anf0~ Anura Alyti~ Disc~ Discog~     1     1     1    NA     NA      NA NA   
##  9 Anf0~ Anura Alyti~ Disc~ Discog~     1     1     1    NA     NA      NA NA   
## 10 Anf0~ Anura Alyti~ Disc~ Discog~     1     1     1    NA     NA      NA NA   
## # ... with 6,766 more rows, and 26 more variables: Fruits <dbl>, Arthro <dbl>,
## #   Vert <dbl>, Diu <dbl>, Noc <dbl>, Crepu <dbl>, Wet_warm <dbl>,
## #   Wet_cold <dbl>, Dry_warm <dbl>, Dry_cold <dbl>, Body_mass_g <dbl>,
## #   Age_at_maturity_min_y <dbl>, Age_at_maturity_max_y <dbl>,
## #   Body_size_mm <dbl>, Size_at_maturity_min_mm <dbl>,
## #   Size_at_maturity_max_mm <dbl>, Longevity_max_y <dbl>,
## #   Litter_size_min_n <dbl>, Litter_size_max_n <dbl>,
## #   Reproductive_output_y <dbl>, Offspring_size_min_mm <dbl>,
## #   Offspring_size_max_mm <dbl>, Dir <dbl>, Lar <dbl>, Viv <dbl>, OBS <lgl>
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
skimr::skim(amniota)
```


Table: Data summary

|                         |        |
|:------------------------|:-------|
|Name                     |amniota |
|Number of rows           |21322   |
|Number of columns        |36      |
|_______________________  |        |
|Column type frequency:   |        |
|character                |6       |
|numeric                  |30      |
|________________________ |        |
|Group variables          |None    |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|class         |         0|             1|   4|   8|     0|        3|          0|
|order         |         0|             1|   6|  19|     0|       72|          0|
|family        |         0|             1|   6|  19|     0|      465|          0|
|genus         |         0|             1|   2|  20|     0|     4336|          0|
|species       |         0|             1|   2|  21|     0|    11548|          0|
|common_name   |         0|             1|   2| 306|     0|    19625|          0|


**Variable type: numeric**

|skim_variable                         | n_missing| complete_rate|     mean|         sd|        p0|     p25|     p50|     p75|         p100|hist  |
|:-------------------------------------|---------:|-------------:|--------:|----------:|---------:|-------:|-------:|-------:|------------:|:-----|
|subspecies                            |         0|             1|  -999.00|       0.00|   -999.00| -999.00| -999.00| -999.00|      -999.00|▁▁▇▁▁ |
|female_maturity_d                     |         0|             1|  -723.70|     830.62| -30258.71| -999.00| -999.00| -999.00|      9131.25|▁▁▁▇▁ |
|litter_or_clutch_size_n               |         0|             1|  -383.91|     488.39|   -999.00| -999.00|    1.69|    3.20|       156.00|▅▁▁▁▇ |
|litters_or_clutches_per_y             |         0|             1|  -766.76|     422.48|   -999.00| -999.00| -999.00| -999.00|        52.00|▇▁▁▁▂ |
|adult_body_mass_g                     |         0|             1| 29107.30| 1278639.85|   -999.00|    4.44|   23.61|  135.00| 149000000.00|▇▁▁▁▁ |
|maximum_longevity_y                   |         0|             1|  -737.06|     444.36|   -999.00| -999.00| -999.00|    1.08|       211.00|▇▁▁▁▃ |
|gestation_d                           |         0|             1|  -874.91|     353.92|   -999.00| -999.00| -999.00| -999.00|      7396.92|▇▁▁▁▁ |
|weaning_d                             |         0|             1|  -892.45|     330.67|   -999.00| -999.00| -999.00| -999.00|      1826.25|▇▁▁▁▁ |
|birth_or_hatching_weight_g            |         0|             1|   -88.57|   26484.20|   -999.00| -999.00| -999.00| -999.00|   2250000.00|▇▁▁▁▁ |
|weaning_weight_g                      |         0|             1|  1116.10|  134348.60|   -999.00| -999.00| -999.00| -999.00|  17000000.00|▇▁▁▁▁ |
|egg_mass_g                            |         0|             1|  -739.64|     445.35|   -999.00| -999.00| -999.00|    0.56|      1500.00|▇▁▂▁▁ |
|incubation_d                          |         0|             1|  -820.49|     394.55|   -999.00| -999.00| -999.00| -999.00|      1762.00|▇▂▁▁▁ |
|fledging_age_d                        |         0|             1|  -909.42|     291.29|   -999.00| -999.00| -999.00| -999.00|       345.00|▇▁▁▁▁ |
|longevity_y                           |         0|             1|  -737.82|     443.03|   -999.00| -999.00| -999.00|    1.04|       177.00|▇▁▁▁▃ |
|male_maturity_d                       |         0|             1|  -827.77|     595.69|   -999.00| -999.00| -999.00| -999.00|      9131.25|▇▁▁▁▁ |
|inter_litter_or_interbirth_interval_y |         0|             1|  -932.50|     249.14|   -999.00| -999.00| -999.00| -999.00|         4.85|▇▁▁▁▁ |
|female_body_mass_g                    |         0|             1|    40.59|   27536.51|   -999.00| -999.00| -999.00|   14.50|   3700000.00|▇▁▁▁▁ |
|male_body_mass_g                      |         0|             1|  1242.90|   62044.69|   -999.00| -999.00| -999.00|   13.34|   4545000.00|▇▁▁▁▁ |
|no_sex_body_mass_g                    |         0|             1| 30689.26| 1467346.84|   -999.00| -999.00| -999.00|   27.77| 136000000.00|▇▁▁▁▁ |
|egg_width_mm                          |         0|             1|  -970.48|     168.36|   -999.00| -999.00| -999.00| -999.00|       125.00|▇▁▁▁▁ |
|egg_length_mm                         |         0|             1|  -968.89|     174.10|   -999.00| -999.00| -999.00| -999.00|       455.00|▇▁▁▁▁ |
|fledging_mass_g                       |         0|             1|  -984.64|     211.46|   -999.00| -999.00| -999.00| -999.00|      9992.00|▇▁▁▁▁ |
|adult_svl_cm                          |         0|             1|  -656.15|     490.74|   -999.00| -999.00| -999.00|    9.50|      3049.00|▇▃▁▁▁ |
|male_svl_cm                           |         0|             1|  -985.12|     120.02|   -999.00| -999.00| -999.00| -999.00|       315.20|▇▁▁▁▁ |
|female_svl_cm                         |         0|             1|  -947.35|     223.83|   -999.00| -999.00| -999.00| -999.00|      1125.00|▇▁▁▁▁ |
|birth_or_hatching_svl_cm              |         0|             1|  -940.34|     236.74|   -999.00| -999.00| -999.00| -999.00|       760.00|▇▁▁▁▁ |
|female_svl_at_maturity_cm             |         0|             1|  -989.36|      98.74|   -999.00| -999.00| -999.00| -999.00|       580.00|▇▁▁▁▁ |
|female_body_mass_at_maturity_g        |         0|             1|  -980.61|    1888.55|   -999.00| -999.00| -999.00| -999.00|    194000.00|▇▁▁▁▁ |
|no_sex_svl_cm                         |         0|             1|  -747.14|     442.27|   -999.00| -999.00| -999.00| -999.00|      3300.00|▇▂▁▁▁ |
|no_sex_maturity_d                     |         0|             1|  -942.59|     465.04|   -999.00| -999.00| -999.00| -999.00|     14610.00|▇▁▁▁▁ |

```r
naniar::miss_var_summary(amniota)
```

```
## # A tibble: 36 x 3
##    variable                  n_miss pct_miss
##    <chr>                      <int>    <dbl>
##  1 class                          0        0
##  2 order                          0        0
##  3 family                         0        0
##  4 genus                          0        0
##  5 species                        0        0
##  6 subspecies                     0        0
##  7 common_name                    0        0
##  8 female_maturity_d              0        0
##  9 litter_or_clutch_size_n        0        0
## 10 litters_or_clutches_per_y      0        0
## # ... with 26 more rows
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
str(amphibio)
```

```
## spec_tbl_df [6,776 x 38] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ id                     : chr [1:6776] "Anf0001" "Anf0002" "Anf0003" "Anf0004" ...
##  $ Order                  : chr [1:6776] "Anura" "Anura" "Anura" "Anura" ...
##  $ Family                 : chr [1:6776] "Allophrynidae" "Alytidae" "Alytidae" "Alytidae" ...
##  $ Genus                  : chr [1:6776] "Allophryne" "Alytes" "Alytes" "Alytes" ...
##  $ Species                : chr [1:6776] "Allophryne ruthveni" "Alytes cisternasii" "Alytes dickhilleni" "Alytes maurus" ...
##  $ Fos                    : num [1:6776] NA NA NA NA NA 1 1 1 1 1 ...
##  $ Ter                    : num [1:6776] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Aqu                    : num [1:6776] 1 1 1 1 NA 1 1 1 1 1 ...
##  $ Arb                    : num [1:6776] 1 1 1 1 1 1 NA NA NA NA ...
##  $ Leaves                 : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Flowers                : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Seeds                  : logi [1:6776] NA NA NA NA NA NA ...
##  $ Fruits                 : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Arthro                 : num [1:6776] 1 1 1 NA 1 1 1 1 1 NA ...
##  $ Vert                   : num [1:6776] NA NA NA NA NA NA 1 NA NA NA ...
##  $ Diu                    : num [1:6776] 1 NA NA NA NA NA 1 1 1 NA ...
##  $ Noc                    : num [1:6776] 1 1 1 NA 1 1 1 1 1 NA ...
##  $ Crepu                  : num [1:6776] 1 NA NA NA NA 1 NA NA NA NA ...
##  $ Wet_warm               : num [1:6776] NA NA NA NA 1 1 NA NA NA NA ...
##  $ Wet_cold               : num [1:6776] 1 NA NA NA NA NA 1 NA NA NA ...
##  $ Dry_warm               : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Dry_cold               : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Body_mass_g            : num [1:6776] 31 6.1 NA NA 2.31 13.4 21.8 NA NA NA ...
##  $ Age_at_maturity_min_y  : num [1:6776] NA 2 2 NA 3 2 3 NA NA NA ...
##  $ Age_at_maturity_max_y  : num [1:6776] NA 2 2 NA 3 3 5 NA NA NA ...
##  $ Body_size_mm           : num [1:6776] 31 50 55 NA 40 55 80 60 65 NA ...
##  $ Size_at_maturity_min_mm: num [1:6776] NA 27 NA NA NA 35 NA NA NA NA ...
##  $ Size_at_maturity_max_mm: num [1:6776] NA 36 NA NA NA 40.5 NA NA NA NA ...
##  $ Longevity_max_y        : num [1:6776] NA 6 NA NA NA 7 9 NA NA NA ...
##  $ Litter_size_min_n      : num [1:6776] 300 60 40 NA 7 53 300 1500 1000 NA ...
##  $ Litter_size_max_n      : num [1:6776] 300 180 40 NA 20 171 1500 1500 1000 NA ...
##  $ Reproductive_output_y  : num [1:6776] 1 4 1 4 1 4 6 1 1 1 ...
##  $ Offspring_size_min_mm  : num [1:6776] NA 2.6 NA NA 5.4 2.6 1.5 NA 1.5 NA ...
##  $ Offspring_size_max_mm  : num [1:6776] NA 3.5 NA NA 7 5 2 NA 1.5 NA ...
##  $ Dir                    : num [1:6776] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Lar                    : num [1:6776] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Viv                    : num [1:6776] 0 0 0 0 0 0 0 0 0 0 ...
##  $ OBS                    : logi [1:6776] NA NA NA NA NA NA ...
##  - attr(*, "problems")= tibble [125 x 5] (S3: tbl_df/tbl/data.frame)
##   ..$ row     : int [1:125] 1410 1416 1447 1448 1471 1488 1539 1540 1543 1544 ...
##   ..$ col     : chr [1:125] "OBS" "OBS" "OBS" "OBS" ...
##   ..$ expected: chr [1:125] "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" "1/0/T/F/TRUE/FALSE" ...
##   ..$ actual  : chr [1:125] "Identified as P. appendiculata in Boquimpani-Freitas et al. 2002" "Identified as T. miliaris in Giaretta and Facure 2004" "Considered endangered by Soto-Azat et al. 2013" "Considered extinct by Soto-Azat et al. 2013" ...
##   ..$ file    : chr [1:125] "'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'" "'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'" "'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'" "'D:/TA files/Winter2021 BIS15L/students_rep/BIS15W2021_irizk/lab7/data/amphibio.csv'" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   id = col_character(),
##   ..   Order = col_character(),
##   ..   Family = col_character(),
##   ..   Genus = col_character(),
##   ..   Species = col_character(),
##   ..   Fos = col_double(),
##   ..   Ter = col_double(),
##   ..   Aqu = col_double(),
##   ..   Arb = col_double(),
##   ..   Leaves = col_double(),
##   ..   Flowers = col_double(),
##   ..   Seeds = col_logical(),
##   ..   Fruits = col_double(),
##   ..   Arthro = col_double(),
##   ..   Vert = col_double(),
##   ..   Diu = col_double(),
##   ..   Noc = col_double(),
##   ..   Crepu = col_double(),
##   ..   Wet_warm = col_double(),
##   ..   Wet_cold = col_double(),
##   ..   Dry_warm = col_double(),
##   ..   Dry_cold = col_double(),
##   ..   Body_mass_g = col_double(),
##   ..   Age_at_maturity_min_y = col_double(),
##   ..   Age_at_maturity_max_y = col_double(),
##   ..   Body_size_mm = col_double(),
##   ..   Size_at_maturity_min_mm = col_double(),
##   ..   Size_at_maturity_max_mm = col_double(),
##   ..   Longevity_max_y = col_double(),
##   ..   Litter_size_min_n = col_double(),
##   ..   Litter_size_max_n = col_double(),
##   ..   Reproductive_output_y = col_double(),
##   ..   Offspring_size_min_mm = col_double(),
##   ..   Offspring_size_max_mm = col_double(),
##   ..   Dir = col_double(),
##   ..   Lar = col_double(),
##   ..   Viv = col_double(),
##   ..   OBS = col_logical()
##   .. )
```

```r
naniar::miss_var_summary(amphibio)
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 OBS        6776    100  
##  2 Fruits     6774    100. 
##  3 Flowers    6772     99.9
##  4 Seeds      6772     99.9
##  5 Leaves     6752     99.6
##  6 Dry_cold   6735     99.4
##  7 Vert       6657     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ... with 28 more rows
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   


```r
amniota %>%
  summarize_all(~(sum(is.na(.))))
```

```
## # A tibble: 1 x 36
##   class order family genus species subspecies common_name female_maturity_d
##   <int> <int>  <int> <int>   <int>      <int>       <int>             <int>
## 1     0     0      0     0       0          0           0                 0
## # ... with 28 more variables: litter_or_clutch_size_n <int>,
## #   litters_or_clutches_per_y <int>, adult_body_mass_g <int>,
## #   maximum_longevity_y <int>, gestation_d <int>, weaning_d <int>,
## #   birth_or_hatching_weight_g <int>, weaning_weight_g <int>, egg_mass_g <int>,
## #   incubation_d <int>, fledging_age_d <int>, longevity_y <int>,
## #   male_maturity_d <int>, inter_litter_or_interbirth_interval_y <int>,
## #   female_body_mass_g <int>, male_body_mass_g <int>, no_sex_body_mass_g <int>,
## #   egg_width_mm <int>, egg_length_mm <int>, fledging_mass_g <int>,
## #   adult_svl_cm <int>, male_svl_cm <int>, female_svl_cm <int>,
## #   birth_or_hatching_svl_cm <int>, female_svl_at_maturity_cm <int>,
## #   female_body_mass_at_maturity_g <int>, no_sex_svl_cm <int>,
## #   no_sex_maturity_d <int>
```

```r
#This output is misleading. I believe it is because R cannot recognize NA values because they are represented by numeric values.
```


```r
amphibio %>%
  naniar::miss_var_summary()
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 OBS        6776    100  
##  2 Fruits     6774    100. 
##  3 Flowers    6772     99.9
##  4 Seeds      6772     99.9
##  5 Leaves     6752     99.6
##  6 Dry_cold   6735     99.4
##  7 Vert       6657     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ... with 28 more rows
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota_tidy <- amniota %>%
  na_if('-999') %>%
  na_if('-30259.')
naniar::miss_var_summary(amniota_tidy)
```

```
## # A tibble: 36 x 3
##    variable                       n_miss pct_miss
##    <chr>                           <int>    <dbl>
##  1 subspecies                      21322    100  
##  2 female_body_mass_at_maturity_g  21318    100. 
##  3 female_svl_at_maturity_cm       21120     99.1
##  4 fledging_mass_g                 21111     99.0
##  5 male_svl_cm                     21040     98.7
##  6 no_sex_maturity_d               20860     97.8
##  7 egg_width_mm                    20727     97.2
##  8 egg_length_mm                   20702     97.1
##  9 weaning_weight_g                20258     95.0
## 10 female_svl_cm                   20242     94.9
## # ... with 26 more rows
```


```r
amniota_tidy %>%
  summarise(NA_total_amniota_tidy = (sum(is.na(.))))
```

```
## # A tibble: 1 x 1
##   NA_total_amniota_tidy
##                   <int>
## 1                528196
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
naniar::miss_var_summary(amniota)
```

```
## # A tibble: 36 x 3
##    variable                  n_miss pct_miss
##    <chr>                      <int>    <dbl>
##  1 class                          0        0
##  2 order                          0        0
##  3 family                         0        0
##  4 genus                          0        0
##  5 species                        0        0
##  6 subspecies                     0        0
##  7 common_name                    0        0
##  8 female_maturity_d              0        0
##  9 litter_or_clutch_size_n        0        0
## 10 litters_or_clutches_per_y      0        0
## # ... with 26 more rows
```
</div>

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
naniar::miss_var_summary(amphibio)
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 OBS        6776    100  
##  2 Fruits     6774    100. 
##  3 Flowers    6772     99.9
##  4 Seeds      6772     99.9
##  5 Leaves     6752     99.6
##  6 Dry_cold   6735     99.4
##  7 Vert       6657     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ... with 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  


```r
amniota_tidy %>%
  select(class, egg_mass_g) %>%
  group_by(class) %>%
  naniar::miss_var_summary()
```

```
## # A tibble: 3 x 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g   4914     50.1
## 2 Mammalia egg_mass_g   4953    100  
## 3 Reptilia egg_mass_g   6040     92.0
```

```r
#These results do not make much sense biologically
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
amphibio %>%
  select(Fos, Ter, Aqu, Arb) %>%
  naniar::miss_var_summary()
```

```
## # A tibble: 4 x 3
##   variable n_miss pct_miss
##   <chr>     <int>    <dbl>
## 1 Fos        6053     89.3
## 2 Arb        4347     64.2
## 3 Aqu        2810     41.5
## 4 Ter        1104     16.3
```

```r
#No - Because these traits are categorical, I believe the authors intended to indicate the presence or absence of a trait. 
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
# amniota_tidier <- read_csv(here('lab7', 'data', 'amniota.csv')) %>%
  # naniar::replace_with_na_all(condition = ~.x %in% common_na_strings) %>%
  # naniar::replace_with_na_all(condition = ~.x %in% common_na_numbers)
  
#or, 
# amniota_tidier <- read_csv(here('lab7', 'data', 'amniota.csv')) %>%
  #na_if(c(' ', '-999', 'NA', '.'))
```
</div>

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
