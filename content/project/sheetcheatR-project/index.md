---
date: 2022-10-25T18:47:33-07:00
title: sheetcheatR R Package
external_link: ''
image:
  caption: sheetcheatR R Package (version 0.2)
  focal_point: Smart
slides: ''
summary: R package of `learnr` tutorials based on RStudio's excellent collection of cheatsheets
tags:
- packages
url_code: "https://mjfrigaard.github.io/sheetcheatR/"
url_pdf: ""
url_slides: ""
url_video: ""
---

The [`sheetcheatR` package](https://mjfrigaard.github.io/sheetcheatR/) contains a series of [`learnr` tutorials](https://rstudio.github.io/learnr/) for new users to R (or the [`tidyverse`](https://www.tidyverse.org/)). 

Install this package from GitHub below: 

```r
install.packages("devtools")
devtools::install_github("mjfrigaard/sheetcheatR")
```

Access the tutorial right in your RStudio session by using the following: 

```r
library(sheetcheatR)
library(learnr)
learnr::run_tutorial("import", package = "sheetcheatR")
```

So far the topics include: 

1. [Command-Line Introduction](https://mjfrigaard.github.io/sheetcheatR/tutorials/command-line.html): tools for navigating and managing files (`cd`, `pwd`, `touch`, `echo`, etc.), pipes, and downloading data

2. [Atomic Vectors](https://mjfrigaard.github.io/sheetcheatR/tutorials/atomic-vectors.html): An introduction to R’s foundational data object 

3. [S3 Vectors](https://mjfrigaard.github.io/sheetcheatR/tutorials/introduction-to-S3-vectors.html): Factors, dates, datetimes, and difftimes

4. [Rectangular data in R](https://mjfrigaard.github.io/sheetcheatR/tutorials/rectangles.html): Matrices, `data.frame`s, `tibble`s, and `data.table`s

5. [Data import](https://mjfrigaard.github.io/sheetcheatR/tutorials/import.html) with `readr`, `readxl`, and `googlesheets4`

6. [Data visualization with `ggplot2`](https://mjfrigaard.github.io/sheetcheatR/tutorials/ggp2-01.html) cheatsheet (part 1)

7. [Tidying your data with `tidyr`](https://mjfrigaard.github.io/sheetcheatR/tutorials/tidyr-01.html) (part 1)  

8. [Data Transformation with `dplyr`](https://mjfrigaard.github.io/sheetcheatR/tutorials/dplyr-p1.html) (part 1) 

This package is still in development--see the posts for more information as I complete additional tutorials. 

Cheers! 

