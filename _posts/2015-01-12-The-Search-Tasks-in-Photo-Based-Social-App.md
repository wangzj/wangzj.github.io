---
layout: post
title: "The Search Tasks in Photo Based Social App"
date: 2015-01-12 11:05:26
categories: database
---


Like most of social applications, it's required to add search utilities to
present more content to users. Common search tasks include geography search,
tag search, popular search.

To cope with these tasks, search engines such as Elasticsearch, Solr, etc. may
have their own advantages over other products. But at present, I decide to
combine mongodb with hadoop, which i am more familier, to get the search
utilities work.

Follow the guide from mongo documents [1], it's easy to setup the environment.

####Reference

[[1][MONGO-INTEGRATION]] http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-hadoop/ <br>
[[2][SINGLE-CLUSTER]] http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html

[MONGO-INTEGRATION]: http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-hadoop/
[SINGLE-CLUSTER]: http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html