---
layout: post
title: "Elastic Rivers to Mongo"
description: "ElasticSearch river to a MongoDB"
category: 
tags: [mongo, esdb, elasticsearch, river, full-text-search]
---
{% include JB/setup %}

In order to get better (faster) and more of a full-text-capable search,
MongoDB is enhanced with the features of ElasticSearch. This is done by
connecting MongoDB to ESDB via a "river", which is basically a way to 
stream content from Mongo's (or any database's) transaction/replication
logs to ElasticSearch. Following are the steps to set this up for Mongo.

### References

* http://docs.mongodb.org/manual/tutorial/convert-standalone-to-replica-set/
* https://coderwall.com/p/sy1qcw
* https://github.com/richardwilly98/elasticsearch-river-mongodb/wiki


### Configuration and Setup

Set up mongodb to have replicasets. In mongod.conf, add:

    replSet rs0

Restart the mongodb service and initiate the replicaset process:

    mongo> rs.initiate()

You'll now need to log out of your shell and come back in, so that
you're connected to the master replicaset. Otherwise you'll get 
errors like:

    > db.articles.count()
    Sun Oct  6 18:59:21.455 JavaScript execution failed: count failed: { "note" : "from execCommand", "ok" : 0, "errmsg" : "not master" } at src/mongo/shell/query.js:L180

So your prompt should look like:
    
    rs0:PRIMARY>

### Installing ESDB

    brew/yum/apt-get install elasticsearch

Install the Mapper Attachment. The specific version is found at
https://github.com/elasticsearch/elasticsearch-mapper-attachments:

    $ES_HOME/bin/plugin -install elasticsearch/elasticsearch-mapper-attachments/1.9.0

Then install the ES 'river' for Mongo. Specific current version information is
at https://github.com/richardwilly98/elasticsearch-river-mongodb.
It's best to install the river manually, since the automatic plugin installer is
not being kept up to date:
    
    git clone https://github.com/richardwilly98/elasticsearch-river-mongodb.git
    cd elasticsearch-river-mongodb
    mvn package

The created target package is:

    target/elasticsearch-river-mongodb-1.7.1-SNAPSHOT.jar

Copy this into the plugins folder:

    mkdir -p /usr/local/var/lib/elasticsearch/plugins/river-mongodb/
    cp /path/to/elasticsearch-river-mongodb/target/elasticsearch-river-mongodb-1.7.1-SNAPSHOT.jar /usr/local/var/lib/elasticsearch/plugins/river-mongodb/

Ensure that you also have the mongo-java-driver, which can be downloaded from
the maven repository: http://mvnrepository.com/artifact/org.mongodb/mongo-java-driver.
As of this writing the current version is:

    (cd /usr/local/var/lib/elasticsearch/plugins/river-mongodb/ && \
     wget http://repo1.maven.org/maven2/org/mongodb/mongo-java-driver/2.11.3/mongo-java-driver-2.11.3.jar)
    
Restart the ES service:
    
    # ubuntu/linux
    sudo service elasticsearch restart
    # osx:
    launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.elasticsearch.plist
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.elasticsearch.plist

Ensure that the elasticsearch-river-mongodb is in the plugins folder, 
which is defined in your elasticsearch's config.yml file:

    ls -l /usr/local/var/lib/elasticsearch/plugins

Configuring the river is best done with a script. Get the configuration syntax
from https://github.com/richardwilly98/elasticsearch-river-mongodb/wiki.
Pay particular attention to the credentials section:

    #!/bin/sh
    curl -XPUT "localhost:9200/_river/tsarticles/_meta" -d '
    {
      "type": "mongodb",
      "mongodb": {
        "servers": [
          { "host": "127.0.0.1", "port": 10092 }
        ],
        "options": { 
            "secondary_read_preference": true 
        },
        "db": "timescapes",
        "collection": "articles"
      },
      "index": {
        "name": "articlesidx",
        "type": "article"
      }
    }'

### Troubleshooting

    Mongo gave an exception com.mongodb.MongoException: not authorized for query on local.system.namespaces

This is due to your mongo having been started with auth=True, but 
no `credentials` section in your river configuration.

For troubleshooting, always look at the logging files. Refer to the mongodb-river
wiki for information on setting this up. For example:

    tail -f /usr/local/var/log/elasticsearch/elasticsearch_sundar.log

Once your config is run, you should test that the river is importing data:

    curl -XGET 'localhost:9200/testmongo/articles/_search?q=something'




