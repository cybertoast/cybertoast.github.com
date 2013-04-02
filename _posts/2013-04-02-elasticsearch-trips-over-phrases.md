---
layout: post
title: "ElasticSearch trips over phrases"
description: ""
category: 
tags: []
---
{% include JB/setup %}

We're using elasticsearch for our location identification and find-as-you-type functionality for identifying a location. I just realized that the standard query_string search in ElasticSearch has either changed or was not tested correctly by us (both Rodrigo and I feel like we tested it so I'm not sure where the issue is).

The problem is that searching for "new y" returns a search for all matches on "new" AND "y". The prioritization could return unintuitive or unexpected results. The normal search pattern for this would be to use a match_phrase or match_phrase_prefix, which works great in certain scenarios (and is actually much faster than the query_string search).

The source of the problem when using `filtered` queries. For example, my query is:

    query = {
      'from': skip, 'size': limit,
      'sort': {'population': 'desc'},
      'query':{
        'filtered': {
          'query': {'query_string': {'query': city + '*'}
          },
          'filter': {
            'or': [
                   {'query': {'query_string': {'query': 'country_name:' + city + '* AND feature_code:PPLC'}}},
                   {'query': {'query_string': {'query': 'admin1_name:' + city + '* AND feature_code:PPLA'}}},
                   {'query': {'query_string': {'query': 'asciiname:' + city + '*'}}}
            ]
          }
        }
      }
    }

The use of the OR filter with the field-specific AND clause (because I want "new y" to match the city of "New York" and the state of "New York") is disallowed for match_phrase_prefix; further the match_phrase queries do not allow field-definition in the query since it does not pass through the query parser.

The solution that I came up with was somewhat sketchy:

* Insert indexes with space replaced by underscore in each of the fields (eg. New York => New_York)
* Replace spaces in the incoming data with underscore: "new y" => "new_y"
* Perform the match, which succeeds with the above query
* Replace all underscore with space in the response data: "New_York" => "New York"

The process is pretty fast, but it's a hack. I'm curious how this problem is solved in the real world since it's inconceivable for me that this problem has not already been solved.

