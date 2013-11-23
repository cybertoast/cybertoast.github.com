---
layout: post
title: "Git goosechase of negative patterns being forbidden"
description: ""
category: 
tags: []
---
{% include JB/setup %}

It's rare that git gets confused (more often it confuses me). But this is an interesting one ...

Create a dummy folder in your repo folder, but don't commit it.

Add the folder into your .gitattributes as an export-ignore. Commit, then try to clone the folder


    cd /path/to/repo
    mkdir foobar
    echo "/foobar        export-ignore" >> .gitattributes
    git add .gitattributes && git commit -m "Testing negative patterns forbidden"
    git clone path/to/repo /tmp/foobar

The error that you'll get is:

    git clone git@git.assembla.com:lp-911-memorial.3.git /tmp/mmweb/releases
    Cloning into '/tmp/mmweb/releases'...
    remote: Counting objects: 16550, done.
    remote: Compressing objects: 100% (4659/4659), done.
    remote: Total 16550 (delta 11529), reused 16401 (delta 11440)
    Receiving objects: 100% (16550/16550), 25.49 MiB | 3.89 MiB/s, done.
    Resolving deltas: 100% (11529/11529), done.
    fatal: Negative patterns are forbidden in git attributes
    Use '\!' for literal leading exclamation.

The problem is that having an entry in .gitattributes without that entry having any context
as far as git is concerned (ie that it's not committed or in .gitignore) seems to cause
an error.

*_Update 3/30/2013_*

It appears that the above assessment is completely incorrect. The problem was due to
version incompatibility between client 1.8.x and server 1.7.x, from what I can tell.

Forcibly using a 1.7.x git (/usr/bin/git which is Apple git vs /usr/local/bin/git, which is
homebrew git) resolves this issue.

