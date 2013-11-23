---
layout: post
title: "Drupal performance debugging"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Some thoughts on debugging drupal performance issues:

* Make sure the `devel` module is not on your production host. The right 
  solution is to remove it from your features:
    ```
    ...
    features_exclude[dependencies][devel] = devel
    features_exclude[dependencies][devel_themer] = devel_themer
    ```
* Make sure permissions are correct. In one case the Storage-API folder did 
  not have read-write permissions for the web user:
    ```
    chown -R www-data:www-data /path/to/files/storage/
    ```
* Log to the filesystem rather than the database. This is done with the `logging_alerts`
  module. It's probably a good idea to have all logs emailed to you as well.
* Ensure that Nginx, APC and FPM are tuned correctly. There are a number of guides out
  there for this, but following are some rough guidelines.
  
_Nginx_

* Do not have more worker_processes than processors
* Set worker_connections in the events{} block to 1024
* Set a fast_cgi_connect_timeout, _read_timeout and _send_timeout to be in line
  with values in FPM configuration
  
_PHP-FPM_

TBD?
