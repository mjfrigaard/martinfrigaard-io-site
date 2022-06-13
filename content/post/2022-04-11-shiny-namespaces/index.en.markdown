---
title: 'Shiny namespaces'
author: 'Martin Frigaard'
date: '2022-04-11'
slug: shiny-namespaces
categories:
  - shiny
tags:
  - modules
subtitle: ''
summary: ''
authors: []
lastmod: '2022-04-11T13:50:54-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

# Namespaces

<br>

This is a quick post about namespaces in shiny modules. I’ll cover how the **`NS()`** function helps organize IDs in the UI, and how these IDs are accessed in the server with **`moduleServer()`**.

<br>

## Keeping track of all those inputIds/outputIds

<br>

As you can imagine, keeping track of all the IDs (**`inputId`**/**`input$`** and **`outputId`**/**`output$`**) becomes [difficult and time consuming](https://engineering-shiny.org/structuring-project.html#a.-your-first-shiny-module). It’s also hard to anticipate which names we’ll need in the future, or the level of precision these names require. This is where modules and namespaces come in handy.

<br>

## What is a namespace in a shiny app?

<br>

#### ***“…a namespace is to an ID as a directory is to a file…”***

<br>

The quote above is from the [**`NS()`** help files](https://shiny.rstudio.com/reference/shiny/0.13.1/NS.html), and I’ve found it’s a great way to conceptualize what **`NS()`** is creating.

If we imagine creating two files with identical names (**`file.R`** and **`file.R`**) we quickly discover they can’t exist in the same directory. See what happens below when we try to create them in a directory named **`folder`**:

``` r
fs::dir_create("folder") # create directory
fs::file_create("folder/file.R") # create file 1
fs::file_create("folder/file.R") # create file 2 (same name)
fs::dir_tree("folder") # check files in directory
```

``` r
folder/
    └── file.R
```

<br>

We can see there is only one **`file.R`** in the **`folder/`** directory (the second **`fs::file_create("folder/file.R")`** overwrote the first **`file.R`**). You’ve probably seen an error like this when trying to save two files with the same name into the same folder.

However, we can have files with identical names in two *different* folders, but we have to reorganize the structure. Below we’ve created a parent folder (**`folder/`**) with two sub-folders (**`sub-folder-A/`** and **`sub-folder-B/`**), where we can place each file:

``` r
 folder/
    ├── sub-folder-A/
    │       └── file.R
    └── sub-folder-B/
            └── file.R
```

<br>

By placing the files in sub-directories, we’ve made the ***path*** to each file unique, which makes it possible for them to have identical names. We can access these files using their ***unique path*** (not their unique name). The **`NS()`** function works in a similar way by creating a unique ‘***path***’ for each ID the module.

<br>

### Using **`NS()`**

<br>

Sometimes you’ll see **`NS()`** used to create a separate **`ns()`** function, which is then used to define IDs in the module (i.e. **`ns <- shiny::NS(id)`** then **`ns("[name]"))`**:

``` r
text_example_01_UI <- function(id) {
  ns <- shiny::NS(id)
  tagList(
    textAreaInput(inputId = ns("text_in"), 
                  label = "Text"),
    verbatimTextOutput(ns("text_out"))
  )
}
text_example_01_UI(id = "text")
```

<div class="form-group shiny-input-container">
<label class="control-label" id="text-text_in-label" for="text-text_in">Text</label>
<textarea id="text-text_in" class="form-control"></textarea>
</div>
<pre class="shiny-text-output noplaceholder" id="text-text_out"></pre>

<br>

Other times you’ll see it written explicitly (with **`NS(namespace = id, id = "[name]")`**).

``` r
text_example_02_UI <- function(id) {
  tagList(
    textAreaInput(inputId = NS(namespace = id, 
                               id = "text_in"), 
                  label = "Text"),
    verbatimTextOutput(outputId = NS(namespace = id, 
                               id = "text_out"))
  )
}
text_example_02_UI(id = "text")
```

<div class="form-group shiny-input-container">
<label class="control-label" id="text-text_in-label" for="text-text_in">Text</label>
<textarea id="text-text_in" class="form-control"></textarea>
</div>
<pre class="shiny-text-output noplaceholder" id="text-text_out"></pre>

<br>

As you can see, both methods work. This is because “*if **`id`** is missing, \[**`NS()`**\] returns a function that expects an **`id`** string as its only argument and returns that **`id`** with the namespace prepended.*”

To see what the unique ‘location’ for a namespaced ID looks like, we can define our namespace in a string (“text”), then create a new object from **`NS()`** (**`name_Spaceded()`**) and examine it’s contents:

``` r
# define a namespace
id <- "text"
# create the function
name_Spaceded <- NS(namespace = id)
# view the contents
name_Spaceded
```

    ## function (id) 
    ## {
    ##     if (length(id) == 0) 
    ##         return(ns_prefix)
    ##     if (length(ns_prefix) == 0) 
    ##         return(id)
    ##     paste(ns_prefix, id, sep = ns.sep)
    ## }
    ## <bytecode: 0x7fe6365d39f0>
    ## <environment: 0x7fe63eb85b30>

As we can see, the function created by **`NS()`** is pasting together two strings: the **`ns_prefix`** (the namespace prefix) and the **`id`**.

So if we supply an empty **`id`** to **`name_Spaceded()`**

``` r
# empty id
name_Spaceded(id = "")
```

    ## [1] "text-"

We can see the first **`id`** we passed was the name for the namespace. When we supply a character string to **`name_Spaceded()`** (imitating an **`inputId`** or **`outputId`**)…

``` r
# "namespace" the id
name_Spaceded("text_in")
```

    ## [1] "text-text_in"

**`name_Spaceded()`** has replaced what would be **`input$text_in`**–shared globally across our entire application–with **`input$text_in`**, which is isolated in the **`text`** module.

<br>

## Module ‘directory’ structure

<br>

Just like we can’t have identical file names in the same folder, we can’t have two IDs with the same name in the same app. To get around this, we create modules by pairing **`NS()`** with **`moduleServer()`**:

-   In the UI module, **`NS()`** isolates the IDs into a namespace (pasting together **`ns_prefix`** and **`id`**), which we can access via **`moduleServer()`**.

-   In the server module, the **`moduleServer()`** function includes both **`id`** and **`module`** arguments. The **`id`** will be linked to it’s complimentary UI function, and **`module`** is defined just like the standard shiny **`server`** function (**`function(input, output, session)`**). There is also a **`session`** argument, but it’s almost always set to the default value.

Below is an application folder-tree that mimics how IDs are contained within a module namespaces:

<br>

``` r
  app/
    └── module-plot/ # plot module UI namespace 
            └── UI/ 
            │   ├── inputId=NS(namespace = id, id = "x_variable")
            │   ├── inputId=NS(namespace = id, id = "y_variable")
            │   ├── inputId=NS(namespace = id, id = "color_variable")
            │   └── outputId=NS(namespace = id, id = "table")
            └── server/ # plot module (server) 
                └── moduleServer(id = id, 
                                 module = function(input, output, session))
```

<br>

1.  Within our **`app/`** directory, we have the **`plot`** module with **`inputId`**s (referenced in the server as **`input$`**) and an **`outputId`** (referenced in the server as **`output$`**)

2.  These IDs must be unique within the module’s namespace (created with **`NS()`**), but they no longer have to be unique within the app)

3.  We use the **`NS()`** function to isolate and name the IDs into a namespace (i.e. **`id = "[name]"`**), then we access these IDs in the server with **`moduleServer()`** (which we will cover below).

<br>

## Example UI

<br>

Below is an example UI function for a module with input and output functions and their IDs (**`input$x_variable`**, **`input$y_variable`**, and **`output$plot`**).

``` r
example_module_UI <- function(id) {
    tagList(
        # input
        selectInput(
            inputId = NS(namespace = id, 
                         id = "x_variable")
            # ...additional arguments omitted...
            ),
        # input
        selectInput(
            inputId = NS(namespace = id, 
                         id = "y_variable")
            # ...additional arguments omitted...
            ),
        # input
        selectInput(
            inputId = NS(namespace = id, 
                         id = "color_variable")
            # ...additional arguments omitted...
            ),
        # output
        plotOutput(
            outputId = NS(namespace = id, 
                         id = "plot"))
    )
}
```

<br>

## Example server

<br>

When we build the server component, we include a call to **`moduleServer()`**, and assume the server will have access to the same inputs and outputs defined in the UI:

<br>

``` r
example_module_Server <- function(id) {
    
  moduleServer(id = id, module = function(input, output, session) {
      
      # data reactive
      data <- reactive(
          select(
            <data>,
            all_of(c(input$x_variable, 
                     input$y_variable, 
                     input$color_variable))
          )
        )
      
      # render plot
      output$plot <- Output({
          data()
      })
      
  })
}
```

<br>

## Example app

<br>

When we build the app, we know the **`id`** argument will be shared between the UI (**`example_module_UI()`**) and server (**`example_module_Server()`**) functions:

``` r
ui <- fluidPage(
  example_module_UI(id = "example")
)

server <- function(input, output, session) {
  example_module_Server(id = "example")
}

shinyApp(ui, server)
```

<br>

***The link between the IDs created with **`NS()`** in the **`ui`** and the **`moduleServer()`** function in the **`server`** is the **`id`**.***

## Recap

Modules make it easier to combine inputs and outputs into the same app without having to worry about [namespace collision](https://en.wikipedia.org/wiki/Naming_collision). Read more about modules in [Engineering Production-Grade Shiny Apps](https://engineering-shiny.org/structuring-project.html#using-shiny-modules) and [Mastering Shiny](https://mastering-shiny.org/scaling-modules.html).
