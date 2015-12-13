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

If you are an R user, you may know that R is potentially slow compared to other languages such as Matlab, Java or C/C++. It's highly recommended to avoid using loops in R. A common alternative way is to vectorize your program, i.e. to use vectors and matrices. There is a family of `apply()`functions developed in R to help you work with vectors, matrices, dataframes. These functions are better than a naive loop in R, but still struggle when the dimension of your vectors or matrices is too big. A better solution I want to talk about in this post is the Rcpp package that helps you connect R and C++ seemlessly. 

I really like `Rcpp` because it allows me to call C++ functions in R and vice versa. If you are strong enough to code your whole program in C++, well, I have nothing more to say. Nothing can beat C++ in term of speed. But if you choose to stay with R (because you are familiar with many useful R packages that can save you a lot of time compared to C++), and if you are an occasionnal C++ user, you may try out `Rcpp`. Keep your R core and you only need to code some parts in C++ and call them in your R program through Rcpp. I will show you how well Rcpp performs.

* Table of Contents
{:toc}

## Installing Rtools
We need to install Rtools in order to have a C++ compilator. Please [**download Rtools here**](https://cran.r-project.org/bin/windows/Rtools/){:target="_blank"}. Make sure that you are downloading an Rtools version compatible with your R version.

After installing Rtools, the system variable PATH needs to be re-defined to tell R where to find the C++ compilator. 

* If your computer does not require an admin mode, it will be done automatically.
* Othersiwe, go to Control Panel, pull down the admin password and then add the below line in your PATH:
{% highlight r %}
c:\Rtools\bin ; c:\Rtools\gcc-4.6.3\bin ; 
{% endhighlight %}

To check whether or not Rtools was sucessfully installed, enter the first line below into your R console. If everything is fine, you will see the second line:
{% highlight r %}
Sys.getenv('PATH')
[1] "c:\\\\Rtools\\\\bin;c:\\\\Rtools\\\\gcc-4.6.3\\\\bin;...
{% endhighlight %}


## Installing Rcpp package





During my part-time internship, I had to speed-up an R program. What I found out is that the core R was already very well written. There is no loop and there are `apply()`, `sapply()`, `lapply()` everywhere. After checking the running time of each bloc of code, I identified where    
