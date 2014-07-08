---
layout: post
title: "Be a virtualenv lover, mavericks"
description: ""
category: 
tags: [virtualenv, python, osx, mavericks]
---
{% include JB/setup %}

So here we are again with OSX and Virtualenv headaches:

	rockoloLP:~ cybertoast$ source /usr/local/bin/virtualenvwrapper.sh 
	/usr/bin/python: No module named virtualenvwrapper
	virtualenvwrapper.sh: There was a problem running the initialization hooks. 

	If Python could not import the module virtualenvwrapper.hook_loader,
	check that virtualenvwrapper has been installed for
	VIRTUALENVWRAPPER_PYTHON=/usr/bin/python and that PATH is
	set properly.

Can't create a virtualenv at all:

	rockoloLP:~ cybertoast$ mkvirtualenv sundar
	Traceback (most recent call last):
	  File "/usr/local/bin/virtualenv", line 5, in <module>
	    from pkg_resources import load_entry_point
	  File "/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/pkg_resources.py", line 2603, in <module>
	    working_set.require(__requires__)
	  File "/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/pkg_resources.py", line 666, in require
	    needed = self.resolve(parse_requirements(requirements))
	  File "/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/pkg_resources.py", line 565, in resolve
	    raise DistributionNotFound(req)  # XXX put more info here
	pkg_resources.DistributionNotFound: virtualenv==1.8.2

Let's start from scratch (assuming that python has been installed from home-brew):

	sudo pip uninstall virtualenv
	sudo pip uninstall virtualenvwrapper
	
Also remove all the cruft that may have been left behind from a previous install:

	sudo rm /usr/local/bin/virtualenv
	sudo rm /usr/local/bin/virtualenv-2.7
	
Then reinstall virtualenv:

	sudo pip install -U virtualenv
	sudo pip install -U virtualenvwrapper

You may also want to specify the python the system should use. Better to put this into 
your .bash_profile or some similar shell launch point:

	export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python
	
Now things should work:

	mkvirtualenv blabla
	
