---
title: "Discrete Data Analysis with R"
author: "Peter Baumgartner"
date: "2022-02-10"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  These are my excerpts and personal notes on the textbook "Discrete Data Analysis with R" by Michael Friendly and David Meyer.
link-citations: yes
github-repo: petzi53/ddar
---


# Preface {-}

These are my excerpts and personal notes on the "Discrete Data Analysis with R" textbook by Michael Friendly and David Meyer, abbreviated `ddar`.

::: {.infobox}
Friendly, M., & Meyer, D. (2015). Discrete Data Analysis with R:
Visualization and Modeling Techniques for Categorical and Count Data
(1st ed.). Chapman and Hall/CRC.
:::

## Motivation {-}

The two primary purposes of my notes are 

1. fostering my learning about categorical data analysis by writing excerpts and experimenting with the code and
2. converting base R code to its newer **{tidyverse}** alternative.

R scripts and data files are taken from the [book's
website](http://ddar.datavis.ca/). The book is supported directly by the R packages **{[vcd](http://cran.r-project.org/package=vcd)}** and **{[vcdExtra](http://cran.r-project.org/package=vcdExtra)}**, along with numerous other R packages. There is 

- a [list of packages](http://ddar.datavis.ca/pages/using#r-packages) used 
- a [list of the data sets](http://ddar.datavis.ca/pages/using#data-sets-by-package) used in the book, by package and
- a [list of files with R code](http://ddar.datavis.ca/pages/using#r-code) per chapter or as a [zip file](http://ddar.datavis.ca/pages/Rcode/DDAR-Rcode.zip).


## Conventions {-}

I do not add prompts (`>` and `+`) to R source code in this book, and I comment out the text output with two hashes `##` by default. This is for your convenience when you want to copy and run the code (the text output will be ignored since it is commented out). 

Package names are in bold text surrounded by curly brackets (e.g., **{vcdExtra}**), and inline code and filenames are formatted in a typewriter font (e.g., `xtabs(Freq ~ ., data = UCB)`). 

Function names are followed by parentheses (e.g., `vcd::structable()`). The double-colon operator :: means accessing an object from a package.

My notes on [@Friendly_Meyer_2015] are part excerpt and part personal notes (comments). Most of the text are quotes taken directly from the book, which I have not marked especially.
Sometimes I commented or put forward my own thought. In those cases, I
have formatted the text as a quotation. Exceptions are paragraphs where it is evident that they are personal notes, for instance, when I used the first person). 

> This is an example of a paragraph marked as a quotation. These type of
> formatted text passages present --- contrary to the usual formatting
> habits --- my own thoughts, whereas excerpts or quotes are not marked specially.

