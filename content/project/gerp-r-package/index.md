---
# Documentation: https://wowchemy.com/docs/managing-content/
date: 2021-05-02T18:47:33-07:00
title: "`gerp` R Package"
summary: "R package based on the excellent paper, \"Good enough practices for scientific computing\" (in R)"
authors: [Martin Frigaard]
tags:
  - packages
categories: [packages]

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: "https://github.com/mjfrigaard/gerp"
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

The [`gerp` package](https://github.com/mjfrigaard/gerp) is based on the **excellent** advice in the
paper, [Good Enough Practices for Scientific Computing](https://swcarpentry.github.io/good-enough-practices-in-scientific-computing/)
by Wilson et al. 

The goal of `gerp` is to setup your project files and folders in a 
structure similar to the one outlined in the paper above (and like the folder tree below): 

```bash
|-- CITATION
|-- README
|-- LICENSE
|-- requirements.txt
|-- data
|   -- birds_count_table.csv
|-- doc
|   -- notebook.md
|   -- manuscript.md
|   -- changelog.txt
|-- results
|   -- summarized_results.csv
|-- src
|   -- sightings_analysis.py
|   -- runall.py
```

`gerp` creates the format below using the following lines of code: 

```r
# install.packages("pak")
pak::pak("mjfrigaard/gerp")
library(gerp)
gerp::ger_proj()
```

![](https://mjfrigaard.github.io/gerp/reference/figures/new_ger_proj.gif)

```bash
├── CITATION
├── LICENSE
├── README.Rmd
├── code
│   ├── 01-import.R
│   ├── 02-tidy.R
│   ├── 03-wrangle.R
│   ├── 04-visualize.R
│   ├── 05-model.R
│   ├── 06-communicate.R
│   └── runall.R
├── data
│   ├── README.md
│   └── raw
├── docs
│   ├── changelog.txt
│   ├── manuscript.Rmd
│   └── notebook.Rmd
├── my_project.Rproj
├── requirements.txt
└── results
    ├── figures
    ├── manuscript
    └── tables
```

## Package website 

[Here is a link](https://mjfrigaard.github.io/gerp/) for the package website I built using the phenomenal [pkgdown](https://pkgdown.r-lib.org/) package. 

Feel free to check it out and leave comments! 

Cheers! 

