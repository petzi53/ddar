# Working with categorical data

## Introduction

Creating and manipulating categorical data sets requires some skills and techniques in R beyond those ordinarily used for quantitative data. This chapter illustrates these for the main formats for categorical data: case form, frequency form, and table form.

> The chapter uses many base R commands I am not familiar with. But I believe that most of these commands could be translated to the tidy data approach. The praxis will show if I have the skills for these transformations. This chapter will focus on those commands where I lack knowledge of their functionality. 

> For example, there is a focus on tables, matrices, and arrays – data structures I am not comfortable with. But it should be easy to convert them to data frames or tibbles. There is a similar situation with the graphic command `plot()`, which should be replaced with `ggplot().`

## Forms of categorical data

> The book distinguishes between case, frequency, and table form. I believe that this differentiation is with the tidy approach not essential anymore. Nowadays, more critical is converting data from wide to long with `pivot_longer()` and vice versa with `pivot_wider()`. But again: Practical challenges will be the proof of my assessment.


### Using tabulizer

Instead of entering the data manually or importing it from a file or website, I was experimenting with the **{tabulizer}** package. 'Tabula' is a Java library designed to extract tables from PDF documents computationally. 

Sometimes there are problems with the employment as **{rJava}** is required and complicated to install. But I had it already installed and can't remember any issues. **{tabulizerjars}** is also necessary. The only purpose of **{tabulizerjars}** is to distribute releases of the 'Tabula' Java library to be used with the **{tabulizer}** package. Note that **{tabulizerjars}** itself does not provide any functionality apart from linking to the Java version installed on the system.

