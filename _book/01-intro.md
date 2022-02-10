# Introduction

::: {.greybox}
Friendly, M., & Meyer, D. (2015). Discrete Data Analysis with R:
Visualization and Modeling Techniques for Categorical and Count Data
(1st ed.). Chapman and Hall/CRC.

Short name (abbreviation): **DDAR**
:::



## Overview


"Discrete Data Analysis with R" [@Friendly_Meyer_2015]
has at its strength the interplay between analysis and visualization strategies for categorical
data. The authors drive the conviction that data analysis and
statistical graphics should go hand-in-hand in the process of
understanding and communicating statistical data. The book features the
well-known **{vcd}** package (Visualizing Categorical Data), and the books
support package **{vcdExtra}** ('vcd' Extensions and Additions), which
provides additional data sets and methods. Both packages come with large
tutorial vignettes explaining the usage. See for instance:

-   Working with categorical data with R and the vcd and vcdExtra
    packages:
    [37 pages](https://cran.r-project.org/web/packages/vcdExtra/vignettes/vcd-tutorial.pdf)
-   The Strucplot Framework: Visualizing Multi-way Contingency Tables with vcd: [48 pages](https://cran.r-project.org/web/packages/vcd/vignettes/strucplot.pdf)

The book divides statistical methods for data analysis into two different
categories:

1.  **Randomized-based methods**: Pearson chi-squared $X^2$ test,
    Fisher's exact test, and Cochran--Mantel--Haenszel tests (Chapter
    3-6) and
2.  **Model-based methods**: Logistic regression, loglinear, and
    generalized linear models (Chapter 7-9).

## What is categorical data?

Categorical variables differ in

1. the number of categories:

-   **binary variables** (or **dichotomous variables**) such as
    `Gender`: "Male," "Female"
-   **multilevel variables**, variables with more than two categories
    (called **polytomous variables**), e.g., `Fielding position` (in
    baseball), with categories "Pitcher," "Catcher," "1st base," "2nd
    base," ..., "Left field."

2. their relation:

-   **ordered categories**: e.g., `Treatment outcome`: "No improvement,"
    "Some improvement," "Marked improvement."
-   **nominal categories**: e.g., `Marital Status`: "Never married,"
    "Married," "Separated," "Divorced," "Widowed."
    
Often, we can rank some variable under a specific point of view. For instance, we could arrange the variable `side effect` in a
pharmacological study according to the seriousness of the side effects or
`party preference` according to the number of party voters in the last
election. But these kinds of classifications are somewhat arbitrary. 

::: {.infobox}
An ordinal variable should be defined as one whose categories are unambiguously ordered along a single underlying dimension. 
:::


Examples are levels of education, for instance the nine levels of the  [International Standard Classification of Education](http://uis.unesco.org/sites/default/files/documents/international-standard-classification-of-education-isced-2011-en.pdf) (ISCED): early childhood education (level 0); primary education (level 1); lower secondary education (level 2); upper secondary education (level 3); postsecondary non-tertiary education (level 4); short-cycle tertiary education (level 5); bachelor’s or equivalent level (level 6); master’s or equivalent level (level 7); doctor or equivalent level (level 8)  or the [Likert scale](https://en.wikipedia.org/wiki/Likert_scale) in survey questions ("strongly agree," "agree," "neutral," "disagree," "strongly disagree.").


### Case vs. frequency form

In many circumstances, data is recorded on each individual or
experimental unit. Data in this form is called case data, or data in
**case form**. Data in **frequency form**, by contrast, has already been
tabulated by counting over the categories of the table variables.

If observations are recorded from operationally independent experimental
units or individuals, typically a sample from some population, this
tabulated data may be called **frequency data**. However, suppose several
events or variables are observed for the same units or individuals. Those events are not operationally independent, and it is helpful to use
the term **count data** in this situation. These terms are by no means standard, but the distinction is often essential, particularly in statistical models for categorical data.

For example, in a tabulation of the number of male children within families, the number of male children in a given family would be a **count variable**, taking values 0, 1, 2, …. The number of independent families with a given number of male children is a **frequency variable**. Count data also arise when we tabulate a sequence of events over time or under different circumstances in a number of individuals.

### Univariate, bivariate, and multivariate data

Another distinction concerns the number of variables: one, two, or (potentially) many shown in a data set or table or used in some analysis.

Most statistical models distinguish between response variables (or dependent or criterion variables) and explanatory variables (or independent or predictor variables).


### Explanatory vs. response variables

- **Response Variable** (also dependent or criterion variable) is
  - either the treatment outcome (primarily quantitative), e.g., mean -> normal distribution
  - or variable of primary interest (e.g., educational achievement level predicted from socioeconomic status)
  - In the classical models, the response variable (“treatment outcome,” for example) must be considered quantitative. The model attempts to describe how the mean of the distribution of responses changes with the values or levels of the explanatory variables, such as age or gender.
  However, when the response variable is categorical, the standard linear models do not apply because they assume a normal (Gaussian) distribution for the model residuals.
- **Explanatory Variable** (independent or predictor variable)
  - experimentally manipulated, e.g., the treatment group each person is assigned to
  - or not manipulated (e.g., socioeconomic status), but used to predict another variable

::: {.infobox}
Hence, a categorical response variable generally requires analysis using methods for categorical data, but categorical explanatory variables may be readily handled by either method.
:::

## Data analysis strategies

### Hypothesis testing approaches

Many studies raise questions concerning hypotheses about the association between variables, a more general idea than that of correlation (linear association) for quantitative variables. If a non-zero association exists, we may wish to characterize the strength of the association numerically and understand the pattern or nature of the association. 

**Examples for Questions**: 

- “Is there evidence of gender bias in admission to graduate school?” Another way to frame this: 
- “Are males more likely to be admitted?”

Questions involving tests of such hypotheses are answered most easily using a large variety of specific statistical tests, often based on randomization arguments. These include 

- the familiar Pearson chi-squared test for two-way tables, 
- the Cochran–Mantel–Haenszel test statistics, 
- Fisher’s exact test, 
- and a wide range of measures of strength of association.

The hypothesis testing approach is illustrated in Chapters 4–6 of DDAR, though the emphasis is on graphical methods that help us understand the nature of the association between variables.

**Illustration of hypothesis testing**

The approach is demonstrated with the `HairEyeColor` from the base R **{datasets}** package. 

As a graphical tool, it uses

- mosaic display `mosaicplot()` from the base R **{graphics}** package. (see Chapter 5 of DDAR) and
- correspondence analysis plot ca() from the **{ca}** package (see Chapter 6 of DDAR)

For the test calculation, it uses

- `chisq.test()` from the base R **{stats}** package and
- `assocstats()` from the **{vcd}** package.

::: {.todobox}
I do not know how to apply these tools. I will postpone my reading until chapters 5 and 6.
:::


###  Model building approaches

Model-based methods provide tests of equivalent hypotheses about associations but offer additional advantages (at the cost of additional assumptions) not provided by the simpler hypotheses-testing approaches. Among these advantages, model-based methods provide estimates, standard errors, and confidence intervals for parameters and obtain predicted (fitted/expected) values with associated precision measures.


**Illustration of model building**

The book presents for demonstration logistic regression of dichotomous response variables. It illustrates the approach with two important real-world examples:

- Space shuttle disaster: dataset `SpaceShuttle` from **{vcd}**.
- Survivals of the Donner party (1846/47 Sierra Nevada): dataset `Donner` from **{vcdExtra}**.

::: {.todobox}
Again I do not understand fully how the model building is done. I am looking forward to reading chapter 7 of DDAR.
:::





## Graphical methods

### Visualization recommendations

This book section presents several advantages of data visualization. Note that data analysis and graphing are iterative processes: Visualization = Graphing + Fitting + Graphing + Fitting + Graphing …

1. **Presentation vs. Exploration**: Different communication purposes require different graphs. There is, for instance, a big [difference between graphs for presentation and for data exploring](http://www.theusrus.de/blog/presentation-vs-exploration/). The book's text links to [Statistical Graphs and More](http://www.theusrus.de/blog/) a blog on visualization by Martin Theus. There are --  for instance -- on the blog side column several links to interesting books by the blog author and other scientists I will mention here (but not include them in the bibliography of this book) for my further involvement with the visualization subject:

::: {.greybox}

**Some books on visualization topics**
- Cook, D., & Swayne, D. F. (2007). Interactive and Dynamic Graphics for Data Analysis.
- Theus, M., & Urbanek, S. (2019). Interactive graphics for data analysis: Principles and examples.
- Unwin, A., Theus, M., & Hofmann, H. (2016). Graphics of Large Datasets: Visualizing a million. Springer-Verlag New York.
:::


2. **Different graphical methods**: Frequencies of categorical variables are often best represented graphically using areas rather than as positions along a scale. Three examples are just illustrated with pictures. There is no R code because the images only serve as a reference for more detailed explanations in later chapters.

I will link these three examples to more detailed web pages:

- [mosaic plot](https://www.jmp.com/en_us/statistics-knowledge-portal/exploratory-data-analysis/mosaic-plot.html) (a modified bar chart), 
- [fourfold display](http://euclid.psych.yorku.ca/www/psy6136/R/output/berk-4fold.html), and 
[agreement chart](https://www.datanovia.com/en/lessons/inter-rater-agreement-chart-in-r/). 

Details on these visualization methods are provided in later chapters.

3. **Effect ordering**: Alphabetically sorted labels for ordered categories result in most of the time in nonsensical display. Therefore sort the data by the effects to be seen to facilitate comparison. This so-called "effect-order sorting" in combination with a colored representation is also possible with the data fields of the table itself.

4. **Interactive and dynamic graphs**: Graphics displayed in print form are necessarily static and fixed when designed and rendered as an image. Yet, recent developments in software, web technology, and media alternative to print have created the possibility to extend graphics in far more helpful and exciting ways for both presentation and analysis purposes. 

**Interactive graphics** allow the viewer to directly manipulate the statistical and visual components of the graphical display. These range from 

- graphical controls (sliders, selection boxes, and other widgets) to control details of analysis (e.g., a smoothing parameter) or graph (colors and other graphic details), to 
- higher-level interaction, including zooming in or out, drilling down to a data subset, linking multiple displays, selecting terms in a model, and so forth. 

The substantial effect is that the analysis and/or display is immediately re-computed and updated visually. 

In addition, **dynamic graphics** use animation to show a series of views, like frames in a movie. Adding time as an additional dimension allows far more possibilities, for example showing a rotating view of a 3D graph or showing smooth transitions or interpolations from one view to another. 

There are now many packages in R providing interactive and dynamic plots. I will only mention them here, planning to look into the details of these packages. Note that the references are from 2015, meaning that there are now even more packages providing interactive graphics and animated plots. On the other hand, some of the mentioned packages may be outdated.

### Recommended Packages

::: {.infobox}

**Packages for interactive and dynamic plots**

- [rggobi](https://www.rdocumentation.org/packages/rggobi/versions/2.1.22): A command-line interface to 'GGobi', an interactive and dynamic graphics package. 'Rggobi' complements the graphical user interface of 'GGobi', providing a way to fluidly transition between analysis and exploration, as well as automating common tasks. But the package was removed from the CRAN repository. Older version are in the [archive](https://cran.r-project.org/src/contrib/Archive/rggobi/).
-- [GGobi](http://ggobi.org/) is not an R package but an open-source visualization program for exploring high-dimensional data. It provides highly dynamic and interactive graphics such as tours and familiar graphics like the scatterplot, bar chart, and parallel coordinates plots. Plots are interactive and linked with brushing and identification. GGobi is fully documented in the GGobi book [Interactive and Dynamic Graphics for Data Analysis](http://ggobi.org/book.html). There are many references on using GGobi with R, including code examples.
- [iPlots](https://cran.r-project.org/web/packages/iplots/index.html): interactive graphics for R, [see the website](http://www.iplots.org/) for detailed information and tutorials. (Last change: June 17, 2018.)
- [rCharts](https://ramnathv.github.io/rCharts/): rCharts is an R package to create, customize and publish interactive javascript visualizations from R using a familiar lattice style plotting interface. See also the not related website [R Charts](https://r-charts.com/) with the especially interesting [ggplot2 section](https://r-charts.com/ggplot2/).
- [ggogleVis](https://cran.r-project.org/web/packages/googleVis/index.html): R interface to Google's chart tools, allowing users to create interactive charts based on data frames. Charts are displayed locally via the R HTTP help server. A modern browser with an Internet connection is required. The data remains local and is not uploaded to Google. Several vignettes demonstrate the application.
- [ggvis](https://cran.r-project.org/web/packages/ggvis/index.html): Interactive Grammar of Graphics. An implementation of an interactive grammar of graphics, taking the best parts of 'ggplot2', combining them with the reactive framework of 'shiny', and drawing web graphics using 'vega'. See website at https://ggvis.rstudio.com/.
- [animation](https://cran.r-project.org/web/packages/animation/index.html): A gallery of animations in statistics and utilities to create animations. See website https://yihui.org/animation/.
- [manipulate](https://cran.r-project.org/web/packages/manipulate/index.html): Interactive Plots for RStudio. Interactive plotting functions for use within RStudio. The manipulate function accepts a plotting expression and a set of controls (e.g. slider, picker, checkbox, or button) used to dynamically change values within the expression. The expression is automatically re-executed when a value is changed using its corresponding control, and the plot is redrawn.
:::

During searching the link of the above packages I came across two other visualization packages. I will list the for later detailed consulting:

::: {.infobox}
- [apexcharter](https://cran.r-project.org/web/packages/apexcharter/index.html): Create Interactive Chart with the JavaScript 'ApexCharts' Library. Provides an 'htmlwidgets' interface to 'apexcharts.js'. 'Apexcharts' is a modern JavaScript charting library to build interactive charts and visualizations with simple API. See 'Apexcharts' examples and documentation at <https://apexcharts.com/>.
- [chronochrt](https://cran.r-project.org/web/packages/chronochrt/index.html): Creating Chronological Charts with R. Easy way to draw chronological charts from tables, aiming to include an intuitive environment for anyone new to R. Includes'  ggplot2' geoms and theme for chronological charts. There are two vignettes: [ChronochRt](https://cran.r-project.org/web/packages/chronochrt/vignettes/ChronochRt.html) and [ChronochRt_examples](https://cran.r-project.org/web/packages/chronochrt/vignettes/Examples.html).

Additionally, I want to mention:

- [plotly](https://cran.r-project.org/web/packages/plotly/index.html): Create Interactive Web Graphics via 'plotly.js'). Create interactive web graphics from 'ggplot2' graphs and/or a custom interface to the (MIT-licensed) JavaScript library 'plotly.js' inspired by the grammar of graphics. There is also a [book on plotly](https://plotly-r.com/).

[autoplotly](https://cran.r-project.org/web/packages/autoplotly/index.html): Automatic Generation of Interactive Visualizations for Statistical Results. Functionalities to automatically generate interactive visualizations for statistical results supported by [ggfortify](https://cran.r-project.org/web/packages/ggfortify/index.html), such as time series, PCA, clustering, and survival analysis, with 'plotly.js' <https://plotly.com/> and 'ggplot2' style. The generated visualizations can also be easily extended using 'ggplot2' and 'plotly' syntax while staying interactive. See vignette [Introduction to autoplotly package](https://cran.r-project.org/web/packages/autoplotly/vignettes/intro.html).
 

:::

::: {.todobox}
1. Investigate the above-mentioned packages
2. Search for more graphic packages via the term 'graph', 'chart', or similar.

It seems that getting an overview of visualization packages would be an essential but time-consuming task. I wonder why there is no [CRAN task view](https://cran.r-project.org/web/views/) specialized in visualization.
:::

### Data und models plot


**Data plots**: 

(a) What do the data look like? 
(b) Are there unusual features? 
(c) What kinds of summaries would be helpful?


**Model plots**:

(a) What does the model "look" like? (plot predicted values); 
(b) How does the model change when its parameters change? (plot competing models); 
(c) How does the model change when the data is changed? (e.g., influence plots).

**Data + Model plots**: 

(a) How well does a model fit the data? 
(b) Does a model fit uniformly good or bad, or just good/bad in some regions? 
(c) How can a model be improved? 
(d) Model uncertainty: show confidence/prediction intervals or regions. 
(e) Data support: where is data too "thin" to make a difference in competing models?

## The 80-20-rule

1. 20% of your effort can generate 80% of your desired result in producing a given plot. (Pareto principle or 80-20 rule)
2. 80% of your effort may be required to produce the remaining 20% of a finished graph.


::: {.todobox}
In the labs part of the book, I found two interesting links:

- [The top ten worst graphs](https://www.biostat.wisc.edu/~kbroman/topten_worstgraphs/) 
- [The R Graph Gallery](https://www.r-graph-gallery.com/)

There are other R visualization gallery sites as well. Prominent to mention is here the [shiny gallery](https://shiny.rstudio.com/gallery/). I think it would pay the effort to search for other specialized pages on visualization.

:::

## Visualization of distributions

There is a short mathematical resume about the most important distribution followed by a typical visualization. The function `expand.grid()` creates a data frame with all the possible combinations of interest. The examples choose several successes out of a row of trials. With one exception, the graphs are not done with **{ggplot2}** but with lattice. But it should be easy to convert them with `ggplot()`.

I did not do it because I just wanted to get an overview of the argumentation during the first reading. Essential, however, was the understanding of `expand.grid()` where I did some experiments. 

I also checked the replacement of `with()` with the `filter()` function from the **{dplyr}** package. It is easy done but `with()` returns numeric vector whereas `filter()` returns a data frame. I believe this difference is an advantage of **{dplyr}**.


```r
with(mtcars, mpg[cyl == 8  &  disp > 350])
```

```
## [1] 18.7 14.3 10.4 10.4 14.7 19.2 15.8
```

```r
dplyr::filter(mtcars, cyl == 8 & disp > 350) |> dplyr::select(mpg)
```

```
##                      mpg
## Hornet Sportabout   18.7
## Duster 360          14.3
## Cadillac Fleetwood  10.4
## Lincoln Continental 10.4
## Chrysler Imperial   14.7
## Pontiac Firebird    19.2
## Ford Pantera L      15.8
```

