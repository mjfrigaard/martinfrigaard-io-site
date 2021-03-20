martin frigaard dot eye oh
===========

# Table of Contents

[TOC]

## Overview

This is the Github repo for my personal website. I built this website in [RStudio](https://rstudio.com/) with [blogdown](https://bookdown.org/yihui/blogdown/) using the [Hugo academic theme](https://themes.gohugo.io/academic/) and [Netlify](https://www.netlify.com/). 

Read more about the Academic theme in the [documentation](https://wowchemy.com/docs/getting-started/install/). 

## Resources

I created this website following the excellent [Twitter thread](https://twitter.com/dsquintana/status/1139846569623281664) by Dan Quintana. I've included information anywhere the directions from the Twitter thread were incomplete, incorrect, or outdated. 


## :bulb: Getting started 

==All of these steps were done using R/RStudio, Github, and Netlify== 

- Technology requirements:  
  - [x] Download and install [RStudio](https://rstudio.com/)  
  - [x] Download and install [R](https://cran.r-project.org/)  
  - [x] Create a [Netlify](https://www.netlify.com/) account  


***

### 1) Create a new RStudio Project 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Install blogdown in R using this command: <br><br>install.packages(&quot;blogdown”) <br><br>Then start a new project, entering “gcushen/hugo-academic” as the Hugo theme. Keep the other options ticked. This will download all the necessary files. <a href="https://t.co/kq6yhgxPSG">pic.twitter.com/kq6yhgxPSG</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846625839464448?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

***

Run the following code to install `blogdown` (also make sure you have Hugo installed). 

```r
install.packages("blogdown”)
library(blogdown) 
blogdown::install_hugo(force = TRUE)
```

***

### Serve site 

Run the following command to build your website in RStudio:

```r
blogdown::serve_site()

==> Found hugo at "/Users/mjfrigaard/Library/Application Support/Hugo/0.80.0/hugo" 
and "/usr/local/bin/hugo". The former will be used. 

If you don't need both copies, you may delete/uninstall the latter with 
system("brew uninstall hugo") in R.

Launching the server via the command:
  /Users/mjfrigaard/Library/Application Support/Hugo/0.80.0/hugo server --bind
  127.0.0.1 -p 4321 --themesDir themes -t starter-academic -D -F --navigateToChanged
  
Serving the directory . at http://localhost:4321
Launched the hugo server in the background (process ID: 23676). To stop it, call 
blogdown::stop_server() or restart the R session.
```

***


### Changing the title of your website

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Let’s start by changing the title of your website, which can be edited in the &quot;config.toml&quot; file. To edit this file, and the others that are referred to in this guide, just select it in the RStudio file browser. <a href="https://t.co/IgfSCxxZeH">pic.twitter.com/IgfSCxxZeH</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846713525571584?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The name of this website is `martinfrigaard.io`

