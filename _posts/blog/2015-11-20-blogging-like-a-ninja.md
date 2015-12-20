---
layout: post
title: "Blogging like a ninja with Jekyll"
modified:
categories: blog
excerpt: "An introduction to Jekyll"
tags: [Jekyll, GitHub, Markdown]
image:
  feature: jekyll+github.png
  credit: bitbyteym.com
  creditlink: http://bitbyteyum.com/articles/2013/05/28/serving-from-github/
comments: true
share: true
date: 2015-11-20T15:39:55-04:00
---

This post is about Jekyll, the site generator behind this blog. I have tried some blog services such as Blogger and Wordpress before ending up with Jekyll. So here's a quick review of these tools based on my experiments!

Back in 2006 when I was in high school, I found out [**Blogger**](https://www.blogger.com){:target="_blank"} (with subdomain .blogspot.com), a blog-publishing service, written in Python and developed by [**Pyra Labs**](https://en.wikipedia.org/wiki/Pyra_Labs){:target="_blank"}, which was bought by Google in 2003. It was quite interesting back then for a kid with no IT knowledge to play with Blogger. I enjoyed customizing my blog by copying and modifying HTML/CSS code written by other people, without knowing exactly what all of that mean. 

One day, I heard about [**Wordpress**](https://en.wikipedia.org/wiki/WordPress){:target="_blank"}, a CMS based on PHP and MySQL, and found out that most of the famous Blogger templates were converted from Wordpress templates. Wordpress is so far the most famous and succesful CMS in the world. It was used by more than 23.3% of the top 10 million websites as of January 2015. It has a huge number of active contributors and users compared to Blogger. Contrarily to Blogger, Wordpress offers 2 options: 

* [**Wordpress.com**](https://wordpress.com){:target="_blank"}: hosted by Wordpress (which means you don't own your site, Worpress does, just like Google owns all Bloggers),
* [**Worpress.org**](https://wordpress.org){:target="_blank"}: a CMS that you can host on your own server and freely add plugins to your Wordpress site. 

Here's an example of website I've created for the **D.I.A.F. (International Diffusion of French Actuarial Science)** by using Wordpress.org: [**assodiaf.org**](http://assodiaf.org/){:target="_blank"}. Wordpress (I mean Wordpress.org) is wonderful. I didn't have to touch a line of code to build such a powerful site. But it also has limits. One of those is that you have to pay for server and domain. Another one is that it works with databases so it is slow and somehow risky (it's a growing target for hackers). 

Thus, I was searching for a new solution, more adapted to my blogging purpose (something free, faster than Wordpress and cool). After visiting some blogs, I realized that many bloggers have moved from Wordpress to **Jekyll**, a simple, blog-aware, static site generator written in Ruby. Don't panic! You don't need to know about Ruby to blog with Jekyll (I don't know Ruby). However, the way Jekyll works is quite different from Wordpress. If you are a fan of Wordpress and want to change to Jekyll, it is not trivial in the beginning. Jekyll is easy, but you have to install and work with Jekyll via your terminal console. It took me a while to get familiar with this. Instead of using databases, Jekyll takes the content (plain text format such as Markdown), combine it with the template to produce a static website ready to be served. There is no more databases. Just write your posts with Markdown and Jekyll will take care of other things. Another cool feature of Jekyll is that it happens to be the engine behind GitHub pages. Thus, you can use GitHub to host your Jekyll site for free, as I do with this blog. Awesome right?

If you want to learn Jekyll, I recommend you to start with some intro to Jekyll videos that can be found on Youtube, like [this one](https://www.youtube.com/watch?v=O7NBEFmA7yA){:target="_blank"}. Check out the [**Jekyll docs**][jekyll]{:target="_blank"} for more info on how to get the most out of Jekyll. File all bugs/feature requests at [**Jekyll's GitHub repo**][jekyll-gh]{:target="_blank"}. With Jekyll, you can build your site from scratch, or simply use a ready-to-use theme (as I do). Here's some [**Jekyll themes**](http://jekyllthemes.org){:target="_blank"}. Finally, you should learn some [**Markdown**](https://daringfireball.net/projects/markdown/){:target="_blank"} syntax before starting your Jekyll blog. That's all for now! 





[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
