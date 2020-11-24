---
layout: page
title:  "Shiny Tutorial"
---



# Using Interactive Shiny Apps for Twitter Data Analysis: An introductory Tutorial
Author: Jason Young,
Date: 12.11.2019

# Abstract
This tutorial describes the theory, development and practical use of Shiny, which is an R-package for building interactive web applications [apps]. It builds on the practical case of developing a Shiny app to collect and analyze Twitter data. With a hands-on approach the tutorial guides through the programming basics and the development of the app, which is available at: https://jason-young.shinyapps.io/twitter-analysis.

# Introduction

The goal of this tutorial is to provide an understanding of the functionalities and operating principles of Shiny. This will be demonstrated based on a practical case by developing a basic Shiny app that makes it easy for non-tech-savvy users to scrape and analyze Twitter data. 

In particular, the tutorial will provide a hands-on introduction with codes and practical tutorials to answer the following two questions:

1) What are the functionalities, operating principles and capabilities of Shiny apps?
2) How can a Shiny app facilitate Twitter data collection and analysis for non-tech-savvy users?

To start off, the tutorial will give some background on Shiny apps and their application possibilities in the Background Chapter. The How-To Chapter then provides an introductory tutorial on building Shiny apps and the Practical Application Chapter sets out the development of the Twitter Mining and Analysis SMO-TMAS app.

# Background

