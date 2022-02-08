# Working with categorical data

## Introduction

Creating and manipulating categorical data sets requires some skills and techniques in R beyond those ordinarily used for quantitative data. This chapter illustrates these for the main formats for categorical data: case form, frequency form and table form.

> The chapter uses many base R commands I am not familiar. But I believe that most of these command could be translated to the tidy data approach. The praxis will show, if I have the skills for these transformations. In this chapter I will focus on those command where I lack knowledge of their function either in base R or tidy approach.

> For example there is a focus on matrices and arrays, data structures I am not comfortable with. But it should be easy to convert them to data frames or tibbles. Similiar with the graphic command `plot()` which I will replace with `ggplot().`

## Forms of categorical data

> The book distinguishes between case, frequency and table form. I believe that this differentiation is with the tidy approach not essential anymore. More important nowadays is the conversion of data from wide to long with `pivot_longer()` and vice versa with `pivot_wider()`. But again: Practical challenges will be the proof of my assessment.


### Using tabulizer

Instead of entering the data manually or importing from a file or website, I was experimenting with the **{tabulizer}** package. 'Tabula' is a Java library designed to computationally extract tables from PDF documents. 

Sometimes there are problems with the employment as **{rJava}** is required and complicated to install. But I had it already installed and cant remember any problems. **{tabulizerjars}** is also necessary. The only purpose of **{tabulizerjars}** is to distribute releases of the 'Tabula' Java library to be used with the **{tabulizer}** package. Note that the package itself does not provide any functionality apart from basic linking to Java version installed on the system.

