---
layout: post
title: "Brew trips over xcode select"
description: ""
category: 
tags: []
---
{% include JB/setup %}

    brew install python
    ..
    sudo xcode-select --print-path
    Error: No Xcode is selected. Use xcode-select -switch <path-to-xcode>, 
        or see the xcode-select manpage (man xcode-select) for further information.

    *** harrumph ***

    xcode-select --switch /usr/bin/
    sudo xcode-select --print-path

    brew install python
    ..
    *** happiness ***

Also installing versions from other repos is easy:

    brew tap homebrew/versions
    brew install python26
    
Grokking the homebrew vernacular/jargon is obviously something that would make running 
commands easier. I'm finally understanding that `brew tap` is a way to tap into non-
standard (or external) repositories.


