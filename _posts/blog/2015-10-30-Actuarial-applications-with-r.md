---
layout: post
title: "(Actuarial) tools with R"
modified:
categories: blog
excerpt: "How to write a well-structured and understandable R program"
tags: [R, Actuary, Actuariat, Actuarial]
image:

comments: true
share: true
date: 2015-10-30T15:39:55-04:00
---

At school, I'd done some simple projects with R, most of which were written in a dirty way (all in a same R script). Then I had a chance to work with R more seriously during my two last internships. I have learned R by reading R programs developed by experienced actuaries. Although they are not programmers, their programs are well-structured and much more readable and understandable than mine. A well-structured program (especially when dealing with a complex problem) helps us easily verify and debug our code and helps others (who have to audit or modify our code) understand it quickly and effortlessly. Here are some rules that I find very useful for deploying a program with R. I believe they are also true for almost other languages.

### #1: Moduling your program

If your program is long and complex, instead of using one single R script, you should divide your program into separate modules whenever it is possible and store them in separate R scripts. For example, if you want to develop a pricing tool that proposes 2 different pricing methods, your program structure may look like this:

~~~
input reading
claims modeling
if (method ==1)
    pricing method 1
else
    pricing method 2
~~~

Your R program will be called through a `main.R` script:

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
if (method == 1)
    source("E-pricing_method1.R")
else
    source("E-pricing_method2.R")

{% endhighlight %}

This script is the main workflow where your program tells R what to do. To run your program, just execute the `main.R` script! Firstly, R will load 4 different scripts that only contain functions (that's why their names begin with `F-`).  While `F-simple_functions.R` stores some simple functions that will be used almost everywhere in your program, `F-claims_modeling.R"` is reserved to specific functions that will be called in `E-claims_modeling.R` and so one. Once all functions are loaded, R will call executive scripts whose name begins with `E-`, in a scenario that you have pre-determined in the main script.

You can also replace each executive script by creating and calling a big function with parameters in order to make your R modules really independent (however, in my experience, it is sometimes complicate to do so). While structuring the program seems useless in this simplified example, it turns out to be very helpful when working with more than hundred lines of code per module.


### #2: Commenting code
Comment a complex line of code, a function or a variable is a good habit and should be done whenever it is possible. I remember how hard it was for me to understand a bloc of code written by others (or even by myself long ago) without any comments.

I recently found out that it is better to comment a function from inside. Let's take an example where I declare the function `beautifulAmount()` in `F-simple_functions.R` and I call it in the main script `main.R`. I used to comment this function this way:

{% highlight r %}
# function to dislay claim amount in a beautiful way
beautifulAmount = function(x){format(round(x/1000),scientific=FALSE, big.mark=" ")}
{% endhighlight %}

When I look at this function in `F-simple_functions.R`, I can understand it immediately thanks to the comment. However, if I identify this function in `main.R`:
{% highlight r %}
y <- beautifulAmount(x)
{% endhighlight %}

and I forget what it was supposed to do, I'm obligated to re-open `F-simple_functions.R`. Now, I put my comment inside the function:

{% highlight r %}
beautifulAmount = function(x){
# function to dislay claim amount in a beautiful way
format(round(x/1000),scientific=FALSE, big.mark=" ")
}
{% endhighlight %}

Whenever I have a doubt with the purpose of this function, I only need to write "beautifulAmount" in my R console and hit enter. R will show me the full code of the function `beautifulAmount()`, including the comment inside. By this way, I don't need to open `F-simple_functions.R` to understand `beautifulAmount()`.


### #3: Naming your variables wisely

A good variable name is a meaningful one, not too short, not too long. However, when naming an index variable, we usually choose a meaningless letter such as "i", "j" or "k". In this case, a friend of mine has taught me that "ii" is better than "i". Instead of:  

{% highlight r %}
beautifulAmount = function(x){
for (i in 1:n){
...
}
{% endhighlight %}
you should write:
{% highlight r %}
beautifulAmount = function(x){
for (ii in 1:n){
...
}
{% endhighlight %}

Using the second option, when you need to find and modify your loop, you just need to jump into your R script and search (`Ctrl + F`) "ii". With the first option, doing a search of "i" letter in your whole script seems desperate.

### #4: Using list()
If you have many variables to work with in your program, you should group them into lists. The two basic lists are `input()`and `output()` but you are not obligated to stay with these two. In the above example of pricing, you can create one list for variables of each method. By doing so, each method is somehow similar to an object.

{% highlight r %}
output.method1 <- list()
output.method2 <- list()
{% endhighlight %}

To declare variables that will be used in the method 1:
{% highlight r %}
output.method1$name <- "historical method"
output.method1$premium <- mean(frequency)*mean(severity)
{% endhighlight %}

For listing all elements of this list
{% highlight r %}
print(output.method1) # or without print()
{% endhighlight %}
For listing the names of all elements of this list, write:
{% highlight r %}
names(output.method1)
{% endhighlight %}

These simple habits of programming are surprisingly useful in practice, for both code writers and code readers.
