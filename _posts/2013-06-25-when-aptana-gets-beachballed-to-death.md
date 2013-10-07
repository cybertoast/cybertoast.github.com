---
layout: post
title: "When Aptana gets beachballed to death"
description: ""
category: 
tags: [aptana development ide]
---
{% include JB/setup %}

Aptana (and Ecliple) sometimes have the bad habit of getting corrupted. This 
often happens when your machine crashes (kernel panic on OSX, BSOD on windows).

The symptom is annoying - aptana/eclipse will open, but no interaction is possible
on the application. All clicks and keypresses are ignored, and either the hourglass
or the beachball are all the feedback you receive. There's no logs that I could find 
either.

The solution seems to be to remove the project metadata. It's worth removing the last
project you were working on, or each one at a time, to see what resolves the problem:

    cd /path/to/aptana/workspace
    cd .metadata/.plugins/org.eclipse.core.resources/.projects/
    
This folder lists all the projects that you have defined in your workspace. Move
any of them out of the way and restart Aptana:

    mv .metadata/.plugins/org.eclipse.core.resources/.projects/myProject /tmp/

This just helps diagnose which project the problem is in. The errant file that's 
causing the issue is a different situation. In my case it turned out to be the
`.marker` file. Deleting it, reopening and closing Aptana solved the hung-state problem.