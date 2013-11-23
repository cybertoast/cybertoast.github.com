---
layout: post
title: "Let my Apache write"
description: ""
category: 
tags: [apache, osx]
---
{% include JB/setup %}

Thanks to Apple's security and knowing that i need to be restrained in everything
that i do, i often will get 

    Forbidden

    You don't have permission to access / on this server.

when I access a local resource that I've defined in my etc/apache2/other/blah.conf.

The way around this is to add \_www into the staff group. Since Apple also deems the 
standard usermod command to be too gauche, they have dscl that I have to remember.

So the way around this is:

    sudo dscl . -append /Groups/staff GroupMembership "\_www"

Restarting apache may still yield the Forbidden message. This is probably because 
of PHP not being configured. Let's test this

* http://localhost/index.html -> works
* http://localhost/index.php  -> forbidden

What this means is that the PHP module is probably not loaded. Ensure that

    LoadModule php5_module libexec/apache2/libphp5.so

is uncommented in /etc/apache2/httpd.conf

Now restarting apache should make your life better.

