---
layout: post
title: "Connecting R and C++ with Rcpp"
modified:
categories: blog
excerpt: "Speed-up your R program with Rcpp package"
tags: [R]
image:

comments: true
share: true
date: 2015-11-25T15:39:55-04:00
---

If you are an R user, you may know that R is potentially slow compared to other languages such as Matlab, Java or C/C++. It's highly recommended to avoid using loops in R. A common alternative way is to vectorize your program, i.e. to use vectors and matrices. There is a family of `apply()`functions developed in R to help you work with vectors, matrices, dataframes. These functions are better than a naive loop in R, but still struggle when the dimension of your vectors or matrices is too big. A better solution I want to talk about in this post is the Rcpp package which provides a C++ API as an extension to the R system. FYI, many R packages today rely on Rcpp. 

If you are strong enough to code your whole program in C++, well, I have nothing more to say. Nothing can beat C++ in term of speed. But if you choose to stay with R (because you are familiar with many useful R packages and pre-built functions that can save you a lot of time compared to C++), and if you are an occasionnal C++ user, you may try `Rcpp`. Keep building your tool with R and you only need to code some parts in C++ and then call them in your R program through Rcpp. I will show you how well Rcpp performs.

* Table of Contents
{:toc}

## Installing Rtools

This is for Windows users only! If you are using Mac, you just need to install Xcode and go to the next section.

We need to install Rtools in order to have a C++ compilator. Please [**download Rtools here**](https://cran.r-project.org/bin/windows/Rtools/){:target="_blank"}. Make sure that you are downloading an Rtools version compatible with your R version.

After installing Rtools, the environment variable PATH needs to be re-defined to tell R where to find the C++ compilator. 

* If your computer does not require an admin mode, it will be done automatically.
* Othersiwe, you have to do it manually. First go to Control Panel, pull down the admin password and then add the below line in your PATH:
{% highlight r %}
c:\Rtools\bin ; c:\Rtools\gcc-4.6.3\bin ; 
{% endhighlight %}

To check whether or not Rtools was sucessfully installed, enter the first line below into your R console. If everything is fine, you will see the second line:
{% highlight r %}
Sys.getenv('PATH')
[1] "c:\\\\Rtools\\\\bin;c:\\\\Rtools\\\\gcc-4.6.3\\\\bin;...
{% endhighlight %}


## Installing Rcpp package

The Rcpp package only works with R3.0.x or later. Download the latest version of Rcpp [**here**](Rcpp) and install it manually, or from within R:
{% highlight r %}
install.packages("Rcpp")
{% endhighlight %}

## Using Rcpp
C++ functions need to be writeen in `.cpp`file(s), apart from R script. If you are using RStudio, you can create one by selecting File > New File > C++ file.
Below the default example of C++ function `timesTwo()` declared in `test.cpp`:
{% highlight c %}
# include <Rcpp.h>
using namespace Rcpp;
// [[Rcpp::export]]
NumericVector timesTwo(NumericVector x) {
return x * 2;
}
{% endhighlight %}

The two headers are indispensable:
{% highlight c %}
# include <Rcpp.h>
using namespace Rcpp;
{% endhighlight %}

In `test.cpp`, we can define different C++ functions. Each of them has to begin with:
{% highlight c %}
// [[Rcpp::export]]
{% endhighlight %}
Attention: it's not like other comment so you cannot omit it! Please note that NumericMatrix and NumericVector are specific types of Rcpp to define matrices and vectors.

Now, to export `timesTwo()` to your R script:
{% highlight r %}
library(Rcpp)
SourceCpp("test.cpp")
v <- c(1,20,100)
timesTwo(v)
{% endhighlight %}

You can also include R code blocks in C++ files processed with sourceCpp. The R code will be automatically run after the compilation:
{% highlight r %}
/*** R
v <- c(1,20,100)
timesTwo(v)
*/
{% endhighlight %}

## Performance comparison
Ok, so now that we've got some basics out of Rccp, you can do some tests to see how well Rcpp performs by timming your code.  You can either use `proc.time()` or `system.time()`:
{% highlight r %}
// using system.time()
system.time(timesTwo(v))
// using proc.time()
t <- proc.time()
timesTwo(v)
t <- proc.time()-t
print(t)
{% endhighlight %}

Of course, this is the simplest example where timesTwo(v) is no better than v*2 because both take 0s. 

During my part-time internship at AXA Global P&C, I had to speed-up an Actuarial pricing tool built with R. What I found out is that the core R was already very well written. There is no loop and there are `apply()`, `sapply()`, `lapply()` everywhere. After checking the running time of each bloc of code, I suprisingly indentified that those `apply()` functions are the main reason of the time cost. 

Thus I decided to recode those functions in C++. What I obtained by the end is a running time reducing of 95%. For example, with 10000 simulations, the running time has reduced from 74.16s to 3.88s. Not bad at all, right?

<figure class="third">
<a href="{{ site.url }}/images/temps1.png"><img src="{{ site.url }}/images/temps1.png" alt="image"></a>
<a href="{{ site.url }}/images/temps2.png"><img src="{{ site.url }}/images/temps2.png" alt="image"></a>
<a href="{{ site.url }}/images/temps3.png"><img src="{{ site.url }}/images/temps3.png" alt="image"></a>
<figcaption>Comparison in term of running time: R vs Rcpp</figcaption>
</figure>

If you are interested in what I have done exactly in my part-time internship, here are my two reports (in french only): [report 1](https://drive.google.com/file/d/0B9sO-FiCPQljQ3M0b1hnTUdmaDg/view?usp=sharing){:target="_blank"}, [report 2](https://drive.google.com/file/d/0B9sO-FiCPQljY28xYnBVQndqck0/view?usp=sharing){:target="_blank"}.

## To go further
Above is a quick introduction to Rcpp. I didn't dig too much into Rcpp since my internship didn't require to. If you want to learn more about Rccp, here are some useful ressources:

* [Rcpp doc](https://cran.r-project.org/web/packages/Rcpp/Rcpp.pdf){:target="_blank"}
* [Using Rcpp with RStudio](https://support.rstudio.com/hc/en-us/articles/200486088-Using-Rcpp-with-RStudio){:target="_blank"}
* [rcpp.org](http://www.rcpp.org){:target="_blank"}
* [Rcpp core on GitHub](https://github.com/RcppCore/Rcpp){:target="_blank"}
* [Rcpp from Advanced R by Hadley Wickham](http://adv-r.had.co.nz/Rcpp.html){:target="_blank"}


