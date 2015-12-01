---
layout: post
title: "Good habits of writing R programs"
modified:
categories: blog
excerpt: "How to well structure an R program"
tags: [R]
image:
  feature: Rprogramming.jpg
  credit: coursera.org
  creditlink: https://www.coursera.org/course/rprog
comments: true
share: true
date: 2015-11-30T15:39:55-04:00
---

When I was at school, I did some simple projects with R. Most of them were written in a dirty way (all in a same R script). Then I had a chance to work with R more seriously during two internships. I have learnt R from reading R programs developed by experienced actuaries. Of course they are not programmers, but their programs are well-structured and much more readable and understanbdle than mine. A well-structured program (especially when dealing with a complex problem) helps you easily verify and debug your code and help others (who have to audit or modify your code) understand it quickly and effortlessly. Here are some rules that I have learnt for deploying a program with R. I believe they are also true for almost other languages.

### Rule number 1: Moduling your program

If your program is long and complex, instead of using one single R script, you should divide your program into separate modules whenever possible and store them in separate R scripts. For example, if you want to develop a pricing tool that proposes 2 different pricing methods, your program structure should look like this:

{% highlight css %}
input reading
claims modeling 
if (method ==1)
    pricing method 1
else
    pricing method 2
{% endhighlight %}

Your R program will be called through a `main.R` script 

{% highlight r %}
# functions loading       
source("F-simple_functions.R")
source("F-claims_modeling.R")
source("F-pricing_method1.R")
source("F-pricing_method2.R")

# input reading
source("E-input_reading.R")

# claim modeling
source("E-claims_modeling.R")
# pricing 
if (method ==1)
    source("E-pricing_method1.R")
else
    source("E-pricing_method2.R")

{% endhighlight %}

This script is the main workflow where your program tells R what to do. to run your program, just execute the `main.R` script! Firstly, R will load 4 different scripts that only contains functions (that's why their names begin with `F-`).  While `F-simple_functions.R` stores some simple functions that will be used almost everywhere in your program, `F-claims_modeling.R"` is reserved to specific functions that will be called in `E-claims_modeling.R` and so one. Once all functions are loaded, R will call executive scripts whose names begin with `E-`, in a scenario that you have pre-determined in the main script. 

The below scheme summarize the structure of your program.

While structuring the program seems useless in this simplified example, it happened to be very helpful when I worked with more than hundred lines of code per modules


### Rule number 2: Adding comments 
Comment a complex line of code, a function or a variable is a good habit and it should be done whenever it is possible. I remember how hard it was for me to understand a bloc of code written by others (or even by myself long ago) without any comments.

I recently found out that it is better to comment a function inside of it. Let's take an example where I declare the function `beautifulAmount()` in `F-simple_functions.R` and I called it in the main script `main.R`. I used to comment this function right above of where it is declared: 

{% highlight css %}
# function to dislay claim amount in a beautiful way
beautifulAmount = function(x){format(round(x/1000),scientific=FALSE, big.mark=" ")}
{% endhighlight %}

When I look at this function in `F-simple_functions.R`, I can understand it immediately thanks to the comment. However, if I identify this function in `main.R`:
{% highlight css %}
y <- beautifulAmount(x)
{% endhighlight %}

and I forget what it was supposed to do, I'm obligated to re-open `F-simple_functions.R`. Now, I put the comment inside the function:

{% highlight css %}
beautifulAmount = function(x){
# function to dislay claim amount in a beautiful way
format(round(x/1000),scientific=FALSE, big.mark=" ")
}
{% endhighlight %}

Whenever I have a doubt with the purpose of this function, I only need to write "beautifulAmount" in my R console and hit enter. R will show me the full code of the function `beautifulAmount()`, including the comment inside. By this way, I don't need to open `F-simple_functions.R` to understand `beautifulAmount()`.


### Rule number 3: Naming your variables wisely

A good variable name is a meaningful name, not too short, not too long. A friend of mine has taught me an interesting way of naming an index variable (what we use to name `i` or `j` or `k`). Instead of:  

{% highlight css %}
beautifulAmount = function(x){
for (i in 1:n){
...
}
{% endhighlight %}
you should write:
{% highlight css %}
beautifulAmount = function(x){
for (ii in 1:n){
...
}
{% endhighlight %}

Using the second option, when you need to find and modify your loop, you just need to jump into your R script and search (`Ctrl + F`) "ii". With the first option, a search of "i" letter in your whole script seems desperate.

### Rule number 4: Using list()
If you have many variables to work with in your program, you should group them into lists. The two basic lists are `input()`and `output()`. In the above example of pricing, you can create one list for variables of each method:

{% highlight css %}
output.method1 <- list()
output.method2 <- list()
{% endhighlight %}

To declare a variable that will be used in the method 1:
{% highlight css %}
output.method1$premium <- ...
{% endhighlight %}

For listing all elements of this list
{% highlight css %}
print(output.method1) # or without print() if you write directly into your R console
{% endhighlight %}
For listing the names of all elements of this list, write:
{% highlight css %}
names(output.method1)
{% endhighlight %}

These simple habits of programming are surprisingly useful in practice, for both code writers and code readers.

