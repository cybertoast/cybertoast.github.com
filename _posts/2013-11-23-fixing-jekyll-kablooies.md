---
layout: post
title: "Fixing jekyll kablooies"
description: "Or just test the damn think locally"
category: 
tags: ["jekyll", "markdown", "errors"]
---
{% include JB/setup %}

Posts that I had created were not being rendered for several months. This was due to an error in my markdown. Github had nicely sent me a notification, but I think their notification system actually started after a certain date, and did not catch earlier rendering errors.

Basically, ensure that all markdown key-symbols are escaped. This means underscores, asterisks, etc.

The easiest way to ensure that everything is hunky (let alone dory) is to run jekyll server locally to ensure that rendering is all happy happy.

Because this is all ruby, I'm going to provide a bit more background on how to separate the virtual-environment out. This is obviously not jekyll specific, but it's certainly convenient.

[Sandbox](https://github.com/nkryptic/sandbox) does in ruby what [virtualenv](http://www.virtualenv.org/en/latest/) does in python:

    rvm use ruby-2.0.0-p247
    gem sources --add http://gems.github.com
    gem install nkryptic-sandbox -s http://gems.github.com

I like my environments separated

    sandbox jekyll -g jekyll
    jekyll serve --trace
    
Errors are then pretty damn apparent through the stracktrace.

### References

* [Nkryptic-Sandbox](https://github.com/nkryptic/sandbox/tree/master)
* [Jekyll Quickstart](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)
* [Github Page Build Failure](http://hokietux.net/blog/blog/2013/04/28/github-pages-page-build-failure/)