See the [Tabula](https://tabula.technology/) webpage but first and foremost the [rOpenSci web page on tabulizer](https://docs.ropensci.org/tabulizer/index.html) for more details and documentation of all the features.

The primary function is `extract_tables()` with several parameters, for instance, the page for the table to be extracted. But in my case, providing the page number pulled another table at the beginning of the page. Therefore I had to use the interactive `locate_areas()` function. The program stops and displays a mini picture of the desired PDF page to select the area with the table.



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
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
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

It sometimes happens that we have a data set with more variables or factors than we want to analyze. The book recommends `aggregate()` in {stats}, `margin.table()` resp. `marginSums()`, `apply()` (both in **{base}**), and `collapse.table()` from the **{vcdExtra}** package.

::: {.todobox}
> Again I have to find equivalents. I believe that the modern approach works with **{dplyr}**, **{tidyr}**, **{purrr}**, **{tibble}** and **{forcats}** (all provided by the **{[tidyverse](https://tidyverse.tidyverse.org/)}** package suite.)
:::

### Converting tables

A given contingency table can be represented equivalently in case, frequency, and table forms. However, some R functions were designed for one particular representation. Therefore converting tables from one structure to another is critical.

In addition to the already mentioned functions of `table()`, `xtabs()` and `as.data.frame()` the book references the `expand.dft()` function in **{vcdExtra}**. For producing a LateX table the **{xtable}** package is recommended.

> Am not sure if the packages of the **{tidyverse}** meta-package solve these conversion problems. But to know more about the requirements, I will print the output from different table commands. I will not use one of the sophisticated table formating and print commands but will apply just the implemented printing method of the referenced table command.


I am going to use for this task the `UCBAdmissions` data from the base **{datasets}** package. For a shorter later reference, I will copy it to `UCB.`

#### table


```r
(UCB <- UCBAdmissions) # same as:  xtabs(Freq ~ ., data = UCB)
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


#### ftable()

`ftable()` creates ‘flat’ contingency tables. They contain the counts of each combination of the levels of the variables (factors) involved. This information is then re-arranged as a matrix whose rows and columns correspond to unique combinations of the levels of the row and column variables (as specified by `row.vars` and `col.vars`, respectively). 

These combinations are created by looping over the variables in reverse order so that levels of the left-most variable vary the slowest. Displaying a contingency table in this flat matrix form is often preferable to showing it as a higher-dimensional array.

`ftable()` returns an object of class "ftable", which is a matrix with counts of each combination of the levels of variables with information on the names and levels of the (row and columns) variables stored as attributes "row.vars" and "col.vars".




```r
ftable(UCB, row.vars = 1:2)
##                 Dept   A   B   C   D   E   F
## Admit    Gender                             
## Admitted Male        512 353 120 138  53  22
##          Female       89  17 202 131  94  24
## Rejected Male        313 207 205 279 138 351
##          Female       19   8 391 244 299 317
```

#### ftable() with formula

The left and right-hand sides of the formula specify the column and row variables, respectively, of the flat contingency table to be created. Only the `+` operator is allowed for combining the variables. A `.` may be used once in the formula to indicate the inclusion of all the remaining variables.

If data is an object of class "table" or an array with more than 2 dimensions, it is taken as a contingency table, and hence all entries should be nonnegative. Otherwise, if it is not a flat contingency table (i.e., an object of class "ftable"), it should be a data frame or matrix, list, or environment containing the variables to be cross-tabulated.

The result is a flat contingency table that contains the counts of each combination of the levels of the variables, collapsed into a matrix for suitably displaying the counts.


```r
ftable(Admit + Gender ~ Dept, data = UCB)
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

#### xtabs() {#xtabs}

The `xtabs()` function allows you to create cross-tabulations of data using formula-style input. This typically works with case- or frequency-form data supplied in a data frame or a matrix. The result is a contingency table in array format, whose dimensions are determined by the formula's terms on the right side.


```r
xtabs(Freq ~ Gender + Admit, data = UCB)
##         Admit
## Gender   Admitted Rejected
##   Male       1198     1493
##   Female      557     1278
```


#### structable()

`structable()` produces a 'flat' representation of a high-dimensional contingency table constructed by recursive splits (similar to the construction of mosaic displays). The result is a textual representation of mosaic displays and thus 'flat' contingency tables. The formula interface is similar to `ftable()` but also accepts the mosaic-like formula interface (empty left-hand side). 

The function results in an object of class "structable", inheriting from class "ftable", with the splitting information ("split_vertical") as an additional attribute.



```r
vcd::structable(UCB)
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

#### tibble()

> The text of this section is derived from the help files and vignette of the **{tibble}** packages. 

`tibble()` is a nice way to create data frames. A tibble is a data frame with class `tbl_df`, a subclass of `data.frame`, designed to have different default behavior. Tibble is the central data structure for the set of packages known as the **{[tidyverse](https://www.tidyverse.org/)}**.

To complement `tibble()`, the **{tibble}** package provides `as_tibble()` to coerce an existing object, such as a data frame or matrix, into tibbles . A tibble encapsulates best practices for data frames:

- It never changes an input’s type (i.e., no more `stringsAsFactors = FALSE`!). This makes it easier to use with list columns. List-columns are often created by `tidyr::nest()`, but they can be useful to create by hand.
- It never adjusts the names of variables.
- It evaluates its arguments lazily and sequentially.
- It never uses `row.names()`. The whole point of tidy data is to store variables in a consistent way. So it never stores a variable as a special attribute.
- It only recycles vectors of length 1. This is because recycling vectors of greater lengths are a frequent source of bugs.



```r
(as_tibble(UCB))
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


> It seems to me that all the complex discussion about different table formats is now outdated with the **{tibble}** data structure from the **{tidyverse}** approach. The same is true with **{dplyr}** concerning subsetting, filtering, or other transforming activities. This is especially important for manipulating factor levels where the **{forcats}** packages replace all the different functions for aggregating and collapsing. But to learn & decide what code is necessary, I would need the actual problem. Therefore I just skimmed chapter 2.

## A complex example

> A good conversion exercise is the following complex example on TV viewing data. I will check if I can provide the necessary R code to fulfill the requirements. I have to inspect it line per line and replace it with working code from the newer tidyverse approach. To see the difference, I will keep the old code and describe the various data-wrangling actions.


### Dataset description

Are you ready for a more complicated example that puts together various skills developed in this chapter? These skills are 

(a) reading raw data, 
(b) creating tables, 
(c) assigning level names to factors and 
(d) collapsing levels or variables for use in the analysis. 

To illustrate these steps, we use the dataset `tv.dat`, supplied with the initial implementation of mosaic displays. They were derived from an early, compelling example of mosaic displays that illustrated the method with data on a large sample of TV viewers whose behavior had been recorded for the Neilsen ratings. This data set contains sample television audience data from Neilsen Media Research for the week starting November 6, 1995. 

The data file, `tv.dat`, is stored in frequency form with 825 rows and 5 columns. There is no header line in the file, so when we use `read.table()` below, the variables will be named `V1 – V5`. This data represents a 4-way table of size $5 × 11 × 5 × 3 = 825$ where the table variables are `V1 – V4`, and the cell frequency is read as `V5`. 

The table variables are: 

- `V1` --- values $1:5$ correspond to the days Monday–Friday; 
- `V2` --- values $1:11$ correspond to the quarter-hour times $8:00$ pm through $10:30$ pm; 
- `V3` --- values $1:5$ correspond to ABC, CBS, NBC, Fox, and non-network choices; 
- `V4` --- values $1:3$ correspond to transition states: turn the television Off, Switch channels, or Persist in viewing the current channel.


### Package dataset {#package-dataset}

There is a `TV` dataset in the **{vcdExtra}** package. To load it would be the easiest way to get the data. In that case, you would not have to worry about data transformation because the dataset is already in the desired form.


```r
library(vcdExtra)
## Loading required package: vcd
## Loading required package: grid
## Loading required package: gnm
## 
## Attaching package: 'vcdExtra'
## The following object is masked from 'package:dplyr':
## 
##     summarise

data(TV)  # the most straightforward way, does not need data wrangling
TV
## , , Network = ABC
## 
##            Time
## Day         8:00 8:15 8:30 8:45 9:00 9:15 9:30 9:45 10:00 10:15 10:30
##   Monday     146  151  156   83  325  350  386  340   352   280   278
##   Tuesday    244  181  231  205  385  283  345  192   329   351   364
##   Wednesday  233  161  194  156  339  264  279  140   237   228   203
##   Thursday   174  183  197  181  187  198  211   86   110   122   117
##   Friday     294  281  305  239  278  246  245  138   246   232   233
## 
## , , Network = CBS
## 
##            Time
## Day         8:00 8:15 8:30 8:45 9:00 9:15 9:30 9:45 10:00 10:15 10:30
##   Monday     337  293  304  233  311  251  241  164   252   265   272
##   Tuesday    173  180  184  109  218  235  256  250   274   263   261
##   Wednesday  158  126  207   59   98  103  122   86   109   105   110
##   Thursday   196  185  195  104  106  116  116   47   102    84    84
##   Friday     130  144  154   81  129  153  136  126   138   136   152
## 
## , , Network = NBC
## 
##            Time
## Day         8:00 8:15 8:30 8:45 9:00 9:15 9:30 9:45 10:00 10:15 10:30
##   Monday     263  219  236  140  226  235  239  246   279   263   283
##   Tuesday    315  254  280  241  370  214  195  111   188   190   210
##   Wednesday  134  146  166   66  194  230  264  143   274   289   306
##   Thursday   515  463  472  477  590  473  446  349   649   705   747
##   Friday     195  220  248  160  172  164  169   85   183   198   204
str(TV)
##  int [1:5, 1:11, 1:3] 146 244 233 174 294 151 181 161 183 281 ...
##  - attr(*, "dimnames")=List of 3
##   ..$ Day    : chr [1:5] "Monday" "Tuesday" "Wednesday" "Thursday" ...
##   ..$ Time   : chr [1:11] "8:00" "8:15" "8:30" "8:45" ...
##   ..$ Network: chr [1:3] "ABC" "CBS" "NBC"
```
'TV' data set comprises a 5 x 11 x 3 contingency table. But this is not the original dataset described under the subsection \@ref(package-dataset).

"The original data, tv.dat, contains two additional networks: "Fox" and "Other", with small frequencies. These levels were removed in the current version. There is also a fourth factor, transition State transition (turn the television Off, Switch channels, or Persist in viewing the current channel). The TV data here includes only the Persist observations." (From the TV **{vcdExtra}** help file.)


We, therefore, will go the hard way and import the `tv.dat` file as mentioned in the book. But viewing the above dataset gives you a good impression of how the data should look at the end of the data wrangling process.



::: {.bluebox}
The **b**ook **v**ersion contains` bv-` in the chunk- and variable names. In contrast to my own version, which has my initials `pb-` in their designations.
:::


### bv-import

**1. Step**: The provided R code in the [ch02.R file](http://ddar.datavis.ca/pages/Rcode/ch02.R) does not work. The book referenced the file `tv.dat` to the `doc/extdata` directory of **{vcdExtra}**. On my (macOS) installation `tv.dat` is indeed inside `extdata`, but `extdata` is not a subdirectory of `doc`.

You can also use the RStudio interactive menu: "File -> Import Dataset -> From text (base) …".

**2. Step**: In the next step we use `xtabs()` to do the cross-tabulation, using $V5$ as the frequency variable. `xtabs()` uses a formula interface as demonstrated in section \@ref(xtabs).

**3. Step**: The third step attaches names to the factors. There is no assignment necessary, but the list has to be ordered.



```r
tv_bv <-
    read.table(system.file("extdata", "tv.dat",   # without "doc" directory
                           package = "vcdExtra"))

tv_bv <- xtabs(V5 ~ ., data = tv_bv)

dimnames(tv_bv) <-
    list(
        Day = c("Mon", "Tue", "Wed", "Thu", "Fri"), 
        Time = c(
            "8:00",
            "8:15",
            "8:30",
            "8:45",
            "9:00",
            "9:15",
            "9:30",
            "9:45",
            "10:00",
            "10:15",
            "10:30"
        ),
        Network = c("ABC", "CBS", "NBC", "Fox", "Other"),
        State = c("Off", "Switch", "Persist")
    )

tv_bv <- as.data.frame(tv_bv, dim = c(5, 11, 5, 3))

str(tv_bv)
## 'data.frame':	825 obs. of  5 variables:
##  $ Day    : Factor w/ 5 levels "Mon","Tue","Wed",..: 1 2 3 4 5 1 2 3 4 5 ...
##  $ Time   : Factor w/ 11 levels "8:00","8:15",..: 1 1 1 1 1 2 2 2 2 2 ...
##  $ Network: Factor w/ 5 levels "ABC","CBS","NBC",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ State  : Factor w/ 3 levels "Off","Switch",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Freq   : int  6 18 6 2 11 6 29 25 17 29 ...
head(tv_bv, 5)
##   Day Time Network State Freq
## 1 Mon 8:00     ABC   Off    6
## 2 Tue 8:00     ABC   Off   18
## 3 Wed 8:00     ABC   Off    6
## 4 Thu 8:00     ABC   Off    2
## 5 Fri 8:00     ABC   Off   11
```




### pb-import



**1. Step**: At first I tried to use the `read_table()` function from the **{readr}** package to import the data into a [tibble](https://tibble.tidyverse.org/). But this didn't work, as I always got an additional column of logical type full with `NA's, and the first row was not imported. After I changed to the free width format `read_fwf()` of the same package, it worked. In the same step, I named the columns and defined the data type of the variables.

The import result is a tibble, a modern take on data frames. It encapsulates best practices for data frames and drops the features that used to be convenient but are now frustrating (i.e., converting character vectors to factors). I also used its superior printing method by using `glimpse()` instead of `str()`.

::: {.warningbox}
Watch the difference between `bv` and `pb` version: The function `read.table()` (with a period) in `bv` is called from the **{base}** package, whereas `read_table()` (with an underscore) in `pb` is part of **{readr}** and has to be loaded as a library.
:::


**2.Step**: In the next step I recoded all factor labels with `fct_recode()` from the **{forcats}** package. I overwrote their old values with `mutate()` from the **{dplyr}** package.

I linked all commands of the sequence with the [pipe operator](https://magrittr.tidyverse.org/) `%>%` using a different appearance |>  with a particular font. All the mentioned packages --- including the pipe operator --- are part of the **{tidyverse}** meta-package, which has to be loaded.





```r
library(tidyverse)

tv_pb <-
    read_fwf(system.file("extdata", "tv.dat",   # without the "doc" directory
                package = "vcdExtra"),
        col_types = 'ffffi',
        fwf_widths(c(2, 2, 2, 2, 2),
               col_names = c("Day", "Time", "Network", "State", "Freq")
    )) |>
    mutate(Day = fct_recode(
        Day,
        Mon = "1",
        Tue = "2",
        Wed = "3",
        Thu = "4",
        Fri = "5"
    )) |>
    mutate(
        Time = fct_recode(
            Time,
            "8:00" = "1",
            "8:15" = "2",
            "8:30" = "3",
            "8:45" = "4",
            "9:00" = "5",
            "9:15" = "6",
            "9:30" = "7",
            "9:45" = "8",
            "10:00" = "9",
            "10:15" = "10",
            "10:30" = "11"
        )
    ) |>
    mutate(Network = fct_recode(
        Network,
        "ABC" = "1",
        "CBS" = "2",
        "NBC" = "3",
        "Fox" = "4",
        "Other" = "5"
    )) |>
    mutate(State = fct_recode(
        State,
        "Off" = "1",
        "Switch" = "2",
        "Persist" = "3"
    ))

glimpse(tv_pb)
## Rows: 825
## Columns: 5
## $ Day     <fct> Mon, Tue, Wed, Thu, Fri, Mon, Tue, Wed, Thu, Fri, Mon, Tue, We…
## $ Time    <fct> 8:00, 8:00, 8:00, 8:00, 8:00, 8:15, 8:15, 8:15, 8:15, 8:15, 8:…
## $ Network <fct> ABC, ABC, ABC, ABC, ABC, ABC, ABC, ABC, ABC, ABC, ABC, ABC, AB…
## $ State   <fct> Off, Off, Off, Off, Off, Off, Off, Off, Off, Off, Off, Off, Of…
## $ Freq    <int> 6, 18, 6, 2, 11, 6, 29, 25, 17, 29, 10, 10, 12, 8, 7, 20, 24, …
head(tv_pb, 5)
## # A tibble: 5 × 5
##   Day   Time  Network State  Freq
##   <fct> <fct> <fct>   <fct> <int>
## 1 Mon   8:00  ABC     Off       6
## 2 Tue   8:00  ABC     Off      18
## 3 Wed   8:00  ABC     Off       6
## 4 Thu   8:00  ABC     Off       2
## 5 Fri   8:00  ABC     Off      11
```

### Summary

After some troubles with the import command (using `read_fwf()` instead of `read_table()`), I had no difficulties converting the code to the newer tidyverse approach. The book version uses for re-coding the implicit order of the factor levels. The bv-code is, therefore, shorter than my pb-conversion. 
On the other hand, the tidyverse version is perhaps more neatly arranged and better understandable. But I admit that there is no advantage over the book version in this example.

::: {.successbox}
The goal was to demonstrate if I could manage to convert the book's code into a version where I do not need to use the special table commands like `ftable()`, `xtable()`, `structable()`. At least with the example of `xtable(),` I have succeeded.
:::


