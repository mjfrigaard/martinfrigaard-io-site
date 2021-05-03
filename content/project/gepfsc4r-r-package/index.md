---
# Documentation: https://wowchemy.com/docs/managing-content/
date: 2021-05-02T18:47:33-07:00
title: "`gepfsc4r` R Package"
summary: "R package based on the excellent paper, \"Good enough practices for scientific computing\" (in R)"
authors: [Martin Frigaard]
tags: [rstats]
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

url_code: "https://github.com/mjfrigaard/gepfsc4r"
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

The [`gepfsc4r` package](https://github.com/mjfrigaard/gepfsc4r) is based on the **excellent** advice in the
paper, [Good Enough Practices for Scientific Computing](https://swcarpentry.github.io/good-enough-practices-in-scientific-computing/)
by Wilson et al. 

The goal of `gepfsc4r` is to create the files and folders outlined in
the paper above (and like the folder tree below)


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

## Installation

You can install the Github version below:

```r
install.packages("devtools")
devtools::install_github("mjfrigaard/gepfsc4r")
```