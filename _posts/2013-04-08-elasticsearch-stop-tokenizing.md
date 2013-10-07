---
layout: post
title: "ElasticSearch, stop tokenizing"
description: ""
category: 
tags: []
---
{% include JB/setup %}

ElasticSearch's standard token parser is \W+, per http://www.elasticsearch.org/guide/reference/index-modules/analysis/pattern-analyzer/. But oftentimes, like when you want to index geo-locations, hyphenated names and space-separated names should NOT be tokenized.

Such is the case with Port-au-Prince, New York and so many others.

The solution is to change the analyzer, which performs the token parsing. The exact
steps will be documented in a later post.

    
## Reference

* http://elasticsearch-users.115913.n3.nabble.com/Faceted-search-grouped-by-full-term-td3185007.html

