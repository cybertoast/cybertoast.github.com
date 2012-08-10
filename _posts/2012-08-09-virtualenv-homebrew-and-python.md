---
layout: post
title: "Virtualenv, Homebrew and Python"
description: ""
category: 
tags: ["python","virtualenv","homebrew"]
---
{% include JB/setup %}

Installing python2.7 on Snow Leopard from homebrew AFTER installing virtualenvwrapper may cause:
    ImportError: cannot find module virtualenvwrapper.hook_loader

The solution is to run the following sequence:
    
    /usr/local/share/python/easy_install2.7 pip
    # Check that pip is 2.7
    pip --version
    # Re-install virtualenvwrapper
    pip install virtualenvwrapper
