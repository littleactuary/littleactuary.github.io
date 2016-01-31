---
layout: post
title: "Web application framework with Shiny (part 2)"
modified:
categories: blog
excerpt: "A batch file to complete your application"
tags: [R]
image:

comments: true
share: true
date: 2016-01-30T15:39:55-04:00
---

In this post, I want to show a convenient way to run R program without opening R console or RStudio. By this way, you can turn your R program into a real desktop application. The end-users just need to double click on a dektop icon to run your R program. I will begin with a simple example without Shiny. We will deal with Shiny in the second example.

* Table of Contents
{:toc}

## R portable
Building a desktop app with R is like packaging all necessary elements, R included, into a main folder. Why do you need to put R into your app folder? Because your app will be delivered to the clients, who don't necessarly have R installed in their computer. A solution suggested by many people is to download R portable and put it in the main folder, with all other elements. 

## Example 1 (R + batch file)
The first example corresponds to a real problem I have faced at work. The aim is to prepare csv input files (for an application) from an Excel file. You can do it with a macro VBA, or a simple script R with the help of the package `readxl`:
{% highlight r %}
install.packages("readxl")
{% endhighlight %}
The think is, I don't want to open R and execute the R script each time I want to construct the csv files. I just want to put my Excel file into a folder, then hit a button to get my csv file. a simple batch file can help me do it. (please note that it only works in Windows). 

First, you need to write your R script as usual. It looks like this:
{% highlight r %}
library(readxl)
paste("__________________________________")
paste("BEGIN: input conversion from xls to csv")
# the folder `input ML` is where I store the Excel file and also where the csv file will be generated. 
inputFolder <- paste(getwd(), "input ML", sep= "/")

AvailableFiles <- dir(paste(getwd(), "input ML", sep="/"))

Excel_file <- AvailableFiles[grepl(".xls", AvailableFiles)]
if(length(Excel_file)==0){
print("  Error: there is no excel file in input ML folder. Please verify that a .xls or .xlsx or .xlsm file exists in this folder!")
}else if(length(Excel_file)>1){
print("  Error: there is more than one excel files in this folder. Please keep only 1 at time!")
}else{
print(paste("  Reading", Excel_file, sep=" " ))
}  
# read xls file
Claim_List <- read_excel(paste(inputFolder, Excel_file, sep="/"), sheet = "Claim List")
Claim_List <- Claim_List[!is.na(Claim_List$id),]
# data treatment, data cleaning or whatever you want :)
# ...
# then build the csv file
write.csv2(Claim_List, paste(inputFolder, "LossMetaData.csv", sep="/"), row.names=FALSE)
paste("END: input conversion from xls to csv")
paste("__________________________________")
{% endhighlight %}

Save your R script under the name `input_construction.R`. Now the fun part begins. We will write a batch file to tell Windows what to do with the above script. Create a text file (with notepad or whatever you want), copy the below line:

SET ROPTS=--no-save --no-environ --no-init-file --no-restore --no-Rconsole
R-Portable\App\R-Portable\bin\Rscript.exe %ROPTS% input_construction.R 1> ShinyApp.log 2>&1

and save the text file a `.bat` extention. 

## Example 2 (R + Shiny + batch file)