## What is Shiny
Shiny is a complementary package for R, which allows to convert R-scripts into user-friendly, interactive web apps. It was developed by Winston Chang, Joe Cheng, J. J Allaire, Yihui Xie and Jonathan McPherson and is available for free on [*GitHub*](https://github.com/rstudio/shiny) and on the Comprehensive R Archive Network [[*CRAN*](www.CRAN.R-project.org/package=shiny)], the official packages repository for R. Shiny allows to build apps straight from R without having to use any other web programming languages. In other words, no HTML/CSS/JavaScript or other web development knowledge is required to develop a Shiny app. These apps can be maintained by a local computer or by a web server running R. Hosting apps on a web server is particularly user-friendly, as users can easily interact with the app through a browser.

## How can Shiny be Used
Shiny is a basic but very flexible way of making data available and it can be used in a variety of settings. Both simple and easy to develop apps as well as complex interactive apps with customized CSS and JavaScript elements can be developed with Shiny. An overview of existing Shiny apps can be found in the official [*Shiny Showcase*](https://shiny.rstudio.com/gallery/) on the Shiny web site. In addition, there is a vibrant community of Shiny app developers who share their code and knowledge on [*GitHub*](https://github.com/), [*Stack Overflow*](https://stackoverflow.com) or other web sites.

Shiny has been used successfully to facilitate learning and teaching in various academic disciplines such as [*Statistics*](https://www.tandfonline.com/doi/full/10.1080/10691898.2018.1436999), [*Economics*](https://www.hec.edu/en/knowledge/instants/how-hec-professors-enhance-their-research-and-courses-using-data-visualization-app) or Computational Social Sciences and Shiny apps are also a great way to teach R skills themselves. The [*learnr package*](https://rstudio.github.io/learnr/) for example facilitates the learning of the R programming language by using interactive Shiny apps as R tutorials. Shiny is also a valuable research tool, as it allows to easily share data within groups of scientists without having to export the data into other formats or programs. In addition, Shiny can be used for interactive presentation and replication of results in publications. 
While it is already best practice to make research data available on open-access databases and repositories, interactive Shiny interfaces allow for rapid re-analysis and visualization of result. As an example, the [*Shiny app*](https://blakemcshane.shinyapps.io/spmeta) of McShane and Bockenholt about single paper meta-analysis in behavioral research provides an interactive and easy-to-use tool for other researchers to implement their results. On top, Shiny apps can also be used for [*experiments*](https://www.r-bloggers.com/programming-simple-economic-experiments-in-shiny/). Users can for example be exposed to textual or audiovisual stimuli and questions and their response can be tracked directly in the app (e.g. by text inputs, slider inputs and action buttons etc.). Shiny even offers the possibility to easily develop adaptative surveys where the questions evolve depending on previous answers. Shiny apps can also be used to gather user information or data such as in the app, which was developed within the course of this tutorial. Shiny is also a potent business tool that allows innovative data monitoring and analysis through easy implementation of interactive data visualizations and dashboards. [*Dashboards*](https://www.inwt-statistics.de/blog-artikel-lesen/best-practice-entwicklung-robuster-shiny-dashboards-als-r-pakete.html) are particularly well suited for quick visualizations of raw data, aggregation of information and analytical results in business contexts.

In a nutshell, Shiny apps can be characterized as:

* dynamic web interfaces for R accessible via browser;
* highly customizable without any knowledge of web development;
* allowing for instant modification and user feedback; 
* facilitating visualization of raw data and results of data analysis;
* letting users run their own data analysis or visualization without any knowledge of R;
* enhancing dynamic and interactive data dissemination and replication; and
* allowing to collect data from app-users.


# How to Build a Basic Shiny App

There is a large and active Shiny developer community and there are many useful teaching aids and tutorials to learn how to build Shiny apps for free such as [*Shiny Fundamentals with R*](https://www.datacamp.com/tracks/shiny-fundamentals-with-r) provided by DataCamp, [*Learn Shiny*](https://shiny.rstudio.com) available on the official Shiny web site or [*Building Shiny Apps*](http://deanattali.com/blog/building-shiny-apps-tutorial/) a by Dean Attali. R Studio also provides a handy [*cheatsheet*](https://rstudio.com/resources/cheatsheets/) on the RStudio web site to remember all the specific coding details. In addition, the Shiny package also has eleven built-in, self-contained Shiny apps as examples to demonstrate how Shiny works. The examples can be accessed with the following code:
```r eval=FALSE, include=TRUE
library(shiny)
runExample()
#Shows the name of the 11 examples
runExample("02_text")
#Selecting desired example
```
Note: The code chunks in this tutorial build on each other and should therefore be executed in preceding order to to ensure proper functionality.

## Basic Structure of a Shiny App
Even though elaborated Shiny apps can get very complex, each Shiny app fundamentally consists of the following three main components:

1) User Interface [UI] component: Consisting of specific input and output functions written in R generating HTML code for the layout of the app;
2) Server Instruction component: Contains instructions on how to assemble inputs into outputs in order to build an app; and
3) shinyApp component: Knits the UI and server instructions together using to create Shiny app objects.

Template that will launch an empty app that serves as foundation for every Shiny app:
```{r eval=FALSE, include=TRUE}
library(shiny)
ui <- fluidPage() 
# 1) UI component consisting of *Input() functions and *Output() functions 
# written in R generating HTML code
server <- function(input, output) {}
# 2) Server instructions component
shinyApp(ui = ui, server = server)
# 3) shinyApp component
```

## User Interface [UI] Component

### Customizing the Layout of the UI Component
Shiny uses the function fluidPage to create a display that automatically adjusts to the dimensions of a user’s browser window. The UI of an app can be customized by placing the following elements in the fluidPage function ([R Shiny 2019](https://shiny.rstudio.com/tutorial/written-tutorial/lesson2/)):

* Title: `titlePanel()`;
* Sidebar: `sidebarLayout(sidebarPanel(),mainPanel())`;
* Multi-page layout including a navigation bar: `navbarPage()`; and
* Grid system layout: `fluidRow(column())`.

More advanced layout options are shown in the [*Shiny Application Layout Guide*](https://shiny.rstudio.com/articles/layout-guide.html) available on the official Shiny web site. 

After choosing a basic layout option, further content can be added to a Shiny app by placing Shiny’s HTML tag functions inside the above listed Panel functions. These HTML tag functions parallel common HTML5 tags. The full Shiny HTML tags glossary can be found in the `shiny::tags` object, which contains 110 HTML tags:
```{r eval=FALSE, include=TRUE}
shiny::tags
# Shows all 110 HTML tags available to customize the UI
```

These HTML tags are added as arguments to the `fluidPage()` section in the UI component as shown in the following code block, which creates a customized multi-page layout app:
```{r eval=FALSE, include=TRUE}
ui <- fluidPage(
navbarPage('',                                             # No title on top of the page
  tabPanel('Home', strong('Developing a web-based Twitter Mining and Analysis Suite
                          using R and Shiny'),             # Bold title 
          p(''),                                           # Empty paragraph
          p(em('Term paper written by Jason Young')),      # Italics
          p(em('Supervisor: Dr. Cornelius Puschmann')), 
          p('This app was developed by Jason Young to show the UI layout options 
            of Shiny.', style='font-family:arial; font-size:10pt; color:red'), 
          p('The first tab', span('HTML tags',             # Playing with style options
             style ='color:blue'), 'shows useful Shiny HTML tags'), p('The second tab',                           
             span('Links', style='color:blue'), 'inlcudes helpful links to build 
             Shiny apps'), align = 'center'),              # Aligning to center
  tabPanel('HTML tags', h1('List of useful Shiny HTML tags'), 
     tags$ul(tags$li('A paragraph of text:', span('p', style='color:red')),
                                                           # Creating a list with bullets
     tags$li('A first level header:',  span('h1', style='color:red')),
                                                           # Creating red HTML tags
     tags$li('A hyper link:', span('a', style='color:red')),
     tags$li('A line break:',  span('br', style='color:red')),
     tags$li('A division of text with a uniform style:', span('div', style='color:red')), 
     tags$li('Italicized text:',  span('em', style='color:red')),
     tags$li('An in-line division of text with a uniform style:',  
             span('span', style='color:red')),
     tags$li('Bold text:',  span('strong', style='color:red')),
     tags$li('A formatted block of code:',  span('code', style='color:red')))),
  tabPanel('Links', h1('List of useful Shiny links'), a('www.shiny.rstudio.com'), br(),                 
           a('www.rstudio.com/resources/cheatsheets/'), br(),                                              
           a('www.datacamp.com/tracks/shiny-fundamentals-with-r'), br(),                                    
           a('www.deanattali.com/blog/building-shiny-apps-tutorial/'))))
                 # Creating a list of links without bullets and line breaks in between
server <- function(input, output) {}
shinyApp(ui = ui, server = server)
```

### Adding Inputs to the UI Component
Inputs are reactive objects (so called widgets) that the user of the app can toggle such as buttons, checkboxes, sliders, drop down menus or inputs such as text, files, numbers, dates or passwords.

Similar to the HTML tags the input functions are added as arguments to the `fluidPage()` section and take the following basic syntax:
```{r eval=FALSE, include=TRUE}
ui <- fluidPage(sliderInput(
# Specifying the input function (a slider in this case)
        inputId = 'num',
# Id to access the input function
# For internal purposes only - not shown in the app
# inputId = can also be omitted
        label = 'Choose a number',
# Label to explain the input function to the user 
# For external purposes - displayed near the input object
# Use empty character case (' ') to exclude label
        value = 25, min = 1, max = 100))
# Further input function specific arguments
# Specific arguments can be found by looking at the help page (e.g. ?sliderInput)
```

The most useful widgets to build Shiny apps are shown in the following code block, which recreates the customized app from above:
```{r eval=FALSE, include=TRUE}
ui <- fluidPage(
navbarPage('',
  tabPanel('Home', strong('Developing a web-based Twitter Mining and Analysis Suite
                          using R and Shiny'), p(''),
      p(em('Term paper written by Jason Young')), 
      p(em('Supervisor: Dr. Cornelius Puschmann')), 
      p('This app was developed by Jason Young to show the UI layout options of Shiny.',
             style='font-family:arial; font-si16pt; color:red'),
      p('The first tab', span('HTML tags',
        style ='color:blue'), 'shows useful Shiny HTML tags'), p('The second tab', 
        span('Links', style='color:blue'), 'inlcudes helpful links to build Shiny apps'), 
      p('The third tab', span('Widgets', style='color:blue'), 'shows useful widgets to  
            build Shiny apps'), align = 'center'),
  tabPanel('HTML tags', h1('List of useful Shiny HTML tags'), 
     tags$ul(tags$li('A paragraph of text:', span('p', style='color:red')), 
     tags$li('A first level header:',  span('h1', style='color:red')),
     tags$li('A hyper link:', span('a', style='color:red')),
     tags$li('A line break:',  span('br', style='color:red')),
     tags$li('A division of text with a uniform style:', span('div', style='color:red')), 
     tags$li('Italicized text:',  span('em', style='color:red')),
     tags$li('An in-line division of text with a uniform style:', 
             span('span', style='color:red')),
     tags$li('Bold text:',  span('strong', style='color:red')),
     tags$li('A formatted block of code:',  span('code', style='color:red')))),
  tabPanel('Links', h1('List of useful Shiny HTML tags'), a('www.shiny.rstudio.com'), br(), 
           a('www.rstudio.com/resources/cheatsheets/'), br(), 
           a('www.datacamp.com/tracks/shiny-fundamentals-with-r'), br(), 
           a('www.deanattali.com/blog/building-shiny-apps-tutorial/')),
    tabPanel('Widgets', h1('Basic widgets'),
          h3("Buttons"),actionButton("action", "Action"),br(),br(),submitButton("Submit"),
          h3("Single checkbox"),checkboxInput("checkbox", "Choice A", value = TRUE),
          checkboxGroupInput("checkGroup", h3("Checkbox group"),
                             choices = list("Choice 1" = 1, 
            "Choice 2" = 2,"Choice 3" = 3), selected = 1),
          dateInput("date", h3("Date input"), value = "2019-10-08"),
          dateRangeInput("dates", h3("Date range")),
          fileInput("file", h3("File input")),
          h3("Help text"),helpText("Note: help text isn't a true widget,", 
            "but it provides an easy way to add text to", "accompany other widgets."),   
          numericInput("num", h3("Numeric input"), value = 1),
          radioButtons("radio", h3("Radio buttons"), choices = list("Choice 1" = 1, 
                                        "Choice 2" = 2, "Choice 3" = 3),selected = 1),
          selectInput("select", h3("Select box"), choices = list("Choice 1" = 1, 
                                       "Choice 2" = 2,"Choice 3" = 3), selected = 1),
          sliderInput("slider1", h3("Sliders"), min = 0, max = 100, value = 50),
          sliderInput("slider2", "", min = 0, max = 100, value = c(25, 75)),
          textInput("text", h3("Text input"), value = "Enter text..."))))
server <- function(input, output) {}
shinyApp(ui = ui, server = server)
```

### Adding Outputs to the UI Component
Outputs are reactive objects that the users see as a result of their inputs such as:

* Tables: `tableOutput()`, `dataTableOutput()`
* raw HTML: `htmlOutput()`
* Images: `imageOutput()`
* Plots: `plotOutput()`
* Text: `textOutput()`, `verbatimTextOutput()`
* Shiny UI elements: `uiOutput()`

The output functions are also added as arguments to the `fluidPage()` section and take the following basic syntax:
```{r eval=FALSE, include=TRUE}
ui <- fluidPage(plotOutput(
# Specifying the output function (a plot in this case)
outputId = 'hist',
# Id to identify the output function
# For internal purposes only - not shown in the app
# outputId = can also be omitted
...))
# Further output function specific arguments
# Specific arguments can be found by looking at the help page (e.g. ?plotOutput)
```
Placing an output function in the UI component tells Shiny where to display an object. But the instructions of how to build the object are still missing. These instructions have to be provided in the server component as shown below.

## Server Component 
The server function contains instructions on how to assemble inputs into outputs. It plays a special role in the Shiny process as it builds a list-like object that contains all of the code needed to update the R objects in an app. Each R object needs to have its own entry in the list (see [R Shiny 2019](https://shiny.rstudio.com/tutorial/written-tutorial/lesson7/)).

There are three basic rules to write the server function:

1) Save output objects to `output$`: Output objects have to be saved using `output$`;
2) Build the output with `render()`: Output objects have to be built with a render function; and
3) Save input objects to `input$`: Input values have to be accessed with `input$`.

These rules are explained separately and in more detail in the following three sub-sections.

1) Saving output objects to `output$`

To include outputs in a Shiny app, the output objects specified in the UI component have to be saved in the server function as `output$outputId`:
```{r eval=FALSE, include=TRUE}
server <- function(input, output) {
output$hist <- ... }
# Using the outputId specified in the UI component
```

2) Building objects to display with render 

The render function assembles output objects into HTML for their final appearance and keeps track of their reactions and reactivity. In other words, the server function creates reactivity by using inputs to build rendered outputs ([R Shiny 2019](https://shiny.rstudio.com/tutorial/written-tutorial/lesson4/)). 

The following `render()` functions are available:

* Tables (data frame, matrix, other table like structures): `renderTable()`, `renderDataTable()`
* Raw html: `htmlOutput()`
* Images (saved as a link to a source file): `renderImage()`
* Plots: `renderPlot()`
* Text: `renderText()`
* Shiny UI elements such as Shiny tag objects or HTML: `renderUI()`
* Code blocks of printed output: `renderPrint()`

Each output entry in the UI component should contain the output of one of the above render functions and each render function takes a single argument: An R expression surrounded by braces `{}` as shown in the following code chunk. The expression will be re-run every time a user changes the value of a widget in order to update the output object ([R Shiny 2019](https://shiny.rstudio.com/tutorial/written-tutorial/lesson4/)).
```{r eval=FALSE, include=TRUE}
server <- function(input, output) {
output$hist <- renderPlot({  })
}
# Specifying type of object to build
# Rendering hist as a plot in this case
# Hist has to be specified as output in the UI component
```

3) Saving input objects to `input$`:

To create an interactive Shiny app, the values of input objects have to be connected to output objects. The current values of input objects are saved under the inputId specified in the UI component. To customize outputs based on these current input values, they have to be saved in the server function as `input$inputId`. Shiny will then automatically make an object reactive if the object uses an input value. Shiny tracks which outputs depend on which widgets. When a user changes a widget, Shiny will rebuild all of the outputs that depend on the widget, using the new value of the widget as it goes. As a result, the rebuilt objects will be completely up-to-date [see @r_shiny_r_2017].

For example, the code junk below creates a basic Shiny app with a reactive histogram output by calling the value of a slider input function called num:
```{r eval=FALSE, include=TRUE}
library(shiny)
ui <- fluidPage(
titlePanel('Developing a web-based Twitter Mining and Analysis Suite
                          using R and Shiny'),
sidebarLayout( 
  sidebarPanel(
    p(em('Term paper written by Jason Young')), 
    p(em('Supervisor: Dr. Cornelius Puschmann')), 
    p('This app was developed by Jason Young to show how the server function assembles 
      inputs into outputs.', style='font-family:arial; font-size:16pt; color:red'), 
    p('The', span('slider input', style ='color:blue'), 'in the main panel on the right 
      can be', span('adjusted', style ='color:blue'), 'to customize the histogram')
    ),
  mainPanel(
    sliderInput(inputId = 'num', label = 'Choose a number',
                            value = 25, min = 1, max = 100), 
plotOutput(outputId = 'hist')
)))
# 1) UI component for a slider input and histogram output
server <- function(input, output) {
output$hist <- renderPlot({
hist(rnorm(input$num))
# Using the Id specified in the input function as input for the histogram 
# rnorm = n random normal values, where n is a number set by the user with the slider
  })}
#2 ) Server component with instructions for a slider input and histogram output
shinyApp(ui = ui, server = server)
# 3) shinyApp component
```

## How to share a Shiny app
Every Shiny app is maintained by a computer running R. In the template example above, the Shiny app runs on a local computer using the currently open R-session. However, as Shiny apps are embedded in a web page, the local computer can easily be replaced with a server. This can either be a local server or a web server and the app can be shared either as R script or as a web page.

No matter how one arranges to share a Shiny app, it has to be saved in a standardized format: The app either has to be contained in a single app.R script or as two separate scripts (ui.R and server.R), which live in one directory with every file the app needs. Saving the app in two separate files used to be the only method of creating a Shiny app and it can still be useful when building a more complicated app, as it allows us to separate the UI (ui.R) from the server logic (server.R).

### Sharing Shiny apps as R scripts
App.R scripts can be shared as files by directly sending them to specific users or by hosting them online. Shiny has three built in commands that make it easy to launch App.R scripts that are hosted online ([R Shiny 2019](https://shiny.rstudio.com/tutorial/written-tutorial/lesson7/)): 

* `runUrl()`: Will download and launch a Shiny app in R straight from a weblink. To use `runUrl`, a Shiny app’s directory has to be saved as a zip file which is hosted at its own link on a web page. Anyone with access to the link can launch the app from inside R by running the following code:
```{r eval=FALSE, include=TRUE}
runUrl( "<specific weblink>")
```
* `runGitHub()`: Will download and launch a Shiny app in R straight from [*GitHub*](www.github.com). To share an app through GitHub, an app.R file has to be stored in a GitHub project repository, along with any supplementary files. Users can launch the app by running:
```{r eval=FALSE, include=TRUE}
runGitHub( "<specific repository name>", "<specific user name>")
```
* `runGist()`: Will download and launch a Shiny app in R straight from [*GitHub Gist*](https://gist.github.com). Gist is a service offered by GitHub that allows to share files online, in anonymous way. To  share an app via Gist the app.R file has to be uploaded to the gist web page which will then provide an anonymous URL to share the app, which can be launch with the following code:
```{r eval=FALSE, include=TRUE}
runGist("<specific gist number>") 
#"<specific gist number>" is the number that appears at the end of Gist’s web address.
```

### Sharing Shiny apps as web pages
Shiny apps can also be shared as web pages by hosting them at an own URL. This is one of the great advantages of using Shiny and definitely the most user-friendly way of sharing apps. Users can simply interact with the app through a web browser without having to use R at all. R Studio offers two easy ways to host Shiny apps as web pages:

* [*Shiny Server*](https://github.com/rstudio/shiny-server/blob/master/README.md): Shiny Server is a server program that builds a web server designed to host Shiny apps. Shiny Server is a free, open source program available from the R Studio GitHub repository. At the time that this tutorial was written, Shiny Server was only explicitly supported for Linux servers (Ubuntu 14.04 and CentOS/RHEL 6 (64 bit) or greater).
* [*shinyapps.io*](www.shinyapps.io): Shinyapps.io is a dedicated server for Shiny apps hosted by RStudio. This is the easiest way to turn a Shiny app into a web page while still giving developers complete control over their apps. The free tier includes 5 apps and 25 active hours per month. Paid subscription plans for more apps and more active hours range from 9-299 Dollars a month. With an active [*shinyapps.io*](www.shinyapps.io) account, apps can be deployed directly from an active R session by clicking on the blue publish button in the top right corner of the interface.

After establishing the Shiny app basics, the remainder of this tutorial is dedicated to a practical application by creating a Shiny app to scrape and analyze Twitter data.

# Practical Application: Developing a Shiny App to Collect and Visualize Twitter Data 

Over the past decade, social media have created a new form of public political interaction that is distorting the boundaries of long-established processes of political discourse, information diffusion and agenda-setting in Western democracies ([Weimann and Brosius 2017: 76](http://www.jbe-platform.com/content/journals/10.1075/asj.1.1.06wei)). Twitter plays a special role in this transformation and has become an integrated part of an emerging hybrid political communication system in which political issues are publicly negotiated and commented by journalists, political elites and ordinary citizens ([Stier et al. 2018: 51](https://www.tandfonline.com/doi/full/10.1080/10584609.2017.1334728); [Ausserhofer and Maireder 2013: 291](http://www.tandfonline.com/doi/abs/10.1080/1369118X.2012.756050)). The widespread use of social media has not only transformed the public sphere but also created vast amounts of data traces, which allows researchers to monitor and compare human behavior and communication at unprecedented sizes and time scales. This has led to an emerging field of computational social sciences, which seeks to test hypotheses drawn from social science theories with the use of large-scale digital data sets ([Lazer et al. 2009](http://www.sciencemag.org/cgi/doi/10.1126/science.1167742); [González-Bailón 2013](http://doi.wiley.com/10.1002/1944-2866.POI328)). At the same time, due to recent data breach scandals such as Cambridge Analytica, access to social media data is increasingly restricted by platform providers through tightened controls and rising technological hurdles (see [Twitter 2017](https://blog.twitter.com/en_us/topics/company/2017/Update-Russian-Interference-in-2016--Election-Bots-and-Misinformation.html)). This shift provides new challenges for social scientists who seek to analyze public posts with a purpose of better understanding human interaction and leads to a growing gap between scholars, who have been doing more qualitative research, and computational social science scholars, who work with big data. 

Based on numerous discussions with researchers in the course of the establishment of the Social Media Observatory as part of the [*Research Institute for Social Cohesion*](https://www.hans-bredow-institut.de/en/news/hans-bredow-institut-will-contribute-to-the-institut-fuer-gesellschaftlichen-zusammenhalt) and a [*review of existing Twitter mining tools*](https://github.com/Leibniz-HBI/Social-Media-Observatory/wiki/Twitter-Tools), it was determined that the main impediment to integrate social media data into qualitative scholarly research is the unavailability of suitable tools for performing common data mining tasks and intuitive data visualization methods that do not require any programming knowledge.

Based on these considerations, a simple Shiny app was developed to bridge the growing gap between qualitative and computational social scientists by allowing non-tech-savvy users to mine and analyze Twitter data with the simple click of a button. In particular, the goal of the app is to assist researchers in exploratory data analysis by facilitating the collection, initial analysis and interpretation of small Twitter data sets. 

The subsequent sections of this tutorial describes the creation of this app, following the four basic steps of app development for dynamic data visualization according to Ellis and Merdian ([2015](https://www.frontiersin.org/article/10.3389/fpsyg.2015.01782)):

1) Data preparation and processing
2) Data analysis and visualization
3) Development and testing
4) Deploying application

## Data Preparation and Processing

To enable easy and customized data access the app has to be connected to Twitter's API with an existing R-package that allows users to pull tweets of specified Twitter handles and tweets containing specified keywords. At the time that the app was set-up (October 2019), there were four well-known R-packages available to access Twitter's API:

* [*rtweet*](https://rtweet.info): A package designed by Michael W. Kearney to connect to Twitter's REST and stream APIs. The package is updated regularly and provides access to Twitter’s REST and stream APIs.
* [*twitteR*](https://github.com/geoffjentry/twitteR): A package designed by Jeff Gentry to connect to Twitter's REST API. The package was not updated since 2016 as it was officially replaced by rtweet.
* [*streamR*](https://github.com/pablobarbera/streamR): A package designed by Pablo Barbera to connect to Twitter's stream API. The package is deprecated as it was last revised in 2018.
* [*RTwitterAPI*](https://github.com/joyofdata/RTwitterAPI): A package designed by Raffael Vogler to connect to Twitter's REST API. The package is deprecated as it was not updated since 2014.

After reviewing all packages, it was decided to harvest tweets with `rtweet` (see [Kearney 2019](https://cran.r-project.org/package=rtweet)) as it is the only regularly updated package, which allows to connect both to Twitter’s REST and stream APIs. In order to seamlessly embed `rtweet into the app, functions have to be programmed that automatically enable the Twitter authentication and API query. The code development of these functions is shown in the next sections.

### Enabling Twitter Authentication 

To enable automatic Twitter authentication and ensure connection stability an official app was created and registered with [Twitter](https://developer.twitter.com/en/docs/basics/getting-started). The approved app can then access Twitter’s API with a token-based authentication method called [Open Authentication](https://blog.twitter.com/en_us/topics/company/2017/Update-Russian-Interference-in-2016--Election-Bots-and-Misinformation.html) and the following code was created to automatically enable the handshake between the app and Twitter:
```{r eval=FALSE, include=TRUE}
# Installing package and loading library
if(!require('rtweet')) install.packages('rtweet')
library('rtweet')

# Twitter authentication <- Insert app keys into the PLACEHOLDERS
token <- create_token(
  app = "PLACEHOLDER", 
  consumer_key = 'PLACEHOLDER',
  consumer_secret = 'PLACEHOLDER',
  access_token = 'PLACEHOLDER',
  access_secret = 'PLACEHOLDER')
```
  
### Collecting Tweets of Specific Twitter Handles 

Next, a function that queries the GET statuses/user_timeline endpoint and creates a data frame containing the results was created in the code chunk below to enable users of the app to harvest tweets of specific twitter handles. Feedbacks were included for ease of use to prompt the app users when the tweet collection starts and finishes. The GET statuses/user_timeline endpoint accepts 1'500 requests per 15 minutes and returns up to 3'200 tweets per request. In addition, there is a limit of 100'000 requests per day to the [statuses/user_timeline endpoint](https://developer.twitter.com/en/docs/basics/rate-limits). Hence, to prevent too many rate limit violations the number per request was limited to one user and 3'200 tweets per request and the respective rate limitations were disclosed on the homepage of the app [(see Development and Testing below)](#development-and-testing). 
```{r eval=FALSE, include=TRUE}
pull_tweets <- function(twitter_handle, n){
  # withProgress(message = "Collecting tweets...", value = 0, { Prompting the user that 
  # tweet collection has started <- only works when Shiny app is running
  tweets <- get_timelines(twitter_handle,        # twitter_handle = text input by user                                          
                          n = n,             # n =  specified number of tweets by user
                          excludeReplies = TRUE)
                                         # Excluding replies (i.e. only source tweets)
  # incProgress(message = "Finished collecting tweets!")              # Prompting user
  #showNotification("Data has been collected - Please change tab to see analysis!")
  return(tweets)
  #})            To include prompting messages <- only works when Shiny app is running
}
```

### Collecting tweets containing specific keywords / hashtags

In addition, a function that queries the GET search/tweets endpoint and creates a data frame containing the results was created in the code chunk below so that users of the app can harvest tweets containing specific key words and hashtags. The [search/tweets endpoint](https://developer.twitter.com/en/docs/basics/rate-limits) returns up to 18'000 tweets per request and accepts up to 450 requests per 15 minutes window. Hence, to prevent too many rate limit violations the number per request was limited to one key word and 18'000 tweets per request and the respective rate limitations were disclosed on the homepage of the app [(see Development and Testing below)](#development-and-testing).
```{r eval=FALSE, include=TRUE}
pull_topics <- function(twitter_topic, n){
  #withProgress(message = "Collecting tweets...", value = 0, {    # Prompting user
  tweets <- search_tweets(twitter_topic,     # twitter_topic = text input by user
                         n = n,         # n =  specified number of tweets by user                                                  
                         include_rts = FALSE,               # Excluding retweets
                         retryOnRateLimit=120)                                       
                                        # wait and retry when rate limit reached
  #incProgress(message = "Finished collecting tweets!")         # Prompting user
  #showNotification("Data has been collected - Please change tab to see analysis!")
  return(tweets)
  #})     # To include prompting messages <- only works when Shiny app is running
}
```

### Collecting Live Tweets

Lastly, a function that queries Twitter's STREAM API was set-up, to allow users to fetch live tweets. Again, prompting messages informing the users about the start and end of the data collection process were included as shown the code block below. Twitter rate limits cap the number of live tweet search results to [1% of the total volume of tweets at that moment](https://www.cambridge.org/core/elements/twitter-as-data/27B3DE20C22E12E162BFB173C5EB2592). In other words, as long as the filter query accounts for less than 1% of the total pool of data, every single tweet containing the matching keywords is received. This 1% sample includes up to 5 million tweets per day as Twitter is one of the largest social networks with more than [500 million tweets per day in Q3 2018](https://www.omnicoreagency.com/twitter-statistics/). To prevent rate limit violations and save computational resources, the number of seconds for collecting live tweets was limited to a maximum of 40 seconds and one key word per request and the respective limitations were disclosed on the homepage of the app [(see Development and Testing below)](#development-and-testing).
```{r eval=FALSE, include=TRUE}
pull_stream <- function(stream_topic, n){
  #withProgress(message = "Collecting tweets...", value = 0, {           # Prompting user
    tweets <- stream_tweets(stream_topic,             # stream_topic = text input by user
                            timeout = n)  # n =  specified number of tweets by user                                 
    #incProgress(message = "Finished collecting tweets!")            # Prompting the user
    #showNotification("Data has been collected - Please change tab to see analysis!")
    return(tweets)
  #})    # To include prompting messages <- only works when Shiny app is running
}
```

The following code chunk tests whether the functions are set up correctly by scraping the last 200 tweets of the Twitter handle 'BredowInstitut' and the hashtag 'LeibnizWGL' as well as collecting live Tweets containing the key word 'NBA' for 20 seconds:
```{r eval=FALSE, include=TRUE}
temp_tweets <- pull_tweets("BredowInstitut", 200)
temp_tweets
temp_topics <- pull_topics("LeibnizWGL", 200)
temp_topics
temp_stream <- pull_stream("NBA", 90)
temp_stream
```

## Data Analysis and Visualization
In this section, suitable analysis and visualization methods are developed that can be embedded in the app to assist the end-users in exploratory data analysis. For this purpose, not only the developer perspective has to be incorporated but also the user needs have to be taken into account in order to get the best representation of the data within the app afterwards. Hence, the requirements for the data analysis and visualization methods as well as available tools are assessed before determined and coding the respective functions.
 
Based on discussions with possible app users as well as practicability considerations it was determined that the data analysis and visualization need to satisfy the following basic requirements:

* Interactivity: Users should be able to toggle and customize the charts easily.
* Responsiveness and fast performance: Charts should run smoothly across all devices when users pull fresh Twitter data.
* Drill-down charts and tables: It should be possible for users to break down aggregated data into more detail by searching and sub-setting.
* Synchronization:  Multiple charts have to be synchronized when users pull new tweets.
* Exporting: Built-in exporting of the gathered data and the visualization charts adds an extra advantage as end-users often need to share the results of their analysis. This export should be integrated seamlessly so that there is no need to install any third-party plugins and the data should be exportable into a standardized formats.

In addition to Shiny and the standard R operations, the following R-packages were identified, which provide the necessary utilities to fulfill these requirements:

* [*Plotly*](https://plot.ly/r/): `Plotly Open Source Graphing Libraries` is an R package for creating interactive web-based graphics via the open source JavaScript graphing library plotly.js. Plotly graphs are rendered locally through the html-widgets framework and can be created either by transforming a [`ggplot2`](http://ggplot2.org) object via `ggplotly()` into a plotly object or by directly initializing a plotly object with `plot_ly()` [see Sievert 2018](https://plotly-r.com) for more details. Plotly provides the required interactivity and responsiveness as plotly graphs can easily be customized and downloaded by clicking and hovering over the graphs when accessing the app through a browser.
* [*DT*](https://rstudio.github.io/DT/): `DT` provides an R interface to the JavaScript library DataTables. This makes it possible to display R data objects (matrices or data frames) as interactive tables on HTML pages. Rendered HTML tables can be filtered, paginated, sorted and downloaded by the end-users directly in the web browser.
* [*crosstalk*](https://rstudio.github.io/crosstalk/): `Crosstalk` is an add-on to the [*htmlwidgets package*](https://www.htmlwidgets.org). It extends htmlwidgets with a set of classes and functions and allows advanced interaction controls for the HTML tables.
* [*dplyr*](https://dplyr.tidyverse.org) and [*stringr*](https://stringr.tidyverse.org): The `dplyr` and `stringr`  packages are part of the [*tidyverse*](https://www.tidyverse.org), an ecosystem of packages designed for easy data manipulation. The packages make it easy to clean the scraped Twitter data and to select useful information for exploration and visualization and rearrange the data so that it can be downloaded to a standardized .csv format.

The following existing Shiny apps with open-source code were reviewed in order to get some inspiration and guidelines on how to use these additional packages for suitable visualization methods:

* [*twittR*](https://github.com/evanm31/twittR): A Shiny app developed by Evan Moore (last revision in 2017), which creates a variety of informative figures based on the bag-of-words distribution of a corpus of Twitter data collected from either a user or a hashtag, including frequency bar plots and word clouds, a word correlation table, topic modeling and a topic distribution heat map as well as a random tweet generator.
* [*Twitter Use Analysis*](https://github.com/harpooooooon/RShiny-Dashboard-for-Twitter-Analysis): A Shiny dashboard developed by Murali Mohana Krishna Dandu (last revision in 2018), which creates charts to visualize the tweeting habits of specific Twitter handles and plots to measure the user engagement generated by each tweet.
* [*Twitter Talks, Shiny Sparks!*](https://medium.com/analytics-vidhya/twitter-talks-shiny-sparks-3ee28f41dc2b): A Shiny app developed by Nipunjeet Gujral (last revision in 2018), which plots token frequencies, word-clouds, sentiment analyses, bi-gram directional networks and correlation networks for a number of tweets with specific hashtags downloaded in September 2018.
* [*Twitter Analytics*](https://aj17.shinyapps.io/twitteranalytics/): A Shiny app developed by Azka Javaid (last revision in 2019), which creates word clouds, topic models, sentiment analyses, text and emoji analyses of user's live tweets along with mapped network analyses of user's followings/followers.

Building on these examples and the respective requirements, the following methods of data analysis and visualization were chosen for the scraped tweets:

* An interactive and downloadable table of all collected tweets;
* A bar chart depicting the days at which a tweet was created;
* A line plot depicting the time at which a tweet was created;
* A pie chart depicting the source from which a tweet was sent;
* A histogram depicting the distributions of the retweet and favorited count metrics;
* A word cloud depicting the 100 most used terms in the collected tweets;
* A word frequency table showing the terms and their frequencies in more detail;
* A network depicting the co-occurrences of the 50 most frequently mentioned users in the text of the scraped tweets; and
* A network depicting the co-occurrences of the 50 most frequently used hashtags in the text of the scraped tweets.

After establishing the most suitable methods of data analysis, the respective code is developed below.

### Data Wrangling 

A tweet is formed of attributes written in JSON (JavaScript Object Notation) structure and the Twitter API not only provides the tweet text itself but also additional metadata such as the username and numerical ID of the sender and a time stamp indicating when the tweet was sent. Further useful metadata include the user description, the number of followers and posted tweets as well as the user creation date. All captured features are described in more detail in the **Table** below.

**Table: Available Tweet Data** 

| **Information** | **Description** | **Source** |
| --- | --------------------- | --- |
| Text | Tweet text | Twitter API |
| Sender | Twitter username, real name and numerical ID | Twitter API |
| Time stamps | Tweet creation date and user creation date | Twitter API |
| Profile information | User description and URL included bio | Twitter API | 
| Total tweets | Total number of posts in account's lifetime | Twitter API |
| Followers | Number of followers of each user | Twitter API |
| Friends | Number of followees | Twitter API |
| Listed | Number of lists user has created in account's lifetime | Twitter API |
| Likes | Number of tweets user has liked in account's lifetime | Twitter API |
| Source | Client used to send the tweet | Twitter API |
| Language | Tweet and user language | Twitter API |
| Reply | User name and status ID if tweet was posted in reply | Twitter API |
| Location | Geo-location of the sender | Twitter API |
| Verified | Verified account status | Twitter API |

Note: All of the information is provided at the time of capturing the tweet.

The `pull_tweets`, `pull_topics` and `pull_stream` functions programmed in the Data Preparation and Processing Section [(see Collecting Tweets of Specific Twitter Handles](#collecting-tweets-of-specific-twitter-handles), [Section 4.1.3](#collecting-tweets-containing-specific-keywords-hashtags) and [Section Collecting Live Tweets)](#collecting-live-tweets), return a list of the collected tweets containing all of the data outlined in the above table. A set of useful information was extracted from this list using the data wrangling package `dplyr` and the tweet creation date converted to show weekdays and hours separately. In addition, a retweet/favorite ratio was calculated by diving the retweet through the favorite count. 
```{r eval=FALSE, include=TRUE}
# Installing packages and loading library
if(!require('dplyr')) install.packages('dplyr')
library('dplyr') #Data wrangling
if(!require('lubridate')) install.packages('lubridate')
library('lubridate') #Changing date format
# Selecting useful rows
data_wrangling <- function(temp_tweets){
temp_tweets%>% 
    dplyr::select(screen_name, description, text, created_at, retweet_count,             
                  favorite_count, source, hashtags, urls_expanded_url, media_type, 
                  mentions_screen_name, followers_count, friends_count, statuses_count, 
                  favourites_count, account_created_at, verified) %>%  
                                                              
    dplyr::mutate(retweet_favorite_ratio = retweet_count/favorite_count,   
                                                 # Calculating retweet/favorite ratio
                  day_of_week = wday(created_at, label = TRUE),    
                                           # Converting created_at to day of the week
                  hour = hour(created_at))            # Converting created_at to hour  
}
```   

### Creating an Interactive Table of the Collected Tweets

With the `DT` package the extracted list was then be converted into an interactive table, which can be sorted and filtered right on the apps' web site. In addition, a download button was added which makes it easy for the end-users to save the collected Twitter data. For this, the wrapped data was rendered as `DT:DataTable` and `downloadHandler` in the UI component as shown in the code below:
```{r eval=FALSE, include=TRUE}
# Installing packages and loading library
if(!require('DT')) install.packages('DT')
library('DT') #creating interactive tables that can display on HTML pages
# Rendering data table with DT to create an output in the UI component
interactive_table <-  DT::renderDataTable({data_wrangling(temp_tweets)})
# Creating download button in the UI component and setting-up function to save collected
  # data as .csv with specified Twitter handle and system date as file name
download <- downloadHandler(filename = function() {paste(input$twitter_topic, Sys.Date(),
                                         '.csv', sep="_")},content = function(file){
                                         save_as_csv(data_wrangling(temp_tweets), file)})
```  

The following code creates a basic Shiny app consisting only of the interactive table and download button to test wether the functions were set-up correctly:
```{r eval=FALSE, include=TRUE}
if(!require('DT')) install.packages('DT')
library('DT') #creating interactive tables that can display on HTML pages
# Creating interactive table of Tweets and download button
 ui = fluidPage(fluidRow(DT::dataTableOutput(outputId = "table"), 
                         downloadButton("download", "Download Tweets")))
    server = function(input, output) {
    output$table <- interactive_table
    output$download <- download}
    shinyApp(ui = ui, server = server)
```    

### Creating a Bar Chart Depicting the Days of the Week that the Tweets were Created

To get insights into to temporal structure of the collected data, a simple bar chart depicting the days of the week on the x axis and the number of tweets on the y axis was created using the `plotly` package. For this, the time stamp provided by Twitter's API (metadata: created_at), was mutated into weekdays (day_of_week) using the `dplyr` package as shown on the previous page [(see Data Wrangling)](#data-wrangling). Afterwards, the data was rearranged into a data frame, which only contains the days of the week (Var1) and the number of posted tweets per day (Freq) as shown in the code chunk below:
```{r eval=FALSE, include=TRUE}
if(!require('plotly')) install.packages('plotly')
library('plotly')                       # interactive plots
tweets_per_day <- function(temp_tweets){
  tweets <- data_wrangling(temp_tweets)
  freq_table <- data.frame(table(tweets$day_of_week)) 
                # forming a data frame of frequency per day
  plot <- plot_ly(x = freq_table$Var1,          # plotting                   
                  y = freq_table$Freq,
                  type = "bar") %>% 
    layout(title = "Tweets per Day of the Week")
  return(plot)}
```

### Creating a Line Plot Depicting the Time at which the Tweets were Created

To show the distribution of tweets over time, a scatter chart depicting hours on the x axis and the number of tweets on the y axis was created using plotly package. For this, the time stamp provided by Twitter's API (metadata: created_at), was mutated into hours (hour) using the `dplyr` package as shown in the above section on [(Data Wrangling)](#data-wrangling). Afterwards, the data was rearranged into a data frame, which only contains the hours (Var1) and the number of posted tweets per hour (Freq) as shown in the code chunk below:
```{r eval=FALSE, include=TRUE}
tweets_per_hour <- function(temp_tweets){
  tweets <- data_wrangling(temp_tweets)
  freq_table <- data.frame(table(tweets$hour))               
             # forming data frame of frequency per hour
  plot <- plot_ly(x = freq_table$Var1,      # plotting                   
                  y = freq_table$Freq,
                  type = "scatter",
                  mode = "line+markers") %>% 
      layout(title = "Distribution of Tweets in a Day",
           xaxis = list(title = "Hour"),
           yaxis = list(title = "Frequency"))
  return(plot)}
```

### Creating a Pie Graph Depicting the Source from which the Tweets were Sent

To get insights into to different web-clients used to send the tweets, a pie chart depicting the percentage allocation of the source of the tweets was created using the `plotly` package. For this, a data frame was formed containing the frequency and the tweet source provided by Twitter's API (metadata: source). Afterwards, the distribution of tweet sources was plotted as interactive pie chart using the function call `plot_ly()` as shown in the code chunk below:
```{r eval=FALSE, include=TRUE}
tweet_source <- function(temp_tweets){
  tweets <- data_wrangling(temp_tweets)
  temp_df <- data.frame(table(tweets$source)) # forming data frame of sources
  colnames(temp_df) <- c("source", "freq")     # changing column names accordingly
  pie <- plot_ly(data = temp_df,                                       # plotting
                 labels = ~source,
                 values = ~freq,
                 type = "pie",
                 textinfo = 'label+percent',
                 textposition = 'inside')%>%
    layout(title = "Distributions of Tweet Sources")
  return(pie)
}
```

### Creating a Histogram Depicting the Distributions of the Retweet and Favorited Metrics

Next, a function that creates a histogram depicting the distributions of the retweet and favorited count metrics was set-up. This was done by creating an overlay of histograms containing the number of retweets and favorites respectively. This histogram overlay was then plotted using plotly's `plot_ly` function call as shown in the following code lines:
```{r eval=FALSE, include=TRUE}
distributions <- function(temp_tweets){
  tweets <- data_wrangling(temp_tweets)
  plot <- plot_ly(data = tweets,         # plotting both RT and Fav distribution
                  alpha = 0.6) %>%
    add_histogram(x = ~retweet_count, name = "Retweet") %>%
    add_histogram(x = ~favorite_count, name = "Favorited") %>%
    layout(barmode = "overlay",
           xaxis = list(title = " "),
           title = "Distributions of Retweet and Favorited Counts")
  return(plot)
}
```

### Creating a Word Cloud

As shown in the code below, a word cloud was set-up using the quantitative text analysis package [`quanteda`](https://quanteda.io). For this, the tweet text was extracted and converted into an ASCII-character corpus. Transforming the tweet text to non-ASCII characters ensures that emoticons and other foreign or unknown characters are removed, which would not be represented properly in the word cloud. Next, the corpus was transformed into a document-feature matrix [DFM] object and the text was then cleaned by removing German, English, French and Italian stop words and words with low information value (symbols, numbers, punctuation etc.) as well as by word stemming. Finally, the `textplot_wordcloud` function was used to plot the DFM as a word cloud, where the feature labels are plotted with their sizes proportional to their numerical values in the dfm. Based on multiple trials the size of the smallest word `min_size` and the size of the largest word `max_size` were adjusted accordingly so that all of 100 words fitted in the app's text plot box. In addition, the [`RColorBrewer package](https://CRAN.R-project.org/package=RColorBrewer) was used to color the words according to their frequency in the text.
```{r eval=FALSE, include=TRUE}
if(!require('quanteda')) install.packages('quanteda')
library('quanteda') # text analysis
if(!require('stopwords')) install.packages('stopwords')
library('stopwords') # multilingual stop word lists
if(!require('RColorBrewer')) install.packages('RColorBrewer')
library('RColorBrewer') # color palettes used for word cloud
tweets_cloud <- function(temp_tweets){
  PostsCorpus <- iconv(temp_tweets,to = "ASCII",sub = "") 
  # Transforming text to remove non-ASCII characters (emoticons, foreign characters etc.)
  PostsCorpus <- corpus(PostsCorpus, text_field='text')  # Creating corpus of tweet text
  Corpus <- dfm(PostsCorpus, tolower = T, stem = T, remove_punct = T, remove_twitter = T, 
                remove_numbers = T, remove_symbols = T, remove_separators = T, 
                remove_hyphens = T, remove_url = T, remove = c(stopwords("en"), 
                stopwords("de"), stopwords("fr"), stopwords("it"), 'rt', 'na')) 
      # Creating dfm and cleaning text: removing stopwords and words with low info value
  textplot_wordcloud(Corpus, color=brewer.pal(8, "Dark2"), min_size = 0.5, max_size = 7,
                     max_words = 100) # Creating word cloud with max 100 words
  return(textplot_wordcloud)
}
```

### Creating a Word Frequency Table

To allow for more detailed insights, a word frequency table was set up using quanteda's `textstat_frequency` function, which returns a data frame of features and their term and document frequencies. As shown in the code lines below, the text was again cleaned by removing all non-ASCII characters, German, English, French and Italian stop words and words with low information value:
```{r eval=FALSE, include=TRUE}
word_frequency <- function(temp_tweets){
  PostsCorpus <- iconv(temp_tweets,to = "ASCII",sub = "") # Removing non-ASCII characters
  PostsCorpus <- corpus(PostsCorpus, text_field='text')   # Creating corpus of tweet text
  Corpus <- dfm(PostsCorpus, tolower = T, stem = T, remove_punct = T, remove_twitter = T, 
                remove_numbers = T, remove_symbols = T, remove_separators = T, 
                remove_hyphens = T, remove_url = T, remove = c(stopwords("en"), 
                stopwords("de"), stopwords("fr"), stopwords("it"), 'rt', 'na'))
       # Creating dfm and cleaning text: removing stopwords and words with low info value
  frequency <- textstat_frequency(Corpus)                 # Creating word frequency table
  return(frequency)
}
```

### Creating a Network of Usernames and Hashtags

Lastly, network functions were created using quanteda’s `fcm()` and `textplot_network()` operations, to perform a visual analysis of the collected tweets in terms of [co-occurrences of usernames and hashtags](https://quanteda.io/articles/pkgdown/examples/twitter.html#construct-feature-occurrence-matrix-of-hashtags). The code starts with the construction of a corpus and DFM, before extracting the 50 most frequently mentioned users and hashtags.  Afterwards, a feature-occurrence matrix [FCM] is created in order to visualize the networks as shown below:
```{r eval=FALSE, include=TRUE}
# Creating a network of usernames
top_users <- function(temp_tweets){
  PostsCorpus <- corpus(temp_tweets, text_field='text')   # Creating corpus of tweet text
  Corpus <- dfm(PostsCorpus)                              # Converting into DFM
  user_dfm <- dfm_select(Corpus, pattern = "@*")          # Selecting  @*mentions
  topuser <- names(topfeatures(user_dfm, 50))             # Selecting the top 50 users  
  user_fcm <- fcm(user_dfm)                               # Converting into FCM
  user_fcm <- fcm_select(user_fcm, pattern = topuser)     # Selecting the top 50 users  
  user_network <- textplot_network(user_fcm, min_freq = 0.1, edge_color = "orange", 
                                  edge_alpha = 0.8, edge_size = 5) # Visualizing Network
  return(user_network)
}     
# Creating a network of hashtags
top_hashtags <- function(temp_tweets){
  PostsCorpus <- corpus(temp_tweets, text_field='text')   # Creating corpus of tweet text
  Corpus <- dfm(PostsCorpus)                                       # Converting into DFM
  tag_dfm <- dfm_select(Corpus, pattern = ("#*"))                # Selecting  #*hashtags
  toptag <- names(topfeatures(tag_dfm, 50))                # Getting the top 50 hashtags  
  tag_fcm <- fcm(tag_dfm)           # Constructing feature-occurrence matrix of hashtags
  topgat_fcm <- fcm_select(tag_fcm, pattern = toptag)    # Selecting the top 50 hashtags  
  hashtag_network <- textplot_network(topgat_fcm, min_freq = 0.1, edge_alpha = 0.8, 
                                      edge_size = 5)               # Visualizing Network
  return(hashtag_network)
}   
```

The following code chunk tests whether the functions are set up correctly by analyzing the tweets collected above [(see Collecting Tweets of Specific Twitter Handles)](#collecting-tweets-of-specific-twitter-handles):
```{r eval=FALSE, include=TRUE}
data_wrangling(temp_tweets)
tweets_per_day(temp_tweets)
tweets_per_hour(temp_tweets)
distributions(temp_tweets)
tweet_source(temp_tweets)
tweets_cloud(temp_tweets)
word_frequency(temp_tweets)
top_users(temp_tweets)
top_hashtags(temp_tweets)
```

## Development and Testing 

After setting up all of the required functions, they can be embedded into the Shiny app. This is done by customizing the UI and Server component as described in the above tutorial [(see User Interface [UI] Component](#user-interface-ui-component) and [Server Component)](#server-component). A series of apps was developed in this process, which progressed in complexity. The first examples made a simple transition from the raw R functions coded above to embedded visualization in Shiny. Afterwards, the set-up of the app was made more sophisticated by testing and adding more advanced customization features to the UI and Server components.

### Basic Layout of the App

Specifically, the UI and Server set-up process started with a sidebar layout `sidebarLayout(…)` and separate tabs using the function `mainPanel(tabsetPanel(…))` for the visualization of the charts and tables from data analysis and visualization functions coded above [(see Data Analysis and Visualization)](#data-analysis-and-visualization). 

The sidebar includes text input boxes `textInput(…)` to enter Twitter handles or keywords and instructions by using the `helpText()` function as well as slider inputs `sliderInput(…)` to indicate the number of tweets or seconds. Lastly, action buttons were included with the function `actionButton()` in the sidebar to initiate the data scraping process. The inputId "twitter_handle", "twitter_topic" and "stream_topic"  were assigned to the text box inputs respectively, so that they feed right in to the Twitter data harvesting functions coded above [(see Data Preparation and Processing)](#data-preparation-and-processing). Additional HTML tags `HTML(…)` as well as further layout options were added with proceeding complexity to make the app more visually appealing for the users. The first tab was set-up as homepage giving background information about the app and how to use it. The four following tabs were set up as panels for the reactive visualization charts and table outputs. 

The code below shows this basic layout of the app consisting of a sidebar with text input boxes, slider inputs and action buttons to enter search queries and separate tabs for the data analysis charts and tables:
```{r eval=FALSE, include=TRUE}
####  1) UI component
ui <- fluidPage(
  # Empty App Title
  titlePanel(""),
  # menu
  sidebarLayout(
    sidebarPanel(width = 2,
      # Get timelines
      h3("Get Timelines", align = "center"),
      # text input for Twitter handles
      textInput(inputId = "twitter_handle",label = "Enter a Twitter Handle",
                width = 200, value = "BredowInstitut"),
      # slider input
      sliderInput(inputId = "tweet_num", label = "Number of Tweets",
                  min = 0, max = 3200, value = 1600, width = 200),
      # action button to scrape users
      actionButton(inputId = "click", width = 175, label = ("Collect user timelines"),
                   icon = icon("users", lib = "font-awesome")),
          # Search tweets
      h3("Search Tweets", align = "center"),
      # text input for keywords / hashtags 
      textInput(inputId = "twitter_topic",label = ("Enter a Keyword"),
                width = 200, value = "LeibnizWGL"),
      # slider input
      sliderInput(inputId = "topic_num", label = "Number of Tweets",
                  min = 0, max = 18000, value = 9000, width = 200),
      # action button to scrape hashtags
      actionButton(inputId = "click2", width = 175, 
                   label = ("Collect tweets from"), br("the past 6-9 days"), 
                   icon = icon("hashtag", lib = "font-awesome")),
      h3("Stream Tweets", align = "center"),
      # text input for keywords / hashtags for stream API (live tweets)
      textInput(inputId = "stream_topic", label = "Enter a Keyword",
                width = 200, value = "NBA"),
      # slider input
      sliderInput(inputId = "stream_time", label = "Number of Seconds",
                  min = 0, max = 40, value = 20, width = 200),
      # action button to scrape live tweets
      actionButton(inputId = "click3", width = 175, 
                   label = ("Collect live tweets"),
                   icon = icon("hashtag", lib = "font-awesome"))),
   # Separate tabs
   mainPanel(
      tabsetPanel(
      tabPanel("About",
      h2('Social Media Observatory - Twitter Mining and Analysis Suite [SMO-TMAS]', 
         align = "center"),
      p(em('Leibniz-Institut für Medienforschung | Hans-Bredow-Institut'),
        align = "center"), p(em('Social Media Observatory'), align = "center"), 
      HTML('<hr style="color: black;">'),
      p(strong('Layout Template:'), align = "center"),
      p("The basic layout of the app consists of a sidebar with text input boxes, slider 
        inputs and action buttons to enter search queries and separate tabs for 
        the visualization of the charts and tables.", align = "center")),
   tabPanel("Tweets"), tabPanel("Statistics"), tabPanel("Text Analysis"), 
   tabPanel("Network Analysis")
   ))))
### 2) Server component 
server <- function(input, output) {}

### 3) shinyApp component
shinyApp(ui = ui, server = server)
```   
  
### Tweets Tab

The Tweets tab was set up to include an interactive table of the scraped Tweets using the output function `DT::dataTableOutput(outputId = "…")` from the DT package and a download button `downloadHandler(…)` to facilitate the export of the scraped Twitter data. As shown in the code chunk below, the click of action bottom was defined as trigger:
```{r eval=FALSE, include=TRUE}
####  1) UI component
ui <- fluidPage(
  # Empty app title
  titlePanel(""),
  # Setting up side bar
  sidebarLayout(
    sidebarPanel(width = 2,
      # Get timelines
      h3("Get Timelines", align = "center"),
      # text input for Twitter handles
      textInput(inputId = "twitter_handle",label = "Enter a Twitter Handle",
                width = 200, value = "BredowInstitut"),
      # slider input
      sliderInput(inputId = "tweet_num", label = "Number of Tweets",
                  min = 0, max = 3200, value = 1600, width = 200),
      # action button to scrape users
      actionButton(inputId = "click", width = 175, label = ("Collect user timelines"),
                   icon = icon("users", lib = "font-awesome"))),
  # Setting-up Text Analysis Tab
   mainPanel(
    tabsetPanel(tabPanel(
    "Tweets",
    h2("Table of scraped Tweets", align = "center"),
    p("The table with the text and further meta data of the scraped tweets can be 
      downloaded by pressing the download button on the bottom left.", align = "center"),
    DT::dataTableOutput(outputId = "table"),
    downloadButton("download", "Download Tweets")
      ))))) # Interactive plotly table output

### 2) Server component
server <- function(input, output, ...){
  ## Get Timelines ##
  observeEvent(input$click, {              # Defining click of action button as trigger
    # Defining user handle and sample size
    handle <- isolate({as.character(input$twitter_handle)})
    sample_size <- isolate({input$tweet_num})
    # Pulling tweets
    temp_tweets <- reactive({
      pull_tweets(handle, sample_size)})
    # Creating interactive table of the collected Tweets
output$table <- interactive_table
# Creating download button 
    output$download <- download
})}
### 3) shinyApp component
shinyApp(ui = ui, server = server)
```   

### Statistics Tab

The Statistics tab was set-up to show the interactive charts visualizations of the tweet meta data as defined in the above section on Data analysis and Visualization [(see Data Analysis and Visualization)](#data-analysis-and-visualization). The `box(plotlyOutput(outputId = "…"))` function from the `plotly` package was used for this to ensure interactivity of the charts. This functions allows users to download the charts as .png, to zoom into the charts, to reset the axes and to get and compare data when hovering over the plots. The following code chunk shows the UI and Server set-up of the Statistics tab:
```{r eval=FALSE, include=TRUE}
# Shiny related library
if(!require('shinydashboard')) install.packages('shinydashboard')
library('shinydashboard')
####  1) UI component
ui <- fluidPage(
  # Empty app title
  titlePanel(""),
  # Setting up side bar
  sidebarLayout(
    sidebarPanel(width = 2,
      # Get timelines
      h3("Get Timelines", align = "center"),
      # text input for Twitter handles
      textInput(inputId = "twitter_handle",label = "Enter a Twitter Handle",
                width = 200, value = "BredowInstitut"),
      # slider input
      sliderInput(inputId = "tweet_num", label = "Number of Tweets",
                  min = 0, max = 3200, value = 1600, width = 200),
      # action button to scrape users
      actionButton(inputId = "click", width = 175, label = ("Collect user timelines"),
                   icon = icon("users", lib = "font-awesome"))),
  # Setting-up Text Analysis Tab
   mainPanel(
    tabsetPanel(tabPanel("Statistics",
        fluidPage(h2("Charts of scraped Tweets", align = "center"),
        p("The interactive charts below visualize the meta data of the scraped Tweets.",
          align = "center"),                           # Interactive plotly table output
        box(plotlyOutput(outputId = "tweets_per_day")),
        box(plotlyOutput(outputId = "tweets_per_hour")),
        box(plotlyOutput(outputId = "metric_histograms")),
        box(plotlyOutput(outputId = "tweet_source"))))))))

### 2) Server component
server <- function(input, output, ...){
  ## Get Timelines
  observeEvent(input$click, {              # Defining click of action button as trigger
    handle <- isolate({as.character(input$twitter_handle)})
    sample_size <- isolate({input$tweet_num})    # Defining user handle and sample size
    # Pulling tweets
    temp_tweets <- reactive({                                         
      pull_tweets(handle, sample_size)})  
    # Creating bar chart depicting the days of the week that the tweets were created
    output$tweets_per_day <- renderPlotly({
      tweets_per_day(temp_tweets())})
    # Creating line plot depicting the time at which the tweets were created
    output$tweets_per_hour <- renderPlotly({
      tweets_per_hour(temp_tweets())})
    # Creating histogram depicting distributions of retweet and favorited metrics
    output$metric_histograms <- renderPlotly({
      distributions(temp_tweets())})
    # Creating pie graph depicting the source from which the tweets were sent
    output$tweet_source <- renderPlotly({
      tweet_source(temp_tweets())}
      )})}
### 3) shinyApp component
shinyApp(ui = ui, server = server)
```

### Text Analysis Tab

The Text Analysis tab shows the word cloud and word frequency table, that were set up in [(Section 4.2.6](#creating-a-word-cloud) and [Section 4.2.7)](#creating-a-word-frequency-table). To allow for a visually appealing tab layout the ´fluidPage(column(1-12,…))´ function was used, were the position of the output elements can be indicated in a 12-wide grid system. This allowed to place the word cloud with respective explanations above of the word frequency table. In addition, the interactive table output function `DT::dataTableOutput(outputId = "…")` from the `DT` package was used to create an interactive word frequency table as shown in the code below:  
```{r eval=FALSE, include=TRUE}
####  1) UI component
ui <- fluidPage(
  # Empty app title
  titlePanel(""),
  # Setting up side bar
  sidebarLayout(
    sidebarPanel(width = 2,
      # Get timelines
      h3("Get Timelines", align = "center"),
      # Text input for Twitter handles
      textInput(inputId = "twitter_handle",label = "Enter a Twitter Handle",
                width = 200, value = "BredowInstitut"),
      # Slider input
      sliderInput(inputId = "tweet_num", label = "Number of Tweets",
                  min = 0, max = 3200, value = 1600, width = 200),
      # Action button to scrape users
      actionButton(inputId = "click", width = 175, label = ("Collect user timelines"),
                   icon = icon("users", lib = "font-awesome"))),
  # Setting-up Text Analysis Tab
   mainPanel(
    tabsetPanel(tabPanel(
    "Text Analysis",
    fluidPage(
      h2("Text analysis of scraped Tweets", align = "center"),
      p("The word cloud and word frequency table below visualize the most frequently used 
        terms in the text of the scraped Tweets.", align = "center"),
      h3("Word cloud of the 100 most used words", align = "center"),
      column(3, p("")),      # Indicating position column 8 out of a 12-wide grid system
      box(plotOutput(outputId = "tweets_cloud")), # Shiny's default plot output
      column(4, p("")),
      column(5, p( em("The color and size of the words indicates their frequency."), 
                   style='font-size:10pt')),
      column(11, p(""),
        h3("Word Frequency Table"), align = "center"),      
      DT::dataTableOutput(outputId = "x2"))))))) # Interactive plotly table output

### 2) Server component
server <- function(input, output, ...){
  ## Get Timelines ##
  observeEvent(input$click, {          # Defining click of action button as trigger
    # Defining user handle and sample size
    handle <- isolate({as.character(input$twitter_handle)})
    sample_size <- isolate({input$tweet_num})
    # Pulling tweets
    temp_tweets <- reactive({
      pull_tweets(handle, sample_size)})
      # Creating word cloud
    output$tweets_cloud <- renderPlot({
      tweets_cloud(temp_tweets())})                # Shiny's default plot output
    # Creating frequency table
    output$x2 <- DT::renderDataTable({
      word_frequency(temp_tweets())})         # Interactive plotly table output
  })}
### 3) shinyApp component
shinyApp(ui = ui, server = server)
```

### Network Analysis Tab

The Network Analysis tab shows the co-occurrence networks of the 50 most frequently mentioned users and hashtags that were coded above [(see Creating a Network of Usernames and Hashtags)](#creating-a-network-of-usernames-and-hashtags). Shiny's default output plots were used, as it was not possible to embed the network graphs with `plotly`. Using Shiny's default plot output operations `plotOutput(outputId = "user_network")` and ´renderPlot(…)` means that the end-users cannot customize the network graphs interactively but they can still save them by right clicking on the plots. In addition, a short definition of in the network edges was included as explanation for the users and a visually appealing tab layout was chosen as shown the subsequent code chunk:
```{r eval=FALSE, include=TRUE}
####  1) UI component
ui <- fluidPage(
  # Empty app title
  titlePanel(""),
  # Setting up side bar
  sidebarLayout(
    sidebarPanel(width = 2,
      # Get timelines
      h3("Get Timelines", align = "center"),
      # text input for Twitter handles
      textInput(inputId = "twitter_handle",label = "Enter a Twitter Handle",
                width = 200, value = "BredowInstitut"),
      # slider input
      sliderInput(inputId = "tweet_num", label = "Number of Tweets",
                  min = 0, max = 3200, value = 1600, width = 200),
      # action button to scrape users
      actionButton(inputId = "click", width = 175, label = ("Collect user timelines"),
                   icon = icon("users", lib = "font-awesome"))),
    # Setting-up Network Analysis Tab
   mainPanel(
    tabsetPanel(tabPanel("Network Analysis", 
    fluidPage(h2("Network analysis of scraped Tweets", align = "center"),
     p("The networks below visualize the co-occurrences of the 50 most frequently 
       mentioned users and hashtags in the text of the scraped Tweets.", align = "center"),
     box(h3("User Network"), align = 'center', 
         plotOutput(outputId = "user_network")), #using Shiny's default plot output
     box(h3("Hashtag Network"), align = 'center', 
          plotOutput(outputId = "hashtag_network")),
     p(em("Edges in the above semantic networks show co-occurrences of the 50 most 
           frequently mentioned users and hashtags"), align = 'center', 
        style='font-size:10pt')))))))

### 2) Server component
server <- function(input, output, ...){
  ## Get Timelines ##
  observeEvent(input$click, {     # Defining click of action button as trigger
    # Defining user handle and sample size
    handle <- isolate({as.character(input$twitter_handle)})
    sample_size <- isolate({input$tweet_num})
    # Pulling tweets
    temp_tweets <- reactive({
      pull_tweets(handle, sample_size)})
    # Creating user network
    output$user_network <- renderPlot({
      top_users(temp_tweets())
      })
    # Creating hashtag network
    output$hashtag_network <- renderPlot({
      top_hashtags(temp_tweets())
      })
    })}
### 3) shinyApp component
shinyApp(ui = ui, server = server)
```

## Deploying the App

A free account was registered at [*shinyapps.io*](www.shinyapps.io) to deploy the app via R Studio's dedicated Shiny apps server. As already mentioned in the Shiny app tutorial above [(Section 3.4.2)](#sharing-shiny-apps-as-web-pages), the free tier limits the app to 25 active hours per month. With an activated and configured account, the app can be uploaded to [*shinyapps.io*](www.shinyapps.io) directly from the R session by simply clicking on the blue publish button in the top right corner of the interface. After registering a free account, rsconnect was installed and configured in the local R session with the following command lines:
```{r eval=FALSE, include=TRUE}
install.packages('rsconnect')
library(rsconnect)
rsconnect::setAccountInfo(name="<ACCOUNT>", token="<TOKEN>", secret="<SECRET>")
#Configuring rsconnect passing in the token and secret from the Profile -> Tokens page.
```