See the [Tabula](https://tabula.technology/) webpage but first and foremost the [rOpenSci web page on tabulizer](https://docs.ropensci.org/tabulizer/index.html) for more details and documentation of all the features.

The main function is `extract_tables()` with several parameters, for instance the page for the table to extracted. But in my case to provide the page resulted in the extraction of another table at the beginning of the page. Therefore I had to use the interactive `locate_areas()` function. The program stops and displays a mini picture of the desired PDF page where one can select the area with the table.



```r
library(tidyverse)
library(tabulizer)
library(tabulizerjars)

f <- normalizePath("test-data/DDAR.pdf")
tab_area <- locate_areas(file = f, page = 53)
saveRDS(tab_area, "test-data/example-tab-area")
```


```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(tabulizer)
library(tabulizerjars)

f <- normalizePath("test-data/DDAR.pdf")
tab_area <- readRDS("test-data/example-tab-area")
df <- extract_tables(file = f,
                    page = 53, 
                    area = tab_area,
                    guess = FALSE,
                    output = "data.frame")
df <- tibble(df[[1]])
colnames(df) <- as.character(df[1, ])
df <- df[-1,]
df <- pivot_longer(df, 2:4, names_to = "party", values_to = "count")
df
```

```
## # A tibble: 6 × 3
##   sex    party count
##   <chr>  <chr> <chr>
## 1 female dem   279  
## 2 female indep 73   
## 3 female rep   225  
## 4 male   dem   165  
## 5 male   indep 47   
## 6 male   rep   191
```


### Generating tables

With data in case form or frequency form, you can generate frequency tables from factor variables in data frames using the `table()` function; for tables of proportions, use the `prop.table()` function, and for marginal frequencies (summing over some variables) use `margin.table()`.

::: {.todobox}
> I have to look up other books using modern approaches to see what functions they use, e.g., [ModernDive](https://moderndive.netlify.app/index.html) (Statistical Inference via Data Science -- A ModernDive into R and the Tidyverse). I believe the **{janitor}** packages may be helpful for `prop.table()` and `margin.table()` functionalities.
:::

### Printing tables

For 3-way and larger tables, the functions `ftable()` (in the **{stats}** package) and `structable()` (in **{vcd}**) provide a convenient and flexible tabular display in a “flat” (2-way) format.

### Collapsing tables

It sometimes happens that we have a data set with more variables or factors than we want to analyze. The book recommends `aggregate()` in {stats}, `margin.table()` resp. `marginSums()`, `apply()` both in {base}, and collapse.table()` from the **{vcdExtra}** package.

::: {.todobox}
> Again I have to find equivalents. I believe that the modern approach works with **{dplyr}**, **{tidyr}**, **{purrr}**, **{tibble}** and **{forcats}** (all provided by the **{[tidyverse](https://tidyverse.tidyverse.org/)}** package suite.)
:::

### Converting tables

A given contingency table can be represented equivalently in case form, frequency form, and table form. However, some R functions were designed for one particular representation. Therefore converting tables from one form to another, are critical.

In addition to the already mentioned functions of `table()`, `xtabs()` and `as.data.frame()` the book references the `expand.dft()` function in **{vcdExtra}**. For producing a LateX table the **{xtable}** package is recommended.

> Am not sure if the tidyverse() packages solves these conversion problems. But to know more about the requirements I will print the output from different table commands.


I am going to use for this task the `UCBAdmissions` data from the base **{datasets}** package. For shorter reference I will copy it to `UCB.`

**UCBAdmissions**:The original structure of the table:


```r
(UCB <- UCBAdmissions)
```

```
## , , Dept = A
## 
##           Gender
## Admit      Male Female
##   Admitted  512     89
##   Rejected  313     19
## 
## , , Dept = B
## 
##           Gender
## Admit      Male Female
##   Admitted  353     17
##   Rejected  207      8
## 
## , , Dept = C
## 
##           Gender
## Admit      Male Female
##   Admitted  120    202
##   Rejected  205    391
## 
## , , Dept = D
## 
##           Gender
## Admit      Male Female
##   Admitted  138    131
##   Rejected  279    244
## 
## , , Dept = E
## 
##           Gender
## Admit      Male Female
##   Admitted   53     94
##   Rejected  138    299
## 
## , , Dept = F
## 
##           Gender
## Admit      Male Female
##   Admitted   22     24
##   Rejected  351    317
```


**ftable()**:


```r
ftable(UCB)
```

```
##                 Dept   A   B   C   D   E   F
## Admit    Gender                             
## Admitted Male        512 353 120 138  53  22
##          Female       89  17 202 131  94  24
## Rejected Male        313 207 205 279 138 351
##          Female       19   8 391 244 299 317
```

**ftable with formula method**: 


```r
# ftable(UCB, row.vars = 1:2)      # same result
ftable(Admit + Gender ~ Dept, data = UCB)   # formula method
```

```
##      Admit  Admitted        Rejected       
##      Gender     Male Female     Male Female
## Dept                                       
## A                512     89      313     19
## B                353     17      207      8
## C                120    202      205    391
## D                138    131      279    244
## E                 53     94      138    299
## F                 22     24      351    317
```

**as_tibble()**: 


```r
as_tibble(UCB)
```

```
## # A tibble: 24 × 4
##    Admit    Gender Dept      n
##    <chr>    <chr>  <chr> <dbl>
##  1 Admitted Male   A       512
##  2 Rejected Male   A       313
##  3 Admitted Female A        89
##  4 Rejected Female A        19
##  5 Admitted Male   B       353
##  6 Rejected Male   B       207
##  7 Admitted Female B        17
##  8 Rejected Female B         8
##  9 Admitted Male   C       120
## 10 Rejected Male   C       205
## # … with 14 more rows
```

**structable()**:


```r
library(vcd)
```

```
## Loading required package: grid
```

```r
structable(UCB)
```

```
##               Gender Male Female
## Admit    Dept                   
## Admitted A            512     89
##          B            353     17
##          C            120    202
##          D            138    131
##          E             53     94
##          F             22     24
## Rejected A            313     19
##          B            207      8
##          C            205    391
##          D            279    244
##          E            138    299
##          F            351    317
```

> It seems to me that all the complex discussion about different table formats is now outdated with **{tibble}** from the **{tidyverse}** approach. The same is true with **{dplyr}** concerning subsetting, filtering or other transforming activities. This is especially important for manipulating factor levels where the **{forcats}** packages replaces all the different function for aggregating and collapsing. But to learn & decide what code is necessary I would need the actual problem. Therefore I just skimmed chapter 2.

## A complex example

> A good conversion exercise is the following complex example on TV viewing data. So I can check if I am able to provide the necessary R code to fulfil the requirements. I have to inspect it line per line and relace it with working code. To see the difference I will keep the old code as comments. 

> (I will do this another time… -- I have to separate the different steps with subtitles and comments about my changes.)


```r
### Check with the original example, TV viewing data, p. 58ff.
library(vcdExtra)
## reading in the data
tv_data <- TV
str(tv_data)
head(tv_data, 5)

## tv_data <- read.table("C:/R/data/tv.dat")

## tv_data <- read.table(file.choose())

## creating factors within the data frame
TV_df <- tv_data
colnames(TV_df) <- c("Day", "Time", "Network", "State", "Freq")
TV_df <- within(TV_df, {
           Day <- factor(Day,
                         labels = c("Mon", "Tue", "Wed", "Thu", "Fri"))
           Time <- factor(Time)
           Network <- factor(Network)
           State <- factor(State)
	 })

## reshaping the table into a 4-way table
TV <- array(tv_data[,5], dim = c(5, 11, 5, 3))
dimnames(TV) <-
    list(c("Mon", "Tue", "Wed", "Thu", "Fri"),
         c("8:00", "8:15", "8:30", "8:45", "9:00", "9:15",
           "9:30", "9:45", "10:00", "10:15", "10:30"),
         c("ABC", "CBS", "NBC", "Fox", "Other"),
         c("Off", "Switch", "Persist"))
names(dimnames(TV)) <- c("Day", "Time", "Network", "State")

## Creating the table using xtabs()
TV <- xtabs(V5 ~ ., data = tv_data)
dimnames(TV) <-
    list(Day = c("Mon", "Tue", "Wed", "Thu", "Fri"),
         Time = c("8:00", "8:15", "8:30", "8:45", "9:00", "9:15",
                  "9:30", "9:45", "10:00", "10:15", "10:30"),
         Network = c("ABC", "CBS", "NBC", "Fox", "Other"),
         State = c("Off", "Switch", "Persist"))

### SECTION ### 2.9.2. Subsetting and collapsing

## subsetting data
TV <- TV[,,1:3,]     # keep only ABC, CBS, NBC
TV <- TV[,,,3]       # keep only Persist -- now a 3 way table
structable(TV)

## collapsing time labels
TV2 <- collapse.table(TV,
                      Time = c(rep("8:00-8:59", 4),
                               rep("9:00-9:59", 4),
			       rep("10:00-10:44", 3)))
structable(Day ~ Time + Network, TV2)
```

