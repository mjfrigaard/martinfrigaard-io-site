---
title: How to manage your files with fs
author: Martin Frigaard
date: '2021-05-02'
slug: how-to-manage-your-files-with-fs
categories:
  - workflow
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-05-02T16:54:31-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---





Organizing your data, code, documents, and figures in a consistent way that's easy to navigate is one of the most thoughtful and generous things you'll ever do. It's also a basic requirement for a reproducible data science project. In RStudio, this process is made easier with `.Rproj` files ([read more](https://www.tidyverse.org/blog/2017/12/workflow-vs-script/)) and the [`here` package](https://github.com/jennybc/here_here). 

## What `fs` does?

The [`fs` package](https://cran.r-project.org/web/packages/fs/readme/README.html) adds another layer of project organization by giving us access to a "*uniform interface to file system operations.*" Many of the `fs` functions aren't new, but they now have consistent names (`path_`, `file_`, `dir_`, and `link_`) and return tidy paths (using `/` and not `//` or trailing `/`). Windows users have likely experienced the bane of collaborating with Unix users (and vice versa) because of the [difference in file paths](https://www.howtogeek.com/181774/why-windows-uses-backslashes-and-everything-else-uses-forward-slashes/). 

## How `fs` works

We will be using [Hadley Wickham's `nycflights13` package](https://github.com/hadley/nycflights13) as an example project to demonstrate some of the features of `fs`. First we'll get a 'big picture' view of the project folder and it's contents using `fs::dir_tree()`:


```r
fs::dir_tree("nycflights13")
## nycflights13
## ├── DESCRIPTION
## ├── NAMESPACE
## ├── NEWS.md
## ├── R
## │   ├── airlines.R
## │   ├── airport.R
## │   ├── flights.R
## │   ├── planes.R
## │   └── weather.R
## ├── README.md
## ├── cran-comments.md
## ├── data
## │   ├── airlines.rda
## │   ├── airports.rda
## │   ├── flights.rda
## │   ├── planes.rda
## │   └── weather.rda
## ├── data-raw
## │   ├── airlines.R
## │   ├── airlines.csv
## │   ├── airports.R
## │   ├── airports.csv
## │   ├── airports.dat
## │   ├── airports.svg
## │   ├── flights.R
## │   ├── planes.R
## │   ├── planes.csv
## │   ├── weather.R
## │   └── weather.csv
## ├── man
## │   ├── airlines.Rd
## │   ├── airports.Rd
## │   ├── flights.Rd
## │   ├── planes.Rd
## │   └── weather.Rd
## ├── nycflights.Rproj
## └── revdep
##     ├── README.md
##     ├── email.yml
##     └── problems.md
```

`dir_tree()` is similar to the [`tree` command in Linux](https://www.geeksforgeeks.org/tree-command-unixlinux/). If we want to narrow the scope of the tree, we can specify a subfolder and use the `regexp` or `glob` arguments.


```r
fs::dir_tree(path = "nycflights13/data-raw", regexp = "[.]csv$")
## nycflights13/data-raw
## ├── airlines.csv
## ├── airports.csv
## ├── planes.csv
## └── weather.csv
```

`fs` does more than just print folder contents to the screen. We can also construct clean file paths with `fs::path()`


```r
# create path to raw data 
raw_data_path <- fs::path("nycflights13", "data-raw")
# view these files in the OS
fs::file_show(raw_data_path)
```

<img src="images/file_show.png" alt="" width="80%" height="80%"/>

## Creating folders and files (with less worry)

I have a tendency to create `data` and `code` folders frequently. The `fs::dir_create()` and `fs::file_create()` functions are great because they don't overwrite my existing folders or files (no need for `if(!file.exists("data))` statements anymore!).


```r
# assume I already have a data-csv folder and data-csv/README.md file
fs::dir_tree("data-csv")
# what is in this file?
base::system(command = "cat 'data-csv/README.md'")
```

```r
# data-csv
# └── README.md
# CSV Data Files
# ==============
# 
# PLEASE!!! ‹(•_•)› NEVER CHANGE ME! ––•––√\/––√\/––•––•––√\/––√\/––•––•
```

Obviously some files should not be overwritten or altered after they've been created. 


```r
# will we overwrite this? try to create new data folder
fs::dir_create("data-csv")
# and try to create new README.md file
fs::file_create("data-csv/README.md")
# check their contents 
base::system(command = "cat 'data-csv/README.md'")
```

```r
# CSV Data Files
# ==============
# 
# PLEASE!!! ‹(•_•)› NEVER CHANGE ME! ––•––√\/––√\/––•––•––√\/––√\/––•––•
```

Well that's good news. 

### Directory/file info in tibbles!

The best part of `fs` is that we can store all this meta data from a project repo and manipulate it just like any `tibble` in `tidyverse`! 


```r
# store directory inforamtion in NycF13DirInfo
NycF13DirInfo <- fs::dir_info("nycflights13", recurse = TRUE)
NycF13DirInfo %>% 
  # check the files only
  dplyr::filter(type == "file") %>% 
  # that are biggest 
  dplyr::arrange(desc(size)) %>% 
  # only view size, birth time, and change time
  dplyr::select(path, size, birth_time, change_time) %>% 
  # return top 5
  utils::head(5)
##                                 path    size          birth_time
## 1      nycflights13/data/flights.rda    4.1M 2019-09-16 10:29:09
## 2  nycflights13/data-raw/weather.csv   2.19M 2019-09-16 10:29:09
## 3 nycflights13/data-raw/airports.dat 830.38K 2019-09-16 10:29:09
## 4   nycflights13/data-raw/planes.csv  241.4K 2019-09-16 10:29:09
## 5 nycflights13/data-raw/airports.svg 226.37K 2019-09-16 10:29:09
##           change_time
## 1 2020-05-29 11:37:44
## 2 2020-05-29 11:37:44
## 3 2020-05-29 11:37:44
## 4 2020-05-29 11:37:44
## 5 2020-05-29 11:37:44
```

This can come in handy if you're looking for the last time you modified a file, or if you're looking for a particularly large/small file. 

## Moving and copying files 

We can also easily copy files from one location to another. Below we move all the `.csv` files from `data-raw` into the parent `data` folder. 


```r
NycF13DirInfo %>% 
  # find all .csv files 
  dplyr::filter(stringr::str_detect(string = path, pattern = ".csv")) %>% 
  # move them into the parent data/ folder
  fs::file_copy(path = .$path, new_path =  fs::path("data-csv"))
# sanity check
fs::dir_tree("data-csv")
## data-csv
## ├── README.md
## ├── airlines.csv
## ├── airports.csv
## ├── planes.csv
## └── weather.csv
```

What if we'd like to read all of these files into one big data frame? We can create an `fs_path` which is a named character vector (with some additional coloring on capable terminals).


```r
# csv files
csv_files <- fs::dir_ls(path = "data-csv", glob = "*.csv")
utils::str(csv_files)
##  'fs_path' Named chr [1:4] "data-csv/airlines.csv" "data-csv/airports.csv" ...
##  - attr(*, "names")= chr [1:4] "data-csv/airlines.csv" "data-csv/airports.csv" "data-csv/planes.csv" "data-csv/weather.csv"
base::class(csv_files)
## [1] "fs_path"   "character"
base::is.character(csv_files)
## [1] TRUE
```

We can now pass this to `purrr::map_df()` and get all the .csv files in a huge dataset. 


```r
AllNycFlights <- csv_files %>%
  purrr::map_df(.f = read_csv, .id = "file", col_types = cols()) 
AllNycFlights %>% glimpse()
## Rows: 30,911
## Columns: 33
## $ file         <chr> "data-csv/airlines.csv", "data-csv/airlines.csv", "data-…
## $ carrier      <chr> "9E", "AA", "AS", "B6", "DL", "EV", "F9", "FL", "HA", "M…
## $ name         <chr> "Endeavor Air Inc.", "American Airlines Inc.", "Alaska A…
## $ faa          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ lat          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ lon          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ alt          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ tz           <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ dst          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ tzone        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ tailnum      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ year         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ type         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ manufacturer <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ model        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ engines      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ seats        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ speed        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ engine       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ origin       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ month        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ day          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ hour         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ temp         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ dewp         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ humid        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ wind_dir     <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ wind_speed   <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ wind_gust    <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ precip       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ pressure     <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ visib        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ time_hour    <dttm> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
```

Yikes! Crazy huh? Well, this isn't very practical to work with, but we could use `dplyr::filter()` to get the datasets we needed. 

The different values for `file` should correspond to the number of observations in each dataset in the `nycflights13` package. 


```r
# are these the number of observations per dataset?
AllNycFlights %>% dplyr::count(file, sort = TRUE)
## # A tibble: 4 x 2
##   file                      n
##   <chr>                 <int>
## 1 data-csv/weather.csv  26115
## 2 data-csv/planes.csv    3322
## 3 data-csv/airports.csv  1458
## 4 data-csv/airlines.csv    16
# sanity check 
nycflights13::weather %>% base::nrow()
## [1] 26115
# yes
```

## Works well with `purrr` package!

Ok, let's time-stamp these files and export them into a `today-csv` folder fun!


```r
# create today-csv folder
fs::dir_create("today-csv")
# create path
today_csv_path <- fs::path("today-csv/", base::noquote(lubridate::today()), "/")
# remove previous path from file
AllNycFlights %>%
  dplyr::mutate(file = stringr::str_remove_all(string = file, 
                                               pattern = "data-csv/")) %>% 
  # group by file
  dplyr::group_by(file) %>% 
  # keep only distinct rows (probably redundant)
  dplyr::group_map(~ dplyr::distinct(.x, .keep_all=TRUE), keep = TRUE) %>%
  purrr::walk(~.x %>% 
                 readr::write_csv(path = paste0(today_csv_path, 
                                         "_",
                                         unique(.x$file))))
# sanity check!
fs::dir_tree("today-csv")
## today-csv
## ├── 2020-05-29_airlines.csv
## ├── 2020-05-29_airports.csv
## ├── 2020-05-29_planes.csv
## └── 2020-05-29_weather.csv
```

Check out the vignette and package site for more information!




