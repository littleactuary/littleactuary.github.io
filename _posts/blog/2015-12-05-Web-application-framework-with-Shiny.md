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

During my last internship, I had to deploy an application for large claims (major losses) reserving. Since the method requires some serious statistical modeling, I didn't choose Excel/VBA. R seems to be a good choice because of its statistical power and also because of some useful R packages that deal with extreme values theory. The question was: "How can I build a frinedly-user application with R ?". The idea is that the end users don't need to open R and run a hundred line of R code. My first thought was to install RExcel in order to use Excel spredsheet as the application interface. I had found a short presentation of using RExcel with the ChainLadder package in R but this solution seems to be a little out of date. Then I tried the TclTk package in R. Not too cool in my opinion! Finally I found something amazing called [Shiny](). It's an R package that allows you to deploy an interactive web application with R. 

## Get inspired

To get you inspired, here is a galery of Shiny applications: http://shiny.rstudio.com/gallery/
You will see that R users have made many amazing applications with Shiny. Since I just passed a little time of my internship to deploy the application (my main mission is to develop the actuarial model), I couldn't build a very complex one, but I think it seems cooler than an Excel spredsheet already. It looks like this:

<a href="{{ site.url }}/images/MajorLossesReserving.gif"><img src="{{ site.url }}/images/MajorLossesReserving.gif" alt="image"></a>

## What is Shiny?

Ok, now we will look a little bit closer to Shiny. I think it's more than just an R package. 
Firstly, Shiny proposes functions with R syntaxes that replace HTML, CSS, Javascript. So you can somehow build a web interface by writing R code. Secondly, Shiny has a feature called Interactivity which is, in my opinion, a big difference compared to standard R program. There's an interaction between the client side (through the user interface) and the server side (your R program). Thirdly, you can run Shiny apps locally with RStudio and its browser or with R and a web browser such as Google Chrome or Internet Explorer. You can also upload your apps on the Shiny server and run them online. I didn't try the last option because of the confidentiality concern. So I will show you in this post how to deploy and run a Shiny app locally on you computer.

## How to learn Shiny?
The best way to learn Shiny, of course, is to watch the series of Shiny tutorials http://shiny.rstudio.com/tutorial/ It will cover all the Shiny topics from beginner to advanced level. It however takes time. What I did in my internship is to look at a simple Shiny example and then began to build my app directly. When there was something I didn't know how to do, I googled it. It is maybe not a good habit of learning, but in a limit time constraint, it has allowed me to build an application that meets all my need (eventhough it's not optimal). 

So the point is, if you want to be a Shiny pro, you can stop reading this post now and switch to the Shiny tutorials series. If you are hurry and want to deploy a Shiny app in about fifteen minutes, you can give a look at the next section where I summarize all the important points I have learned about Shiny.
