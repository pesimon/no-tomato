---
layout: post
title: Using Graylog2 messages as annotations in Grafana
---

A central logging infrastructure like Graylog2 or Logstash is something that should't be missed in modern data centres. 
The same is true for aggregation and graphing of your infrastructure, application and bussines metrics. 

A central point for collecting those metrics can be done with tools like Graphite, OpenTSDB, or InfluxDB.  


But the real [#monitoinglove](https://twitter.com/hashtag/monitoringlove) comes in place when you combine metrics and events into a single graph. 
Imagine the following scenario: You discovers an anomaly in your metrics and start digging into it.
Than you've found log messages that could potential be in coherence with the anomaly. 
Now you want to see if every time the anomaly appears the same message is logged as well. 
So you want to see the correlation of metric and event date in a single graph. 
It is something that I really want to have for over the last 10 year working in systems engineering. 

This is where Grafana and it's [annotation feature](http://grafana.org/docs/features/annotations/) plays well and what I like to describe in combination with Graylog2 in this post. 
Graylog2 uses Elasticsearch as search engine which is the same as in the popular Elasticsearch/Logstash/Kibana ELK-Stack. 

## Requirements

* Browser needs to have access to Elasticsearch API
* This Elasticsearch API is configured as data source in Grafana's ```config.js```

```
elasticsearch: {
    type: 'elasticsearch',
    url: "http://elasticsearch:9200",
    index: 'graylog2',
    grafanaDB: false,
}
```

## Building a log query in Graylog2

* For this post I'm using a query for the start of all sudo root session:
![Graylog2 query dialog](/public/img/graylog_query.png)
  The hostname of the source ssytem will be replace with a variable by grafana. 

* The details of this filtered message shows the information you need for adding a Grafana annotation. 
![Details of a Graylog2 message](/public/img/graylog_message.png)

## Add an annotation in Grafana 

![Add annotation with specific settings for Graylog2 query](/public/img/grafana_annotations.png)
* Use the information from the Graylog2 message 
* Use a wild card for the index name, as elasticsearch populates the indices with a prefix
* Replace the server name with a variable, if you're using [templated dashboards](http://grafana.org/docs/features/templated_dashboards/). 

Now your done and hopefully see annotation marker in your graph, like this:

![Memory usage graph with an annotation marker showing a sudo root session was started](/public/img/annotated_memory_graph.png)

* Changes done by your Configuration Management system, like Puppet, Cheft, Ansible etc. 
* Code Commits, automatic software deployments, daemon restarts
* Application specific events


## Troubleshooting 

The best way to troubleshoot this to watch Chome Inspect or Firebug, while turning on and off the displaying of this annotation. 
There you find the HTTP POST request to ```http://elasitcsearch:9200/graylog2_*/_search```

Please note that the annotations with Elasticsearch seam only to work (at least for me) if you're use a time range like "2 days ago". 
If I drill into a graph or define an explicit time range, I've got a "Annotation error" resulting from a Elasticsearch API: ```ElasticsearchParseException[failed to parse date field [2015-01-29T01:42:29.330Z], tried both date format [yyyy-MM-dd HH:mm:ss.SSS], and timestamp number]```
 

#monitoinglove, #graylog2, #grafana, #devops, #elasticsearch

https://github.com/grafana/grafana/issues/201
http://metabroadcast.com/blog/graph-all-the-things
http://grafana.org/docs/features/annotations/