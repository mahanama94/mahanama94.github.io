---
layout: post
title: Create your own blog in minutes!
tags: [Blogs, Jekyll]
---

There are multiple ways to get started with building your own blog. Among the many, I choose github pages since I felt it would be easier than
configuring the CMS in wordpress or somewhere else.

Github offers hosting for each account in which is accessible thorugh `username.github.io`, and In the blog we will be

  1. Creating Jekyll ( See in a later blog post ) powered blog
  2. Host it in github
  3. Making the necessary tweaks.


<img src="{{ site.baseurl }}/images/create-blog/power-of-markdown.jpg" alt="Vader power of markdown" title="Vader power of markdown">

### 1. Creating Jekyll powered blog

There are many ways to get started with creating the Jekyll powered blog.
  1. Through the Jekyll command line tool
  2. By forking a repo.

Creating the blog with Jekyll command line tool is the infact the harder way. You will have to install and configure Ruby, RubyGems and many more stuff.
( If I was to build with the Command line tool, I should have changed the title to in some more minutes :D ). So
we'll be forking a repo and our blog will be based on the forked repo.

To find a repo to fork, you can always Google for it and you will probably find some amazing themed repos to build your blog.
In the case of our blog, we will be using a minimal theme that I have used for my blog.

[Jekyll Now](https://www.google.com "Jekyll Now") is a repo I have found after some surfing and Googling with minimal design, which I prefer most. So all you have to do is head to [Jekyll Now](https://www.google.com "Jekyll Now") and fork the repository to your account. Once you have forked, It will appear under your repositories tab in Github.

### 2. Host the forked repo

As a github user, you are entitled to host a user website with which is accessible through `username.github.io`. To use that all you have to do is to change the name of the forked repository to `username.github.io` in the "Settings" tab. Once you have completed, you will have your blog runing on `username.github.io`.

You will have to delete the CNAME file unless you are working with some domain of your own.

Sometimes, the update process time can take some time upto few minutes. If the blog doesn't show up, wait for sometime and refresh the page.
Once the blog has been successfully hosted, the home page you will have similar home page ( I have already done the tweaks :D ).

![Home]({{ site.baseurl }}/images/create-blog/home.png "Home page")


### 3. Making the tweaks

Once the blog is up and running, you can update the content of the Blog by changing the content of the **_config.yml** file in the root of the blog.

![Config File]({{ site.baseurl }}/images/create-blog/config.png "Config file")

**_config.yml** file contains all the configuration of the blog. Your blog title, facebook links, email links, github username and many more. Tweak as per your requirements in the file. For editing you can either use the web editor provided by Github or you can either clone the repository to your PC and make changes using a text editor.

Cheers !!!

**mahanama94**
