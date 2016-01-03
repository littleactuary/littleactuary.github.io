---
layout: post
title: "Web application framework with Shiny"
modified:
categories: blog
excerpt: "Build an awesome R application with user-friendly web interface"
tags: [R]
image:

comments: true
share: true
date: 2015-11-30T15:39:55-04:00
---

In my final internship, I had to deploy an application for large claims (major losses) reserving. Since the method required some serious statistical modeling, I didn't choose Excel/VBA. R seemed to be a good choice because of its statistical power and also because of some useful R packages that deal with extreme values theory, distribution fitting. The question was: "How can I build a frinedly-user application with R ?". The idea is that the end users don't need to open R and run a hundred line of R code. My first thought was to install RExcel in order to use Excel spredsheet as the application interface. I had found a short presentation of using RExcel with the ChainLadder package in R but this solution seems to be a little out of date. Then I tried the TclTk package in R. Not too cool in my opinion! Finally I found something amazing called [**Shiny**](http://shiny.rstudio.com){:target="_blank"}. It's an R package that allows you to deploy an interactive web application with R. 

## Get inspired

To get you inspired, [**here is a galery of Shiny applications**](http://shiny.rstudio.com/gallery/){:target="_blank"}
You will see that R users have made many amazing applications with Shiny. Since I just passed a little time of my internship to deploy the application (my main mission is to develop the actuarial model), I couldn't build a very complex one, but I think it seems cooler than an Excel spredsheet already. It looks like this:

<a href="{{ site.url }}/images/MajorLossesReserving.gif"><img src="{{ site.url }}/images/MajorLossesReserving.gif" alt="image"></a>

## What is Shiny?

Ok, now we will look a little bit closer to Shiny. Firstly, Shiny proposes functions with R syntaxes that replace HTML, CSS, Javascript. So you can somehow build a web interface by writing R code. Secondly, Shiny has a feature called Interactivity which is, in my opinion, a big difference compared to standard R program. There's an interaction between the client side (through the user interface) and the server side (your R program). Thirdly, you can run Shiny apps locally with RStudio and its browser or with R and a web browser such as Google Chrome or Internet Explorer. You can also upload your apps on the Shiny server and run them online. I didn't try the last option because of the confidentiality concern. So I will show you in this post how to deploy and run a Shiny app locally on you computer.

Let's begin by installing the Shiny package
{% highlight r %}
install.packages("Shiny")
{% endhighlight %}

## How to learn Shiny?
The best way to learn Shiny, of course, is to watch the series of [**Shiny tutorials**](http://shiny.rstudio.com/tutorial/){:target="_blank"} It will cover all the Shiny topics from beginner to advanced level. It however takes time. What I did in my internship is to look at a simple Shiny example and then began to build my app directly. When there was something I didn't know how to do, I googled it. It is maybe not a good habit of learning, but in a limit time constraint, it has allowed me to build an application that meets all my need (eventhough it's not optimal). 

So the point is, the series of Shiny tutorials is where to start learning. Other useful guides could by found on Goolge. Since I have passed some time on doing the same thing, I'll try to list all the important points I have learned about Shiny in the next sections for you, so that you don't have to google too many times when developing your Shiny app.

## Shiny structure

The logic of Shiny relies on the 2 notions close to the web logic: client side and server side. Client side is the component in interaction with the end-users. Server side is the component in which your R program recieves requests from users, runs R code and then send the response to the client side.
<center>
<a href="{{ site.url }}/images/Shiny_scheme.png"><img src="{{ site.url }}/images/Shiny_scheme.png" alt="image"></a>
</center>

Based on this principle, Shiny requires 2 R scripts: one for the User Interface, named `ui.r`, and one for the Server, named `server.r`. What you want to display on the the interface needs to be declared in the `ui.r` script, how this thing is done needs to be defined in `server.r`. Below is a typical structure of a Shiny application:
<center>
<a href="{{ site.url }}/images/Shiny_structure.png"><img src="{{ site.url }}/images/Shiny_structure.png" alt="image"></a>
</center>

R calls the Shiny app in a main script, thanks to the Shiny package. I prefer to create a folder named `Shiny folder` where I store 2 principal elements: `ui.r` and `server.r`. You may notice that there is third element named `www`. It's a sub-folder where I store graphical elements (images, icons, etc.) and file (pdf) that I want to display on my application. (Attention, these three elements need to be named exactly like that). I didn't show it in the scheme but the `server.r` can call other R scripts. Normally, an application has to read input files stored somewhere that I've named `Input folder` and write output files (in addition of displaying them on the user interface) in `Output folder`. 

The user interface can be displayed on the default browser of RStudio if you run Shiny through it, or on a web browser (if you are using Internet explorer version < 10, you could get into trouble with Shiny). This user interface works exactly like a web page. There are buttons, input text, etc. for you to make your decision and there will be output (graphs, images, tables, etc.) displayed on the main panel, or separed into different tabs.

## Some Shiny code

### Choosing Browser
You can either launch your Shiny app on the RStudio browser:
{% highlight r %}
Shiny::runApp('./Shiny folder/') 
{% endhighlight %}
(An alternative way: in RStudio, when you open `server.r` and `ui.r`, you can see the `Run App` button on the top right. You can launch your app by clicking on it) 
If you want to launch you app on your default web browser instead of the browser of RStudio:
{% highlight r %}
Shiny::runApp('./Shiny folder/', launch.browser = TRUE)
{% endhighlight %}

### Shiny theme

Shiny has some pre-built theme for you. All you need to do is to install the [**shiny theme**](http://rstudio.github.io/shinythemes/){:target="_blank"} package and choose the theme you want.

### Title panel

In `ui.r`, you can define a header for you application where you show the app name and the logo for example. This is done thanks to the `titlePanel()`

{% highlight r %}
shinyUI(fluidPage(
    theme = shinytheme("cerulean"),

    # Application title
    titlePanel(
        fluidRow(
            column(9, strong("Name of you application"),align = "center" ),
            column(3,img(src = "logo.png", height = 72, width = 150))
        )
    ),

    sidebarLayout(
        sidebarPanel(...),
        mainPanel(...)
    )
)

{% endhighlight %}

Since the Shiny interface is a web interface, it's designed with the famous Bootstrap framework. The screen is divided into 12 columns and all elements will be arranged related to these 12 columns. Here I devided the title panel into 2 parts, the first one takes 9/12 columns to show the app name, the second one takes 3/12 collumns to show the logo. Please note that logo.png needs to be stored in the `www`sub-folder.

### Tab panel

Look at the screenshot of the applciation I've created in my internship. There are different tab panels. You can create as many tab panels (and sub-tab panels) as you want.
{% highlight r %}
mainPanel(

    tabsetPanel( 
        tabPanel("tab 1", 
            tabsetPanel(
                tabPanel("sub-tab 1a", dataTableOutput("st1a")),
                tabPanel("sub-tab 1b", dataTableOutput("st1b"))
            )
        ), # end of tab 1

        tabPanel("tab 2", 
            tabsetPanel(
                tabPanel("sub-tab 2a", dataTableOutput("st2a")),
                tabPanel("sub-tab 2b", dataTableOutput("st2b"))
            )
        )
    )

{% endhighlight %}

### Event reactivity
You want to create a button in the user interface so that your app does something in response to the users demand when they click on the button? 
{% highlight r %}
# in ui.r
# define the Inputs Reading button
actionButton("read_inputs","Read Inputs"),

# in server.r:
variable1 <- eventReactive(input$read_inputs, {
    # what you want to do when the user clicks on the button
},
{% endhighlight %}

### Two way to display a table

In `server.r`, to put a table into the output() list in order to display it on the user interface later, you can either use the `renderTable()` (not pretty) or the `renderDataTable()` (much more beautiful). With the second function, you can customize the table with options.


### Ordering your program
Sometimes, you need to order different parts of code in a scenario. Unlike the normal logic of R, Shiny doesn't interprete your code in the descending order. In `server.r` each bloc of code aims to return an output element and Shiny will interprete them simultaneously. It's very often that one output element depends on another. In Shiny you need to tell the program when to do or to not do something. The simplest way is to create reactive button so that a bloc of code will only be interpreted when the user clicks on the button. However, I find it not practicle to create too many buttons. Sometimes I just want to tell my program that when to do something conditionnally on a variable. I use `reactiveValues` to do so. 

{% highlight r %}
display.order <- reactiveValues(part2 = FALSE, part3 = FALSE) 
{% endhighlight %}

I only want to do the part 2 when the part 1 is done and to do the part 3 when the part 2 is done. So I create a list of two reactive values preset at FALSE. At the end of the part 1 code, I change the value of part 2 to TRUE. At the end of the part 2 code, I change the value of part 3 to TRUE. Please note that at the beginning of the part 1, I insist that part 2 is FALSE (idem for part 3). It is because part 1 can be interpreted more than 1 time and each time it's interpreted, I want the part 2 to be updated.




