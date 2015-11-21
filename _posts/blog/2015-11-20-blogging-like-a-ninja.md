---
layout: post
title: "Blogging like a ninja with Jekyll"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2014-08-08T15:39:55-04:00
---

So, this is not about Data Science or Actuarial Science. This is in fact about Jekyll. I believe that the title of this post was inspired by some [Udemy] course. Well, I don't know why there's a ninja here, maybe because of the character of GitHub. Anyway, that sounds funny :)

Back in 2006 when I was in high school, I found out **Blogger** (with a subdomaine .blogspot.com), a blog-publishing service, written in Python and developed by Pyra Labs, which was bought by Google in 2003. It was quite interesting for a kid with no IT knowledge to blog with Blogger. I enjoyed customizing my blog by reading and modifying HTML/CSS code written by others, without knowing exactly what all of that mean. 

One day, I heard about **Wordpress**, a CMS written in PHP and MySQL. I found out that most of the famous blogger templates were converted from wordpress templates. Wordpress is so far the most famous and succesful CMS in the world. About 60% of the websites are powered by Wordpress. It has a huge number of contributors and users compared to Blogger. Contrarily to Blogger, Wordpress offers 2 options: 

* wordpress.com hosted by Wordpress (which means you don't own you site, Worpress does, just like Google owns alll Bloggers),
* worpress.org which is a CMS that you can host on your own server and freely add plugins to your Wordpress site. Here's an example of site I've created for **D.I.A.F. (International Diffusion of French Actuarial Science)** by using Wordpress.org: [**assodiaf.org**](http://assodiaf.org/). Worpress is wonderful. I didn't touch a line of code to build such a powerful site. But it also has limits. One of those is that you have to pay for server and domain. Another one is that it works with databases so it is somehow risky and slow. 


For my personal blog, I've chosen **Jekyll**, a simple, blog-aware, static site generator written in Ruby. Don't panic. You don't need to know about Ruby to blog with Jekyll (I don't know Ruby yet). However, the way Jekyll works is quite different from Wordpress. If you are a fan of Wordpress and want to change to Jekyll, it is not trivial at the first time. Jekyll is easy, but you have to use your terminal console to work with Jekyll. It took me a while to get familiar with this. Instead of using databases, Jekyll takes the content (plain text format such as Markdown), combine with template to produce a static website ready to be served. There is no more databases. You write your posts with Markdown and Jekyll will take care of other things. Another cool thing about Jekyll is that it happens to be the engine behind GitHub pages. Thus, you can use GitHub to host your Jekyll site for free. Awesome isn't it?

If you want to learn Jekyll, I recommend you to start with some intro to Jekyll video that can be found on Youtube, like [**this one**](https://www.youtube.com/watch?v=O7NBEFmA7yA). Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh]. You also need to know about [**GitHub pages**](https://pages.github.com). With Jekyll, you can build your site from scratch, or simply use a ready-to-use template (as I did). Here's some [**Jekyll themes**](http://jekyllthemes.org). Finally, you learn some [**Markdown**](https://daringfireball.net/projects/markdown/) syntax to write your blog. That's all for now.  





[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
[Udemy]: https://www.udemy.com
