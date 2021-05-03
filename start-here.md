Building a website with RStudio and Hugo Academic 
===========


## Overview

I recently created a website in [RStudio](https://rstudio.com/) with [blogdown](https://bookdown.org/yihui/blogdown/) using the [Hugo academic theme](https://themes.gohugo.io/academic/) and [Netlify](https://www.netlify.com/). I followed the excellent [Twitter thread](https://twitter.com/dsquintana/status/1139846569623281664) by Dan Quintana. [Here](https://evamaerey.github.io/what_how_guides/academic_website_w_blogdown) is a cleaner version.

I've included some additional information anywhere the directions from the Twitter thread were different and/or outdated. Read more about the academic theme in the [documentation](https://wowchemy.com/docs/getting-started/install/). 


## 1) Getting started 

All of these steps were done using R, RStudio, GitHub, and Netlify.

- Technology requirements:  
  - [x] Download and install [RStudio](https://rstudio.com/)  
  - [x] Download and install [R](https://cran.r-project.org/)  
  - [x] Create a [Netlify](https://www.netlify.com/) account  
  - [x] Create a [GitHub](https://github.com) account.  

You can optionally purchase a domain through a service like [Google Domains](https://domains.google/). Feel free to read more about domain registration [here in the `blogdown` book](https://bookdown.org/yihui/blogdown/domain-name.html).  



***

## 2) Installing the `blogdown` package 

The first tweet covers installation and loading the `blogdown` package.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Install blogdown in R using this command: <br><br>install.packages(&quot;blogdown”) <br><br>Then start a new project, entering “gcushen/hugo-academic” as the Hugo theme. Keep the other options ticked. This will download all the necessary files. <a href="https://t.co/kq6yhgxPSG">pic.twitter.com/kq6yhgxPSG</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846625839464448?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Run the following code to install `blogdown` (also make sure you have Hugo installed). 

```r
install.packages("blogdown”)
library(blogdown) 
blogdown::install_hugo(force = TRUE)
```

After running `blogdown::install_hugo(force = TRUE)`, you might need to update the version of Hugo in your `.Rprofile`.

```r
The latest Hugo version is v0.82.0
------------------------------------------------------------------------
You have set the option 'blogdown.hugo.version' to '0.80.0' (perhaps in 
.Rprofile), but you are installing the Hugo version '0.82.0' now. You may
want to update the option 'blogdown.hugo.version' accordingly.
-----------------------------------------------------------------------
trying URL 'https://github.com/gohugoio/hugo/releases/download/v0.82.0/
hugo_extended_0.82.0_macOS-64bit.tar.gz'
Content type 'application/octet-stream' length 15013690 bytes (14.3 MB)
==================================================
downloaded 14.3 MB

Hugo has been installed to "/Users/mjfrigaard/Library/Application
Support/Hugo/0.82.0".
```

The .Rprofile file should automatically open, and we can change this on the following line, 

```r
# fix Hugo version
options(blogdown.hugo.version = "0.82.0")
```

***

## 3) Creating a new website with `blogdown` in RStudio

You can do this using the IDE (*if your RStudio IDE doesn't look as cool as mine, consider installing the [rsthemes](https://www.garrickadenbuie.com/project/rsthemes/) package from the embassador of all things cool, [Garrick Aden‑Buie](https://www.garrickadenbuie.com/)*)

### 3.1) Using the RStudio IDE

Click on ***Project***, ***New Project***

![](https://i.imgur.com/oDKAgAe.png)

Then ***New Directory***

![](https://i.imgur.com/P81r5aJ.png)

Then ***Website using blogdown***

![](https://i.imgur.com/OaDjjuP.png)

Fill in  
1) ***Directory Name***, 
2) ***Create project as subdirectory of:***, and 
3) ***Hugo theme:***

![](https://i.imgur.com/tZH1Pit.png)

Click on ***Create Project*** and this will create a new RStudio session for our blog! 

We should see this in the ***Files*** pane: 

![](https://i.imgur.com/aqV0JtT.png)

### 3.2) Using the command line 

Or you can enter the following lines of code in the **Console**

```r
blogdown::new_site(dir = "~/Documents/example-academic", 
                   install_hugo = TRUE, 
                   theme = "wowchemy/starter-academic")
```

You will see the following output: 

```r
― Creating your new site
| Installing the theme wowchemy/starter-academic from github.com
trying URL 'https://github.com/wowchemy/starter-academic/archive/master.tar.gz'
downloaded 7.3 MB

trying URL 'https://github.com/wowchemy/wowchemy-hugo-modules/archive/17505c4581cd.tar.gz'
downloaded 520 KB

| Adding the sample post to /Users/mjfrigaard/Documents/example-academic/content/post/2020-12-01-r-rmarkdown/index.en.Rmd
| Converting all metadata to the YAML format
| Adding netlify.toml in case you want to deploy the site to Netlify
● [TODO] The file 'netlify.toml' exists, and I will not overwrite it. 
  If you want to overwrite it, you may call blogdown::config_netlify() 
  by yourself.
| Adding .Rprofile to set options() for blogdown
― The new site is ready
○ To start a local preview: use blogdown::serve_site(), or the RStudio
  add-in "Serve Site"
○ To stop a local preview: use blogdown::stop_server(), or restart 
  your R session
► Want to serve and preview the site now? (y/n)
```

We are impatient and want to see this thing, so we enter `y`

```r
► Want to serve and preview the site now? (y/n) y
```

And voila! Our new site!

![](https://i.imgur.com/RTBoQuR.png)



***



## 4) Building vs. serving the new site 

There are two commands to use when building a new website:  [`blogdown::build_site()`](https://pkgs.rstudio.com/blogdown/reference/build_site.html) and [`blogdown::serve_site()`](https://pkgs.rstudio.com/blogdown/reference/serve_site.html)

### 4.1) `blogdown::build_site()`

The first, `blogdown::build_site()`, will rebuild your website locally. See the example below:

![](https://i.imgur.com/85dkP6v.png)

### 4.2) `blogdown::serve_site()`

The second command, `blogdown::serve_site()` will allow us to preview a local version of our website. 

![](https://i.imgur.com/qQRZ0eg.png)

When we makes changes, we will use the combination of `blogdown::build_site()` and `blogdown::serve_site()` to deploy our site to Netlify. You can read more about this workflow in the [blogdown book](https://bookdown.org/yihui/blogdown/workflow.html), but the gist of it is that all the website's files are created in the `public/` folder when we build and serve our site, and then when we push the changes to GitHub, Netlify deploys our new site: 

```bash
git add -A 
git commit -m "updates"
git push
```

There is a more in-depth summary of this process on the [deployment chapter of the blogdown book](https://bookdown.org/yihui/blogdown/deployment.html#deployment), too. 


*** 

## Making changes 

The next sections will cover how to make changes to the content of your academic site. As you can see from the preview, the demo site comes with a ton of example content, so most of the changes can be done to these existing files. Other changes will involve creating new files, or removing the example files and folders. 


### 5) Changing the title of your website

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Let’s start by changing the title of your website, which can be edited in the &quot;config.toml&quot; file. To edit this file, and the others that are referred to in this guide, just select it in the RStudio file browser. <a href="https://t.co/IgfSCxxZeH">pic.twitter.com/IgfSCxxZeH</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846713525571584?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


In this step we learn how to change the title on our academic website. 

The name of this website is `martinfrigaard.io`, and we will change the first three lines in the `config.yaml` file:

```yaml
theme: starter-academic
title: Martin Frigaard dot io
baseurl: 'https://www.martinfrigaard.io/'
```

We can check this with another call to `blogdown::serve_site()`. 

![](https://i.imgur.com/V78y3Ze.png)

Great! So far, so good!


***

### 6) Changing the colors 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">In the &quot;config/\_default/params.toml&quot; file you can change the color theme and font style of your website, so have a play around with these options. <a href="https://t.co/Sajbncx2gR">pic.twitter.com/Sajbncx2gR</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846761760067584?ref\_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


The `config/_default/params.toml` file contains the colors settings for your blog. 


```bash
config/_default/
├── languages.toml
├── menus.toml
└── params.toml
```

These are actually controlled by the `theme` settings: 

```toml 
############################
## Theme
############################
# Choose a theme.
#   Latest themes (may require updating): 
#     https://sourcethemes.com/academic/themes/
#   Browse built-in themes in `themes/academic/data/themes/`
#   Browse user installed themes in `data/themes/`
theme = "mr robot"
```

#### Error alert!

After setting the `theme` to `mr robot`, we rebuild and serve the site, and we get the following warning: 

```toml 
Start building sites … 
WARN 2021/03/20 11:43:48 Alert shortcode will be deprecated in future. 
Use Callout instead. Rename `alert` to `callout` in 
"post/writing-technical-content/index.md"
WARN 2021/03/20 11:43:48 Alert shortcode will be deprecated in future. 
Use Callout instead. Rename `alert` to `callout` in "home/demo.md"
```

![](https://i.imgur.com/czDSC2q.jpg)

After changing all the `alert`s to `callout`s in `index.md` and `demo.md`, we no longer see the warning.


### 7) Changing the `hero` widget

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Your main homepage is made up of a set of widgets, which you can customize or remove entirely. For example, let’s say we want to remove the big header image, called the “hero” widget. Let&#39;s open up the &quot;content/home/hero.md&quot; file and change &quot;active = true&quot; to &quot;active = false&quot; <a href="https://t.co/4BXz8rGe3e">pic.twitter.com/4BXz8rGe3e</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846818039123968?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Next we will change the `hero` widget. We can do this by going into `content/home/hero.md` and changing `active = true` to `active = false`

```markdown
# Hero widget.
widget = "hero"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = false  # Activate this widget? true/false
weight = 10  # Order that this section will appear.
```

![](https://i.imgur.com/IvfKP3Y.jpg)

### 8) Updating Profile Photo

This tweet covers the profile photo:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Let’s now update the profile photo. Just save your profile in the &quot;content/authors/admin&quot; folder, calling the file &quot;avatar.jpg&quot;. This will automatically update your profile picture. <a href="https://t.co/6YW3KkwsdC">pic.twitter.com/6YW3KkwsdC</a></p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846874775638018?ref_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


We're going to change our profile photo using a local image file (a headshot my mother approves of :))

We'll move it in `content/authors/admin` as `avatar.jpg`. After rebuilding, we see the following: 

![](https://i.imgur.com/4tSueK9.png)

*But wait, that's not my name!!!*

### 9) Editing Biography

This tweet covers the biography details:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Edit your biography details (e.g, position, affiliation, education details) in the &quot;content/authors/admin/\_index.md&quot; file. In this file you can also add your social media details and a link to your Google Scholar profile page.</p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846878856658944?ref\_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

We navigate to the `content/authors/admin/_index.md` file and edit the relavent content. 

After rebuilding and serving, the site looks like this: 

![](https://i.imgur.com/ecfgdfg.png)

### 10) Editing Contact Details

This tweet covers the contact details:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">To edit your contact details, navigate to the &quot;config/\_default/params.toml&quot; file, and scroll down to the &quot;Contact Widget setup&quot; section.</p>&mdash; Dan Quintana (@dsquintana) <a href="https://twitter.com/dsquintana/status/1139846880786096128?ref\_src=twsrc%5Etfw">June 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

To edit the contact details, we'll use the same `config/\\_default/params.toml` file we did previously, but this time we'll change the `Contact details` section:

```toml 
############################
## Contact details
##
## These details power the Contact widget (if enabled).
## Additionally, for organizations, these details may be used to enrich
## search engine results.
############################
```

We change the following info: 

```toml
# Enter contact details (optional). To hide a field, clear it to "".
email = "mjfrigaard@pm.me"
phone = "503 333 0531"

# Address
# For country_code, use the 2-letter ISO code (see https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2 )
address = {street = "2034 S Paseo Loma Circle", city = "Mesa", region = "AZ", postcode = "85202", country = "United States", country_code = "US"}

# Geographic coordinates
# To get your coordinates, right-click on Google Maps and choose "What's here?". The coords will show up at the bottom.
coordinates = {latitude = "", longitude = ""}

# Directions for visitors to locate you.
directions = ""

# Office hours
# A list of your office hours. To remove, set to an empty list `[]`.
office_hours = []

# Enter an optional link for booking appointments (e.g. calendly.com).
appointment_url = "https://calendly.com/mfrigaard"

# Contact links
#   Set to `[]` to disable, or comment out unwanted lines with a hash `#`.
contact_links = [
  {icon = "twitter", icon_pack = "fab", name = "DM Me", link = "https://twitter.com/mjfrigaard"},
  # {icon = "skype", icon_pack = "fab", name = "Skype Me", link = "skype:echo123?call"},
  # {icon = "keybase", icon_pack = "fab", name = "Chat on Keybase", link = "https://keybase.io/"},
  # {icon = "comments", icon_pack = "fas", name = "Discuss on Forum", link = "https://discourse.gohugo.io"},
  # {icon = "telegram", icon_pack = "fab", name = "Telegram Me", link = "https://telegram.me/@Telegram"},
  ]
```

When we rebuild and serve the site, we see the following:

![](https://i.imgur.com/dCH1Hc4.png)


### 11) Removing Demo

There isn't a tweet for this section, but we want to remove the demo from our landing page. We can do this by going to `content/home/demo.md` and changeing `active: true` to `active: false`

```markdown
# Activate this widget? true/false
active: false
```

Build, serve, and we see: 

![](https://i.imgur.com/NyRHgFG.png)

We can see there is a `Demo` option listed on the header, which we can remove with by editing the `config/_default/menus.toml` file: 

```toml
[[main]] 
  name = "Demo"
  url = "#hero"
  weight = 10
```

We comment this out and add a note 

```toml
# [[main]] -> demo.md was set to `active: false`
#  name = "Demo"
#  url = "#hero"
#  weight = 10
```

Build, serve, and we see:

![](https://i.imgur.com/wgwXnNi.png)

The `Demo` item has been removed from the header!


### 12) Editing Experience 

The experience section is located in `content/home/experience.md`. The details on how to add/change this content is provided below:

```markdown
# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
```

Each experience is listed under `experience:` and has the following information: 

```markdown 
experience:
  - title: Senior Clinical Programmer
    company: BioMarin
    company_url: https://www.biomarin.com/
    location: California
    date_start: '2020-10-01'
    date_end: ''
    description: |2-
        Responsibilities include:
        * Centralized statistical monitoring (CSM)
        * Shiny app development 
        * Natural language processing
```

When we rebuild and serve the site, we see the following: 

![](https://i.imgur.com/qM1ifOk.png)


### 13) Editing Accomplishments

The accomplishments portion is available in the `content/home/accomplishments.md` file. The unstructions are below: 

```markdown
# Accomplishments.
#   Add/remove as many `item` blocks below as you like.
#   `title`, `organization`, and `date_start` are the required parameters.
#   Leave other parameters empty if not required.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
```

I've included an example below that uses the YAML multi-line pefix and hyperlinks. 

```markdown 
item:
- certificate_url: https://www.datacamp.com/statement-of-accomplishment/track/35a719d02626487a2b9ef32451c1523041d21a5c
  date_end: ""
  date_start: "2020-03-31"
  description: |2-
    Covers shiny reactivity, UI and server components, and includes the following courses:
    * [Building Web Applications with Shiny in R](https://learn.datacamp.com/courses/building-web-applications-with-shiny-in-r)
    * [Case Studies - Building Web Applications with Shiny in R](https://learn.datacamp.com/courses/case-studies-building-web-applications-with-shiny-in-r)
    * [Building Dashboards with shinydashboard](https://learn.datacamp.com/courses/building-dashboards-with-shinydashboard)
  organization: DataCamp
  organization_url: https://www.datacamp.com
  title: Shiny Fundamentals with R Track
  url: https://www.datacamp.com/tracks/shiny-fundamentals-with-r
```

When we run `blogdown::build_site()` and `blogdown::serve_site()`, we see the following: 

![](https://i.imgur.com/FVoIbST.png)


### 14) Editing Projects 

The projects represent a *Portfolio* section and are stored in the following folder: 

```bash
site/content/project/
	├── external-project
	│   ├── featured.jpg
	│   └── index.md
	└── internal-project
	    ├── featured.jpg
	    └── index.md
```

Each project gets it's own folder with an `index.md` file and `featured.jpg` image. There are two types of projects:  
1) `external-project`s link out to a website url, and  
2) `internal-project`s link to a page within our site  

#### 14.1) Internal projects

Note the `external_link:` is set to `""`

```markdown
---
date: "2016-04-27T00:00:00Z"
external_link: ""
image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/georgecushen
slides: example
summary: An example of using the in-built project page.
tags:
- Deep Learning
title: Internal Project
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---

Lorem ipsum dolor...(omitted)

```

#### 14.2) External project

Here the `external_link:` is set to `http://example.org`

```markdown
---
date: "2016-04-27T00:00:00Z"
external_link: http://example.org
image:
  caption: Photo by Toa Heftiba on Unsplash
  focal_point: Smart
summary: An example of linking directly to an external project website using `external_link`.
tags:
- Demo
title: External Project
---
```

#### 14.3) Creating a new project 

Run the following command in the Terminal to create a new project: 

```bash
$ hugo new  --kind project project/my-new-project
# you should see this...
example-academic/content/project/my-new-project created
```

We can see this (along with the two default projects) in the folder tree below: 

```r
content/project/
	├── external-project
	│   ├── featured.jpg
	│   └── index.md
	├── internal-project
	│   ├── featured.jpg
	│   └── index.md
	└── my-new-project
    	    └── index.md
```

There are two places we need to make changes in order to add a new project: 

1. We also need to change the `content/project/my-new-project/index.md` file to it's referenced correctly  
3. We also need to update the `content/home/projects.md` file with the name of our new project  

If we open the `content/project/my-new-project/index.md` file, we see the following boiler-plate template: 

```markdown
---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "My New Project"
summary: ""
authors: []
tags: []
categories: []
date: 2021-03-21T22:41:03-07:00

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, 
# Right, BottomLeft, Bottom, BottomRight.
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

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides. Simply enter your 
#   slide deck's filename without extension. 
#   E.g. `slides = "example-slides"` references 
#   `content/slides/example-slides.md`. Otherwise, set `slides = ""`.
slides: ""
---
```

If we build and serve the site right now, we see the following: 

![](https://i.imgur.com/RYPkZnL.png)

The new project is added, but we can't using the filtering toolbar (and there is no text summary for this project). We'll add the following content to the project: 

```markdown
---
# Documentation: https://wowchemy.com/docs/managing-content/

title: CalFresh Shiny Dashboard
summary: A shiny dashboard to track CalFresh grant activities for the California State University system.
authors: []
tags:
- flexdashboard
categories: []
date: "2021-03-19T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft,
# Bottom, BottomRight.
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

url_code: ""
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

The [CHC/CalFresh Shiny Dashboard](https://mjfrigaard.shinyapps.io/2-1-chc-calfresh-dashboard/) is a [shiny](https://shiny.rstudio.com/) dashboard built with the [`flexdashboard` package](https://rmarkdown.rstudio.com/flexdashboard/). 
```

Note that we did not add the link for this application to `external_link:`. Instead, we will add it to the text below the header (`---`): 

If we build and serve again, we see the project has been added to the `Projects` section, but it's not available to us in the toolbar. 

![](https://i.imgur.com/yVPrbVu.png)


We can change this in the `content/home/projects.md` file (which controls how the projects are displayed on the menu):

```markdown
---
# An instance of the Portfolio widget.
# Documentation: https://wowchemy.com/docs/page-builder/
widget: portfolio

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 65

title: Projects
subtitle: ''

content:
  # Page type to display. E.g. project.
  page_type: project

  # Default filter index (e.g. 0 corresponds to the 
  # first `filter_button` instance below).
  filter_default: 0

  # Filter toolbar (optional).
  # Add or remove as many filters (`filter_button` instances) as you like.
  # To show all items, set `tag` to "*".
  # To filter by a specific tag, set `tag` to an existing tag name.
  # To remove the toolbar, delete the entire `filter_button` block.
  filter_button:
  - name: All
    tag: '*'
  - name: Deep Learning
    tag: Deep Learning
  - name: Other
    tag: Demo

design:
  # Choose how many columns the section has. Valid values: '1' or '2'.
  columns: '2'

  # Toggle between the various page layout types.
  #   1 = List
  #   2 = Compact
  #   3 = Card
  #   5 = Showcase
  view: 2

  # For Showcase view, flip alternate rows?
  flip_alt_rows: false
---
```

#### 14.4) The Project `filter_button:`

The `filter_button:` section is where we can reference the contents of our new `content/project/my-new-project/index.md` file. Enter the `title` (as `name:`) and `tag`. 

```markdown
---
  # Filter toolbar (optional).
  # Add or remove as many filters (`filter_button` instances) as you like.
  # To show all items, set `tag` to "*".
  # To filter by a specific tag, set `tag` to an existing tag name.
  # To remove the toolbar, delete the entire `filter_button` block.
  filter_button:
  - name: All
    tag: '*'
  - name: Deep Learning
    tag: Deep Learning
  - name: Other
    tag: Demo
  - name: CalFresh Shiny Dashboard
    tag: flexdashboard
```

Now when we build and serve, we can see the project listed. 

![](https://i.imgur.com/V4y6Esa.png)

If we click on the project and follow it to the application, we can take a screenshot and save it in the `content/project/my-new-project/` folder as `featured.jpg`. 

Rebuild and re-serve, and we can our new project listed like the previous two. 

![](https://i.imgur.com/eSjJwbR.png)


#### 14.5) Removing old projects

As nice as it is for the academic theme to provide us with example content, we will probably want to remove the existing projects, blog posts, etc. from our site. 

We can do this by deleting the `content/project/external-project` *and* the `content/project/external-project` folder, so we only have our new project and associated files: 


```bash
content/project
	└── my-new-project
    	   ├── featured.jpg
    	   └── index.md
```

We can also delete these folders from `public/project/` folder, which gets created whenever we rebuild the site (*In fact, you can delete the entire `public/` folder, but that's a little extreme...*)

Remember that we also need to remove the following lines from `content/home/projects.md`: 

```markdown
  - name: Deep Learning
    tag: Deep Learning
  - name: Other
    tag: Demo
```

Now when I rebuild and reload, only the single project remains. 

![](https://i.imgur.com/ihxh5iB.png)


### 13) Editing Publications

All of the publications are stored in 

### 14) Editing Courses 



### 15) Editing Talks

Edit the file stored in this folder: 

`content/event/example/`


```markdown
---
abstract: Presentation by PDG with RStudio coding walk-through on "Analytic Literacy for the 21st Century" given at California State University, Chico.
address:
  city: Chico
  country: United States
  postcode: "95926"
  region: CA
  street: 400 W 1st St, Chico, CA 
all_day: false
authors: []
date: "2019-04-19T13:00:00Z"
date_end: "2019-04-19T13:00:00Z"
event: Analytic literacy (presentation and demo) 
event_url: https://media.csuchico.edu/media/0_2yf52lzi
featured: false
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/bzdhc5b3Bxs)'
  focal_point: Right
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/mjfrigaard
location: California State University, Chico
projects:
- internal-project
publishDate: "2019-04-26T00:00:00Z"
slides: example
summary: Paradigm Data Group talk for CSUC Data Science Initiative.
tags: []
title: Analytic Literacy for the 21st Century
url_code: ""
url_pdf: ""
url_slides: "https://fr.slideshare.net/MartinFrigaard/data-fluency-for-the-21st-century-147187624"
url_video: "https://media.csuchico.edu/media/0_2yf52lzi"
---
```

```markdown

{{% callout note %}}
Click on the **Slides** button above to view the built-in slides feature.
{{% /callout %}}

Slides can be added in a few ways:

- **Create** slides using Wowchemy's [*Slides*](https://wowchemy.com/docs/managing-content/#create-slides) feature and link using `slides` parameter in the front matter of the talk file
- **Upload** an existing slide deck to `static/` and link using `url_slides` parameter in the front matter of the talk file
- **Embed** your slides (e.g. Google Slides) or presentation video on this page using [shortcodes](https://wowchemy.com/docs/writing-markdown-latex/).

Further event details, including [page elements](https://wowchemy.com/docs/writing-markdown-latex/) such as image galleries, can be added to the body of this page.
```

Then delete all the files stored in the public folder here, 

`public/event/`

and rename the example folder: 

`content/event/csuc-pdg-dsi-2019/`

I also changed the `_index.md` file stored in `content/event/`

```markdown
---
header:
  caption: ""
  image: ""
title: Recent & Upcoming Talks
view: 1
---
```


***

###### tags: `rstats` `RStudio`