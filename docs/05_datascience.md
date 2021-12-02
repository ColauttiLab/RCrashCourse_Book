# Data Science Intro

## Setup

If you don't have it installed, then run `install.packages("dplyr")` -- it can take a while to download and install.

## Introduction to Data Science

Data Science is a relatively new field of study that merges **computer science** and **statistics** to answer questions in other domains  (e.g. business, medicine, biology, psychology). Data Science as a discipline has grown in popularity in response to the rapid rate of increase in data collection and publication. 

Data Science often involves 'Big Data', which doesn't have a strict quantitative definition but will usually have one or more of the following characteristics:

  1. High **Volume** -- large file sizes with lots of observations.
  2. Wide **Variety** -- lots of different types
  3. High **Velocity** -- accumulating at a high rate
  4. Compromised **Veracity** -- variable quality that must be dealt otherwise downstream analyses will be compromised.

> What are some examples of 'big data' in Biology?

Medical records, remote sensing data (e.g. climate stations, satellite images), and 'omics data are good examples of 'big data' in biology.

In biology, it can be helpful to think of Data Science as a continuous life-cycle with multiple stages:

### Data Science Life-Cycle

  1. **Hypothesize** -- Make initial observations of about the natural world, or insights from other data, that lead to testable hypotheses. Your core biology training is crucial here.
  2. **Collect** -- This may involve taking measurements yourself, manually entering data that is not yet in electronic format, requesting data from authors of published studies, or importing data from online sources. Collecting data is a crucial step that is often done poorly. Some tips on this are provided in a [paper by Wu et al](https://www.biorxiv.org/content/early/2018/10/30/457051)
  3. **Correct** -- Investigate the data for quality assurance, to identify and fix potential errors. Start to visualize the data to look for outliers or nonsensical relationships (or lack thereof).
  4. **Explore** -- Try to understand the data, where they come from, and potential limitations on their use. Continue visualizing data; this may cause you to modify your hypotheses slightly.
  5. **Model** -- Now that hypotheses are clearly defined, apply statistical tests of their validity.
  6. **Report** -- Use visualizations along with the results of your statistical tests to summarize your findings.
  7. **Repeat** -- Return to step 1.

In this tutorial, we focus mainly on coding in R for steps 2, 3, and 6. Step 5 requires a firm understanding of statistics. Step 4 is covered in the tutorials on [qplot](./2_qplot.html) and [ggplot](./3_ggplot.html). Step 1 is everything covered in a typical degree in the biological sciences. 

Data collection and management are crucial steps in the Data Science Life-Cycle. Read the paper by [Wu et al](https://www.biorxiv.org/content/early/2018/10/30/457051). called *baRcodeR with PyTrackDat: Open-source labelling and tracking of biological samples for repeatable science.* Pay particular attention to the '*Data Standards*' section. The *baRcodeR* and *PyTrackDat* programs and their application to current projects may also be of interest.

### Data Science in R

The book [R for Data Science](http://shop.oreilly.com/product/0636920034407.do) by Hadley Wickham & Garrett Grolemund is an excellent resource using R in Data Science projects. 

> TIP: In general, any book by Hadley Wickham that you come across is worth reading if you want to be proficient in R.

Here we'll be focusing on the `dplyr` packages from Wickham et al.

## 2D Data Wrangling

The `dplyr` library in R has many useful features for importing and reorganizing your data for steps 2, 3 and 4 in the Data Science Life-Cycle outlined above. Don't forget to install the `dplyr` library and load it into memory.


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

> Note: This error message just informs you that `dplyr` uses function or parameter names that are the same as other base or stats packages in R. These base/stats functions are 'masked' meaning that when you run one (e.g. `filter`) then R will run the `dplyr` version rather than the stats version.

We'll work with our FallopiaData.csv dataset, and remind ourselves of the structure of the data


```r
Fallo<-read.csv("https://colauttilab.github.io/RCrashCourse/FallopiaData.csv")
str(Fallo)
```

```
## 'data.frame':	123 obs. of  13 variables:
##  $ PotNum      : int  1 2 3 5 6 7 8 9 10 11 ...
##  $ Scenario    : chr  "low" "low" "low" "low" ...
##  $ Nutrients   : chr  "low" "low" "low" "low" ...
##  $ Taxon       : chr  "japon" "japon" "japon" "japon" ...
##  $ Symphytum   : num  9.81 8.64 2.65 1.44 9.15 ...
##  $ Silene      : num  36.4 29.6 36 21.4 23.9 ...
##  $ Urtica      : num  16.08 5.59 17.09 12.39 5.19 ...
##  $ Geranium    : num  4.68 5.75 5.13 5.37 0 9.05 3.51 9.64 7.3 6.36 ...
##  $ Geum        : num  0.12 0.55 0.09 0.31 0.17 0.97 0.4 0.01 0.47 0.33 ...
##  $ All_Natives : num  67 50.2 61 40.9 38.4 ...
##  $ Fallopia    : num  0.01 0.04 0.09 0.77 3.4 0.54 2.05 0.26 0 0 ...
##  $ Total       : num  67.1 50.2 61.1 41.7 41.8 ...
##  $ Pct_Fallopia: num  0.01 0.08 0.15 1.85 8.13 1.12 3.7 0.61 0 0 ...
```

This file is an example of a 2-dimensional data set, which is common in biology. 2D datasets have the familiar row x column layout used by spreadsheet programs like Microsoft Excel or Google Sheets. There are some exceptions, but data in this format should typically follows 3 rules:

  1. Each cell contains a single value
  2. Each variable must have its own column
  3. Each observation must have its own row

Making sure your data are arranged this way will usually make it much easier to work with.

### `filter()` 

Let's subset observations based on value


```r
Pot1<-filter(Fallo,PotNum==1)
head(Pot1)
```

```
##   PotNum Scenario Nutrients Taxon Symphytum Silene Urtica Geranium Geum
## 1      1      low       low japon      9.81  36.36  16.08     4.68 0.12
##   All_Natives Fallopia Total Pct_Fallopia
## 1       67.05     0.01 67.06         0.01
```

### `rename()`

It's possible to change the names of columns in your data. In base R you can use the `names()` function with the square bracket index `[]`:


```r
X<-Fallo
names(X)
```

```
##  [1] "PotNum"       "Scenario"     "Nutrients"    "Taxon"        "Symphytum"   
##  [6] "Silene"       "Urtica"       "Geranium"     "Geum"         "All_Natives" 
## [11] "Fallopia"     "Total"        "Pct_Fallopia"
```

```r
names(X)[12]<-"Total_Biomass"
names(X)
```

```
##  [1] "PotNum"        "Scenario"      "Nutrients"     "Taxon"        
##  [5] "Symphytum"     "Silene"        "Urtica"        "Geranium"     
##  [9] "Geum"          "All_Natives"   "Fallopia"      "Total_Biomass"
## [13] "Pct_Fallopia"
```


```r
X<-rename(Fallo, Total_Biomass = Total)
names(X)
```

```
##  [1] "PotNum"        "Scenario"      "Nutrients"     "Taxon"        
##  [5] "Symphytum"     "Silene"        "Urtica"        "Geranium"     
##  [9] "Geum"          "All_Natives"   "Fallopia"      "Total_Biomass"
## [13] "Pct_Fallopia"
```

There is also a simple `dplyr` function to do this:

### `arrange()` 

Use the `arrange()` function to sort the *rows* of your data based on the *columns* of your data. For example, let's re-arrange our FallopiaData.csv dataset based on Taxon (a string denoting the species of Fallopia used) and Total (a float denoting the total biomass in each pot).


```r
X<-arrange(Fallo, Taxon, Total)
head(X)
```

```
##   PotNum Scenario Nutrients Taxon Symphytum Silene Urtica Geranium Geum
## 1     26      low       low bohem     13.25  18.11   0.00     0.00 0.10
## 2     17      low       low bohem      4.90  29.52   1.36     0.00 0.19
## 3     80  gradual      high bohem     11.92  17.16   8.92     0.94 0.18
## 4     18      low       low bohem      3.51  27.61   8.14     3.81 0.21
## 5     28      low       low bohem     10.59  18.78   7.19     6.73 0.17
## 6     22      low       low bohem      0.76  22.66   9.85    10.60 0.74
##   All_Natives Fallopia Total Pct_Fallopia
## 1       31.46     0.00 31.46         0.00
## 2       35.97     0.00 35.97         0.00
## 3       39.12     3.35 42.47         7.89
## 4       43.28     0.00 43.28         0.00
## 5       43.46     0.00 43.46         0.00
## 6       44.61     0.00 44.61         0.00
```

use the `desc()` function with `arrange()` to change to descending order


```r
X<-arrange(Fallo, Taxon, desc(Total))
head(X)
```

```
##   PotNum     Scenario Nutrients Taxon Symphytum Silene Urtica Geranium Geum
## 1    148 fluctuations      high bohem      4.15  38.70  23.59     5.11 1.36
## 2     86      gradual      high bohem      2.93  60.93   4.11     6.67 1.27
## 3     60         high      high bohem      7.77  51.45   5.13    10.10 0.37
## 4     85      gradual      high bohem     10.19  26.66  15.01     3.07 0.14
## 5     53         high      high bohem      7.05  56.29   1.14     4.07 0.00
## 6    118      extreme      high bohem     14.21  25.15  12.69     8.06 0.29
##   All_Natives Fallopia Total Pct_Fallopia
## 1       72.91     5.89 78.80         7.47
## 2       75.91     0.00 75.91         0.00
## 3       74.82     0.00 74.82         0.00
## 4       55.07    14.47 69.54        20.81
## 5       68.55     0.00 68.55         0.00
## 6       60.40     7.46 67.86        10.99
```

### `select()` 

The `select()` function can be used to select a subset of columns (i.e. variables) from your data.

Suppose we only want to look at total biomass, but keep all the treatment columns:


```r
X<-select(Fallo, PotNum, Scenario, Nutrients, Taxon, Total)
head(X)
```

```
##   PotNum Scenario Nutrients Taxon Total
## 1      1      low       low japon 67.06
## 2      2      low       low japon 50.22
## 3      3      low       low japon 61.08
## 4      5      low       low japon 41.71
## 5      6      low       low japon 41.81
## 6      7      low       low japon 48.27
```

You can also use the colon `:` to select a range of columns:


```r
X<-select(Fallo, PotNum:Taxon, Total)
head(X)
```

```
##   PotNum Scenario Nutrients Taxon Total
## 1      1      low       low japon 67.06
## 2      2      low       low japon 50.22
## 3      3      low       low japon 61.08
## 4      5      low       low japon 41.71
## 5      6      low       low japon 41.81
## 6      7      low       low japon 48.27
```

Exclude columns with `-`


```r
X<-select(Fallo, -PotNum:Taxon, -Total)
```

```
## Warning in x:y: numerical expression has 12 elements: only the first used
```

> Oops, what generated that error? Take a careful look at the error message and see if you can figure it out.

The problem is we are using the range of columns between PotNum and Taxon, but in one case we are excluding and the other we are including. We need to keep both the same:


```r
X<-select(Fallo, -PotNum:-Taxon, Total)
head(X)
```

```
##   Symphytum Silene Urtica Geranium Geum All_Natives Fallopia Total Pct_Fallopia
## 1      9.81  36.36  16.08     4.68 0.12       67.05     0.01 67.06         0.01
## 2      8.64  29.65   5.59     5.75 0.55       50.18     0.04 50.22         0.08
## 3      2.65  36.03  17.09     5.13 0.09       60.99     0.09 61.08         0.15
## 4      1.44  21.43  12.39     5.37 0.31       40.94     0.77 41.71         1.85
## 5      9.15  23.90   5.19     0.00 0.17       38.41     3.40 41.81         8.13
## 6      6.31  24.40   7.00     9.05 0.97       47.73     0.54 48.27         1.12
```

Or a bit more clear:


```r
X<-select(Fallo, -(PotNum:Taxon), Total)
head(X)
```

```
##   Symphytum Silene Urtica Geranium Geum All_Natives Fallopia Total Pct_Fallopia
## 1      9.81  36.36  16.08     4.68 0.12       67.05     0.01 67.06         0.01
## 2      8.64  29.65   5.59     5.75 0.55       50.18     0.04 50.22         0.08
## 3      2.65  36.03  17.09     5.13 0.09       60.99     0.09 61.08         0.15
## 4      1.44  21.43  12.39     5.37 0.31       40.94     0.77 41.71         1.85
## 5      9.15  23.90   5.19     0.00 0.17       38.41     3.40 41.81         8.13
## 6      6.31  24.40   7.00     9.05 0.97       47.73     0.54 48.27         1.12
```

### `everything()`

Use the `everything()` function with `select()` to rearrange your columns without losing any:


```r
X<-select(Fallo, Taxon, Scenario, Nutrients, PotNum, Pct_Fallopia, everything())
head(X)
```

```
##   Taxon Scenario Nutrients PotNum Pct_Fallopia Symphytum Silene Urtica Geranium
## 1 japon      low       low      1         0.01      9.81  36.36  16.08     4.68
## 2 japon      low       low      2         0.08      8.64  29.65   5.59     5.75
## 3 japon      low       low      3         0.15      2.65  36.03  17.09     5.13
## 4 japon      low       low      5         1.85      1.44  21.43  12.39     5.37
## 5 japon      low       low      6         8.13      9.15  23.90   5.19     0.00
## 6 japon      low       low      7         1.12      6.31  24.40   7.00     9.05
##   Geum All_Natives Fallopia Total
## 1 0.12       67.05     0.01 67.06
## 2 0.55       50.18     0.04 50.22
## 3 0.09       60.99     0.09 61.08
## 4 0.31       40.94     0.77 41.71
## 5 0.17       38.41     3.40 41.81
## 6 0.97       47.73     0.54 48.27
```

### `mutate()`

Suppose we want to make a new column (variable) to our data.frame object (dataset) that is the sum of biomass of Urtica and Geranium only. In base R you would use `$`:


```r
X<-Fallo
X$UrtSil<-X$Urtica+X$Silene
```

In the dplyr package you can use mutate


```r
X<-mutate(Fallo, UrtSil = Urtica + Silene)
head(X)
```

```
##   PotNum Scenario Nutrients Taxon Symphytum Silene Urtica Geranium Geum
## 1      1      low       low japon      9.81  36.36  16.08     4.68 0.12
## 2      2      low       low japon      8.64  29.65   5.59     5.75 0.55
## 3      3      low       low japon      2.65  36.03  17.09     5.13 0.09
## 4      5      low       low japon      1.44  21.43  12.39     5.37 0.31
## 5      6      low       low japon      9.15  23.90   5.19     0.00 0.17
## 6      7      low       low japon      6.31  24.40   7.00     9.05 0.97
##   All_Natives Fallopia Total Pct_Fallopia UrtSil
## 1       67.05     0.01 67.06         0.01  52.44
## 2       50.18     0.04 50.22         0.08  35.24
## 3       60.99     0.09 61.08         0.15  53.12
## 4       40.94     0.77 41.71         1.85  33.82
## 5       38.41     3.40 41.81         8.13  29.09
## 6       47.73     0.54 48.27         1.12  31.40
```

This is a lot more readable, especially when you have complicated equations or you want to add lots of new columns.

> What if you only wanted to retain the new columns and delete everything else? Try it.

Which functions did you use?

### `transmute()`

You can also use `transmute()` instead of `mutate()` + `select()`


```r
X<-transmute(Fallo, UrtSil = Urtica + Silene)
head(X)
```

```
##   UrtSil
## 1  52.44
## 2  35.24
## 3  53.12
## 4  33.82
## 5  29.09
## 6  31.40
```

### `summarize()` + `group_by()`

This can be useful for quickly summarizing your data, for example to find the mean or standard deviation based on a particular treatment or group.


```r
TrtGrp<-group_by(Fallo,Taxon,Scenario,Nutrients)
summarize(TrtGrp, Mean=mean(Total), SD=sd(Total))
```

```
## `summarise()` has grouped output by 'Taxon', 'Scenario'. You can override using the `.groups` argument.
```

```
## # A tibble: 10 x 5
## # Groups:   Taxon, Scenario [10]
##    Taxon Scenario     Nutrients  Mean    SD
##    <chr> <chr>        <chr>     <dbl> <dbl>
##  1 bohem extreme      high       58.3  7.34
##  2 bohem fluctuations high       58.4  9.20
##  3 bohem gradual      high       57.5  9.34
##  4 bohem high         high       60.3  8.68
##  5 bohem low          low        48.0  8.86
##  6 japon extreme      high       57.2 10.9 
##  7 japon fluctuations high       56.4 13.7 
##  8 japon gradual      high       59.7  9.57
##  9 japon high         high       56.4  8.20
## 10 japon low          low        52.0  8.29
```

### Weighted Mean

In our dataset, the **Taxon** column shows which of two species of *Fallopia* were used in the competition experiments. We might want to take the mean total biomass for each of the two *Fallopia* species:


```r
X<-group_by(Fallo,Taxon)
summarize(X, Mean=mean(Total), SD=sd(Total))
```

```
## # A tibble: 2 x 3
##   Taxon  Mean    SD
##   <chr> <dbl> <dbl>
## 1 bohem  56.3  9.54
## 2 japon  56.4 10.4
```

However, there are other factors in our experiment that may affect biomass. The *Nutrients* column tells us whether pots received high or low nutrients, and this also affects biomass:


```r
X<-group_by(Fallo,Nutrients)
summarize(X, Mean=mean(Total), SD=sd(Total))
```

```
## # A tibble: 2 x 3
##   Nutrients  Mean    SD
##   <chr>     <dbl> <dbl>
## 1 high       58.0  9.61
## 2 low        49.9  8.66
```

Now imagine if our sampling design is 'unbalanced'. For example, maybe we had some plant mortality or lost some plants to a tornado. If one of the two species in the *Taxon* column had more high-nutrient pots, then it would have a higher mean. BUT, the higher mean is not an effect of the Taxon, but is simply due to the unbalanced nature of the design. We can simulate this effect by re-shuffling the species names:


```r
RFallo<-Fallo
set.seed(256)
RFallo$Taxon<-rbinom(nrow(RFallo),size=1,prob=0.7)

X<-group_by(RFallo,Taxon)
summarize(X, Mean=mean(Total))
```

```
## # A tibble: 2 x 2
##   Taxon  Mean
##   <int> <dbl>
## 1     0  56.1
## 2     1  56.5
```

To fix this problem, we may want to take a *weighted mean*:


```r
X1<-group_by(RFallo,Taxon,Scenario,Nutrients)
Y1<-summarize(X1,Mean=mean(Total))
```

```
## `summarise()` has grouped output by 'Taxon', 'Scenario'. You can override using the `.groups` argument.
```

```r
X2<-group_by(Y1,Taxon,Scenario)
Y2<-summarize(X2,Mean=mean(Mean))
```

```
## `summarise()` has grouped output by 'Taxon'. You can override using the `.groups` argument.
```

```r
X3<-group_by(Y2,Taxon)
Y3<-summarize(X3, Mean=mean(Mean))
arrange(Y3,desc(Mean))
```

```
## # A tibble: 2 x 2
##   Taxon  Mean
##   <int> <dbl>
## 1     1  56.6
## 2     0  55.8
```

### `%>%` (pipe)

The combination `%>%` is called 'pipe' for short. Be careful though -- in Unix, 'pipe' is slightly different and uses the vertical line (often shift-backslash): `|`

The pipe is useful to combine operations without creating a whole bunch of new objects. This can save on memory use. 

For example, we can re-write the weighted mean example using pipes:


```r
RFallo %>% 
  group_by(Taxon,Scenario,Nutrients) %>% 
  summarize(Mean=mean(Total)) %>% 
  group_by(Taxon,Scenario) %>% 
  summarize(Mean=mean(Mean)) %>% 
  group_by(Taxon) %>% 
  summarize(Mean=mean(Mean)) %>% 
  arrange(desc(Mean))
```

```
## `summarise()` has grouped output by 'Taxon', 'Scenario'. You can override using the `.groups` argument.
```

```
## `summarise()` has grouped output by 'Taxon'. You can override using the `.groups` argument.
```

```
## # A tibble: 2 x 2
##   Taxon  Mean
##   <int> <dbl>
## 1     1  56.6
## 2     0  55.8
```

This also avoids potential for bugs in our program. Imagine if we mis-spelled 'Taxon' in our second line by accidentialy pressing 's' along with 'x'. Compare the output:


```r
X<-group_by(Fallo,Taxon,Scenario,Nutrients)
X<-group_by(X,Tasxon,Scenario)
```

```
## Error: Must group by variables found in `.data`.
## * Column `Tasxon` is not found.
```

```r
X<-group_by(X,Taxon)
X<-summarize(X, Mean=mean(Total), SD=sd(Total))
arrange(X,desc(Mean))
```

```
## # A tibble: 2 x 3
##   Taxon  Mean    SD
##   <chr> <dbl> <dbl>
## 1 japon  56.4 10.4 
## 2 bohem  56.3  9.54
```


```r
Fallo %>% 
  group_by(Taxon,Scenario,Nutrients) %>%
  group_by(Tasxon,Scenario) %>%
  group_by(Taxon) %>%
  summarize(Mean=mean(Total), SD=sd(Total)) %>%
  arrange(desc(Mean))
```

```
## Error: Must group by variables found in `.data`.
## * Column `Tasxon` is not found.
```

In both cases we get an error, but in one case we still calculate the means and sd of the two species. 

> A bug that produces no output is much less dangerous than an error that gives an output. Why?


## Missing Data

So far we have worked on a pristine data set that has already been edited for errors. More often datasets will contain missing values.

### `NA` and `na.rm()`

The R language uses a special object `NA` to denote missing data. 


```r
Vec<-c(1,2,3,NA,5,6,7)
Vec
```

```
## [1]  1  2  3 NA  5  6  7
```

When a function is run on a vector or other object containing NA, the function will often return NA or give an error message:


```r
mean(Vec)
```

```
## [1] NA
```

This is by design, because it is not always clear what NA means. Many functions in R include an na.rm parameter that is set to FALSE by default. Setting it to true tells the function to ignore the NA


```r
mean(Vec, na.rm=T)
```

```
## [1] 4
```

### `NA` vs `0`

A common mistake students make is to put 0 for missing data. This can be a big problem when analyzing the data since the calculations are very different.


```r
Vec1<-c(1,2,3,NA,5,6,7)
mean(Vec1, na.rm=T)
```

```
## [1] 4
```

```r
Vec2<-c(1,2,3,0,5,6,7)
mean(Vec2, na.rm=T)
```

```
## [1] 3.428571
```

### `is.na()`

In large datasets you might want to check for missing values. Let's simulate this in our *FallopiaData.csv* dataset. 

To set up a test dataset, randomly select 10 rows and replace the value for 'Total' with NA. The `sample` function is a 


```r
X<-round(runif(10,min=1,max=nrow(Fallo)),0)
Fallo$Total[X]<-NA
Fallo$Total
```

```
##   [1] 67.06 50.22 61.08 41.71 41.81 48.27 55.42 42.68    NA 45.89 59.02 57.66
##  [13] 48.98 35.97 43.28 52.27 45.92 44.61 59.13 58.97 55.36 31.46 43.46 44.65
##  [25] 59.69 60.82 57.21 34.09 58.57 66.74 63.18    NA 54.09 55.27 61.31 53.56
##  [37] 52.66 64.71 61.06 45.34 64.20 57.50 68.55 49.55 56.70 54.06 66.60 74.82
##  [49] 53.71 49.75 58.45 66.06 67.01 70.41    NA 63.43 77.05 47.50 61.79 54.96
##  [61] 48.99 52.01    NA 57.18 42.47 46.18 62.56 54.36 69.54 75.91 56.34 64.97
##  [73] 60.71 57.80 41.72 67.44 58.78    NA    NA 58.42 55.35    NA 55.04 39.56
##  [85] 71.07 45.23 57.20 67.70 52.46 60.86    NA 65.53 48.19 60.89 48.13 60.37
##  [97] 67.86 56.40 49.13 56.11 49.78 69.00 65.40 50.73 63.08 60.93    NA 49.12
## [109] 68.73 31.90 69.88 69.48 47.88 51.42 58.13 50.51 54.83 66.80 50.31 56.12
## [121] 62.96 78.80 64.25
```

Use `is.na()` to check for missing values:


```r
is.na(Fallo$Total)
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
##  [13] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [25] FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE
##  [37] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [49] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
##  [61] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [73] FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE  TRUE FALSE FALSE
##  [85] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
##  [97] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
## [109] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [121] FALSE FALSE FALSE
```

Note that the output is a vector of True/False. Each cell corresponds to a value of 'Total' with TRUE indicating missing values. This is an example of a boolean variable, which has some handy properties in R. 

First, we can use it as an index. For example, let's see which pots have missing 'Total' values:


```r
Missing<-is.na(Fallo$Total)
Fallo$PotNum[Missing]
```

```
## [1]  10  39  68  78  95  96 100 111 129
```

Another handy trick to count missing values is:


```r
sum(is.na(Fallo$Total))
```

```
## [1] 9
```

This takes advantage of the fact that the boolean TRUE/FALSE variable is equivalent to the binary 1/0 values.

## Naughty Data

Naughty data contain the same information as a standard row x column (i.e. 2-dimensional) dataset but break the rules outlined above:

1. Each cell contains a single value
2. Each variable must have its own column
3. Each observation must have its own row

Examples of Naughty Data are shown in Figure 2A of the [Wu et al. manuscript](https://www.biorxiv.org/content/early/2018/10/30/457051). :

![](./Wu_Fig2.jpg)

Naughty data can be very time consuming to fix, but regular expressions can make this a bit easier (see the [Regex tutorial](https://colauttilab.github.io/RCrashCourse/4_regex.html)).








