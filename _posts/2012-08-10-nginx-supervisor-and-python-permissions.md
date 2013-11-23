---
layout: post
title: "Nginx, supervisor and Python: Permissions"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Once nginx, supervisor and your python application are set up you think all's
hunky dory because dammit you worked hard to get those configurations just
right. So you fire up your browser, go to your URL and what do you get? 
    
    403 Permission Denied

And your logs tell you stuff like:

    2012/08/10 22:02:38 [error] 1382#0: *3 open()
    "/home/mmweb/www/mmweb/current/ns11mm/static/ns11mm_logo.png" failed (13: Permission denied), client: 127.0.0.1, server: localhost, request: "GET /static/ns11mm_logo.png HTTP/1.1", host: "localhost"

The problem is that nginx runs as root, but each nginx worker runs as the nginx
user. In the case of applications that reside under a user context, this means
that nginx cannot access the application's path. You may ask "well why does the
nginx log get written to the user's path then, genius?!". Well, Lucy, let me
'splain. The logs are written by the core nginx process. You'll know this is
true because nginx-access.log and nginx-error.log are both owned by root:root.
But the nginx worker, who runs as nginx, does not have permissions to your
app's document-root, which resides under ~username/www/blah-blah-blah.

The way around this is to add nginx into the application's user-group:

    usermod -a -G appuser nginx

i.e., add user nginx into Group appuser. When you created your application's
user context, i.e. appuser, the primary group was also set to appuser. So now
you just ensured that the nginx worker is part of the appuser group and can
read and write to folders owned by appuser.

This might bring up a security question: if you're giving the entire user's
home directory access to the nginx worker, then is that not unsafe? Good
question - yes it is. But you're ONLY storing application-related information
in the user directory, aren't you? Do NOT store stuff that the app does not
need to know about. Do NOT store private keys, or other credential files in the
application's space!
