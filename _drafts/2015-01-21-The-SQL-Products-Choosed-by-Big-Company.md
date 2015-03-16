---
layout: post
title: "The SQL Products Choosed by Big Company (TO BE REVISED)"
date: 2015-01-21 11:10:05
categories: Database
---

This post logs the search results about which sql products the companies, such
as Instagram, Facebook, etc. choose for all kinds of purposes.

### Facebook

MySQL: wall posts, user information, timeline etc. This data is replicated between their various data centers.
MEMCACHED:
HAYSTACK: the biggest photo sharing website. For each uploaded photo, Facebook generates and stores four images of different sizes, which translates to a total of 60 billion images and 1.5PB of storage. The current growth rate is 220 million new photos per week, which translates to 25TB of additional storage consumed weekly.
CASSANDRA: scalability and  high availability. Facebook uses it for its Inbox search.

    For Facebook's inbox search, the inverted index of <keyword, messages*>   is updated in real time in cassandra (whenever you send a message to one of your friends) so that inbox search index is a real-time index and also super fast.

SCRIBE: a flexible logging system
VARNISH: Varnish is an HTTP accelerator which can act as a load balancer and also cache content which can then be served lightning-fast. Facebook uses Varnish to serve photos and profile pictures,handling billions of requests every day.
HIPHOP: 

### TWITTER

Google Maps: AFAIK this is all backed by a proprietary set of databases and map reduce engines.
Foursquare: uses MongoDB and Hadoop (based on public blog posts)
Facebook: well known users of MySQL (they have their own fork), they use Memcache heavily and also use Hadoop for Map/Reduce. They also created Cassandra, though it doesn't seem to be heavily used.
Twitter: is answered on Quora, they use a long list of tech: MySQL,------> FlockDB <-------, Memcache, Cassandra, Gizzard, Lucene, HBase/Hadoop, Redis.
Which database system(s) does Twitter use?
Quora: MySQL, detailed here
Why does Quora use MySQL as the data store instead of NoSQLs such as Cassandra, MongoDB, or CouchDB?
OpenStreetMaps: appear to be using PostGres
openstreetmap.org
Database - OpenStreetMap Wiki

Flickr: MySQL
Instagram: PostgreSQL


Foursquare: Elasticsearch - for real-time search and analytics, geo search
