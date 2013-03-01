---
layout: post
title: "Virtualenv headaches"
description: "Investigations into 'ImportError: No module named virtualenvwrapper.hook_loader'"
category: 
tags: []
---
{% include JB/setup %}

Every so often (python upgrade, meteors, etc.) python's virtualenv decides to go bonkers and give me:


    Traceback (most recent call last):
      File "<string>", line 1, in <module>
    ImportError: No module named virtualenvwrapper.hook_loader
    virtualenvwrapper.sh: There was a problem running the initialization hooks. 

The basic steps that made everything work for me are:

    sudo pip-2.6 uninstall virtualenvwrapper
    sudo pip-2.5 uninstall virtualenvwrapper
    sudo pip-3.3 uninstall virtualenvwrapper
    sudo pip-3 uninstall virtualenvwrapper
    sudo pip3 uninstall virtualenvwrapper
    brew unlink python
    brew link --overwrite python
    sudo pip --version
    sudo pip install virtualenvwrapper
    workon <virtualenv>

This obviously needs more writing up for why the problem surfaces, but the above is one possible set of steps that solves the issue. The other is this [stackoverflow post](http://stackoverflow.com/questions/11507186/python-virtualenv-no-module-named-virtualenvwrapper-hook-loader)
