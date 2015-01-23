---
layout: post
title: Using Graylog2 messages as annotations in Grafana
---

A central logging infrastructure like Graylog2 or Logstash is something that should't be missed in modern data centres. 
The same is true for aggregation and graphing of your infrastructure, application and bussines metrics. 

A central point for collecting those metrics can be done with tools like Graphite, OpenTSDB, or InfluxDB.  

But the real #monitoinglove comes in place when you combine metrics and events into a single graph. 
Imagine the following scenario: You discoverd an anomaly in your metrics and start digging into it.
Than you've found log messages that could potential be in coherence with the anomaly. 
Now you want to see if every time the anomaly appears the same message is logged as well. 
So you want to see the correlation of metric and event date in a single graph. 
It is something that I really want to have for over the last 10 year working in systems engineering. 

This is where Graphana and it's annotation feature plays well and what I like to describe in combination with Graylog2 in this post. 
Graylog2 uses Elasticsearch as search engine which is the same as in the popular Elasticsearch/Logstash/Kibana ELK-Stack. 

## Requierments

Browser needs to have access to Elasticsearch API

>> Grafik

## building a log query in Graylog

## add a annotation in Grafana 

- index name
- replace servernam with variable 

TODO: Link Howto

- Puppet Runs
- Deamon restarts
- App specific 

# debuging
ss


#monitoinglove, graylog2, grafana, devops

https://github.com/grafana/grafana/issues/201
http://metabroadcast.com/blog/graph-all-the-things
http://grafana.org/docs/features/annotations/