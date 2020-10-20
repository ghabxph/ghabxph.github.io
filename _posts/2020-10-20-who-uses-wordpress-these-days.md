---
layout: post
title:  "Who uses WordPress these days?"
date:   2020-10-20 02:30:00 +0800
categories: blogging
comments: true
---

## Introduction

![WordPress](/assets/images/wordpress-bg-medblue.png)

WordPress is wonderful. It’s free and open-source and it has great community support. It simply does
the job.  Setting it up is very easy.  All you need is to install  all prerequisite dependencies  on
your server,  and extract  WordPress’s source code  and run the installation setup  in your browser.
Setting it  up needs almost zero technical knowledge.  There's nothing to  code from  the operator's
perspective.

But WordPress is expensive  if you  have a  large number  of concurrent  visitors per second.  In my
experience, apache web servers  can only accomodate  a few  numbers of concurrent visitors,  and RAM
usage rises dramatically as users grow. I am not very sure if Nginx + PHP-fpm solves these issues.

## Going back to the stone age
In the beginning,  webmasters use plain HTML to  publish their written works online.  Scaling things
up, this is seriously  tedious to work on and the use of platforms  such as WordPress saves everyone
some time of thinking about the technical side.

## The cost grows as visitors grow.
There are two ways of scaling things up:  Vertical and Horizontal.  Vertical scale-up is adding more
resources to the machine.  This is the first recommended scale up for WordPress. You add more RAM to
the machine to accommodate more visitors.  Now eventually,  you'll add a load balancer to your setup
and scale things horizontally. Horizontal scale up means adding a duplicate setup that points to the
same database behind a load balancer and thus, traffic is divided among instances. This is expensive
and there are ways to optimize things while keeping the same specs.

## Going back to the stone age: The era of Static Page Hosting.
People have learned from the past.  No one renders dynamic pages anymore in the backend.  That's the
job for JavaScript.  With tools such as Jekyll and GitHub,  the cost of starting a blog cuts down to
zero.

Jekyll is a static page generator.  One can simply write a blog through markdown under _posts folder
and push your  work to GitHub,  and GitHub will  do the rest  of the initial rendering.  GitHub will
serve the rendered static pages which eliminates much of the load on their side, so it is guaranteed
that it can serve thousands of visitors simultaneously while keeping things cheap.

If you would like to add features such as a comment system, in which an important requirement to any
blog for engagement with your readers, you can rely on free embeddable plugins such as Disqus, which
is already integrated into minima theme,  a default template for Jekyll.  But if you and your target
visitors are privacy-minded individuals,  you may develop your very own comment system and create an
embeddable system that you  can easily  plug into any static pages.  Or to save you time,  there are
open source tools  that you can use immediately,  or you'd like to modify to fit your  privacy needs
such as [isso](https://posativ.org/isso/)

## Analytics, advertisements, and more dynamic stuff
I am pretty much sure  that there are solutions out there that  will allow you to see statistics, or
to embed advertisements,  or add further  dynamic  stuff within  the blog.  If you take some time on
reading the Jekyll docs, setting things up is very easy and intuitive. 

## There's just no reason to use WordPress at all (in my opinion)
In my opinion,  there's no reason to use  WordPress or any other  blogging platform at all.  And the
best thing of all,  starting  things up  is free  of charge  which saves  you some monthly  bills on
hosting.  The only cost that  you need is a domain,  which is around 7 USD per year,  or just settle
with *.github.io subdomain if you want to keep things fully free of charge.

## Markdown is easy. Designing your page is easy.
Writing a new page is very easy. Markdown is very easy and intuitive to learn. You can just focus on
writing as you used to on WordPress and keep yourself productive.

Now, if you invested your time to learn the basic HTML and CSS,  you might  consider developing your
very own template.  I  was never able to learn to develop on WordPress. For me, there's just so much
information to learn and to me,  it's not  worth studying.  The pay is  very small to my standard so
I never try to learn WordPress development.

But on using Jekyll,  I'm pretty  confident that anytime,  I would be  able  to design  my very  own
template today if I want to.  There are only a  few things to learn such as the folder structure and
its templating mechanism.  The design is very simplistic. Simply, it's just so beautifully designed.
Anyone can learn it in a day, given that you have web development experience.

## Conclusion
I love GitHub pages and Jekyll.  It does the job  very well  in keeping  things very simplistic.  My
taste of managing things is to manage things through Git. I have tried to build my very own platform
of git-ops  type of blogging and using Emacs Org (kinda similar to emacs, but Org is for Emacs users
like me).  I dropped the  idea and  decided to  quickly get  started by using Jekyll + GitHub Pages.
There's no regret. Through this setup, I learned a lot. I think I still consider developing my very
own platform. Through this setup,  I shall use  this knowledge  to build  a more  convenient  setup,
in which I will not disclose for now.

That's all! Thank you for reading.
