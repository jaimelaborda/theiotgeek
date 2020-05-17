---
layout: default
title: How I have set up this blog
published: 2020-04-28T19:00:11.778Z
date: 2020-04-28T19:00:11.789Z
thumbnail: /assets/images/first_post.jpg
categories: web
tags:
  - web
  - serverless
comments: false
---
Hello fellow readers and welcome to my blog! I have not found a better way to start this blog  than a post that explains how I have set up this. So, I will try to explain the arquitecture I have used, why I have elected to use this and what other alternatives do we have when it comes to having a site on the Internet.

<!--more-->

## Introduction

I have decided to follow a modern serverless arquitecture using a static site generator, but why?.. Well, why not?

Static site generators have been with us for more than \[REVISAR ESTO]. They are simple, they have no database, no updates, no mantenances... just your content!

The most important thing when you set up a blog (at least a personal blog) is your content, thats where the effor should go. I wanted to look for the easiest way of starting to publish my content to the word as quick and easy as posible. 

Nevertheless, this option is not for everybody. I consider that at least you need to have a little bit of knowing of what you are doing or at least you should have a good friend that set up all for you, because when it is already setyp, as you will see, it is really easy to mantain and to update.

\[INTRODUCIR IMAGEN DE COMO ESTÁ MONTADO EL SISTEMA]

## Jekyll

There are a ton of static site generators this days. Almost every javascript framework or lenguaje have it own generator. Yo don't believe me? Take a look at [this ](https://www.staticgen.com/)and tell me. We have options for JS React framework ([Gatsby](https://www.gatsbyjs.org/), [Next.js](https://nextjs.org/)), pure Javascript ([Docusaurus](https://docusaurus.io/)), Go lenguaje ([Hugo](https://gohugo.io/)), Ruby ([Jekyll](https://jekyllrb.com/)), Python ([Sphinx](https://www.sphinx-doc.org/en/master/)) and much more. These are just one of the most used ones. Some are focused on generating documentation for developers, others are more for blog/web but the concept is exactly the same.

And now the question is: Why Jekyll?... And the answer another time is *Why not?*

I really don't know why but the first thing it came up to my mind when I think about static site generator is Jekyll. Perhaps because it is (I think) one of the first to came along, perhaps it is because its integration with Github Pages... But for me talking about static site generator is talking about Jekyll. 

How do they work? It is actually very simple: The user writes the content, normally, as a [Markdown ](https://en.wikipedia.org/wiki/Markdown)files, the static site generator, in this case Jekyll, take this Mardown files, and renders it to produce a complete, static HTML website ready to be deployed in your favourite hosting. Notice that this is plain HTML static files (plus css and js), so there is not interpreter, no PHP, no backend, no database, no nothing! So you just need a simple Apache that everybody gives for free nowaday that serve this files to the browser. This means that you will be able to host it in a [AWS S3](https://aws.amazon.com/s3/) bucket, a conventional shared hosting, [Github Pages](https://pages.github.com/) and so on.

Jekyll is an open source static site generator written in [Ruby ](https://www.ruby-lang.org/en/)(don't worry, you don't need to know Ruby in order to understand Jekyll, I don't know nothing about Ruby, in fact making this article is the first time I searched it in Google). It has [MIT ](https://opensource.org/licenses/MIT)open source license so feel free to use for whaterever you want.

Jekyll, and all the others static generators, use what is known as layouts o templates. This is normally a HTML like file that has some directives that are substituted by your content. Nevertheless this is not intended to be a tutorial on how to use Jekyll, if you are looking for more information, the documentation of Jekyll is pretty good: <https://jekyllrb.com/docs/>

## Github Integration

## Netlify CMS

Jekyll is very cool but it is perhaps a little geeky (well, that is what the blog is about, right?). Yes, we do love geeky things but I wanted to have a way to make it even simpler. So I was looking more to a Wordpress like approach and I find Netlify CMS. 

[Netlify CMS](https://www.netlifycms.org/) is a Content Management System for static websites whose objective is precisely to provide a convenient and friendly editing interface for content. Cool!

It provides you with a menu with all the post you have, and you can edit it with the embedded editor. You will be able to view a customizable preview of your post with your own CSS (this has to be configured, but you can). 



![](/assets/images/netlify_cms.png){:.img-responsive}

But how can I include this in my Jekyll blog? Easy! You just need to add a directory with two files (literaly) and you have everything. An index.html that that builds the page and powers Netlify CMS together with a javascript CDN that takes care of the login authentication, and a config.yml that will describe and configure all the necesary things . (Off topic: I don't know if I love or hate yml files. The indentation sometimes is a little nightmare)

I could have done a step-by-step guide to how I've put it together but I think it makes no sense as there are already good guides that I have follow on (Links below on the bibliography)

Constructing the CMS with a CDN link has the advantage that you will never have to worry about updates! It will always be updated, at least in theory...



\[CONTAR QUE TIENE PREVIEWS CUSTOMIZABLES, GESTION DE LOS POSTS, DE LAS PAGES, DEFINICIÓN DE COLLECTIONS, ETC]

## Netlify Continuous Deployment

Netlify is...

Combining Netlify CMS with Netlify has a lot of advantages, it offers a Continuous Deployment integration (also known as CI/CD). This means that it automates the deployment of your website and makes it very easy to use, as you don't have to take around with pesky commands, dependencies and packages installed on your computer, you just make your commit (with Netlify CMS this is just click "Publish") and Netlify CMS does everything for you. It compiles and build the application to generate the static files and then it deploys this static files to their own host. Yes, it also sever as your host. 

Netlify CMS is a service so it has a free tier that, of course, has some limitations as a 100GB maximum bandwith I wish I have so many visitors but it will not be the case. It also offers a pro and enterprise pricing tags. But, for the bast mayority of user, and for this personal blog, with the free plan will be enough.



## Conclusions

If you are looking to start up a blog I wonder you take a look to static websites generators

And if you still want more I invite you to have a look to the ins and outs of this blog by visiting its Github repository at [github.com/jaimelaborda/theiotgeek](https://github.com/jaimelaborda/theiotgeek). Feel free to Fork the repo and to build in your own machine or with Netlify (should be ready to play). Also feel free to make me a pull request (I would greatly appreciate any help with my English or wathever thing you are not confortable with)

## Bibliography

* Netlify CMS Jekyll integration (Official documentation): <https://www.netlifycms.org/docs/jekyll/>
* 10 steps to configure Jekyll with Netlify as a CMS: <https://blog.mvp-space.com/10-steps-to-configure-jekyll-with-netlify-as-a-cms-d754d73ea731>