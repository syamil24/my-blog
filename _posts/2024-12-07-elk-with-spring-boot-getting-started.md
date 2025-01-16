---
title: Getting Started ELK With Spring Boot in Windows
created: '2024-12-07T06:51:36.809Z'
modified: '2024-12-07T04:17:06.115Z'
categories: [Programming, IT, coding, Java]
tags: [coding, IT]
date: '2024-09-06'
---

I wrote this article because I found that it;s quite hard to find a starter guide for someone to use ELK and integrate it with their Spring Boot log file. It's not necessarily Spring Boot logfile, but yeah to find article related to this topic
with a much simpler and quick guide one is very difficult based on my experience. In this guide I'm using ELK stack including Filebeat in my Windows 11 machine to fetch my Spring Boot log file and display it on the web. So let's
just deep dive into it.

### Prerequisites
1. Download ELK stack
First thing first you need to have all three ELK stack (Elastic Search, Logstash, Kibana) installed in your machine. You can download all of it below and extract the compressed file.
 https://www.elastic.co/downloads/elasticsearch
 https://www.elastic.co/downloads/logstash
 https://www.elastic.co/downloads/kibana

2. Download Filebeat
https://www.elastic.co/downloads/beats/filebeat

3. Existing Spring Boot Project with logfile configured
If you don't have any Spring Boot project or any logfile configured, you can just donwload any sample logfile(not necessarily Spring Boot since we only need a structutured logfile) online. As for example, here is my logback.xml file that will be used to integrate with ELK.

### Configuring ElasticSearch
To run ELastic Search on Windows OS, we need to run in cmd `elasticsearch.bat` inside **bin** folder. It depends on your OS performance, it might take minutes in order for everything to be perfectly run. If you see in your logs, during first time running it will show your password to be logged in inside Kibana. If you didn't lucky enough or can't see the details in the log, you need to run command `elasticsearch-reset-password -u elastic` . This will reset elastic user password with a new one. This password will be used for you to log in Kibana later.

### Configuring Logstash
Ok now for logstash part, few configurations need to be taken care of before you run it.
1. Go to oor logstash folder and find config folder. And  find .conf folder. In my case it will be logstash-sample.conf file.
2. Inside this file we will use to create index and connect with our elastic search. You can refer the config below:

```
# Sample Logstash configuration for creating a simple
# Beats -> Logstash -> Elasticsearch pipeline.

input {
	beats {
	port => 5044
	}
}

output{
	elasticsearch {
	hosts => ["https://localhost:9200"]
	#index => "%{[metadata][beat]}-%{[metadata][version]}-%{+YYYY.MM.dd}"
	user => "elastic"
	password => "=MMadZCXVGJ+a1Cd32Ub"
	ssl_certificate_verification => false
	}
}
```

To run logstash in cmd, go to bin folder and execute command `logstash.bat -f config/logstash-sample.conf`. Just wait for a few seconds and logstash will be successfully run.

### Configuring Kibana
Configuring Kibana for starter is just a direct process.
1. Just for verification, just go to your kibana folder and go to **config** folder and kibana.yml
2. At the end of the file, there will be one entry for **elasticsearch.hosts**
3. Make sure it is pointing to your correct elastic search URL. By default if you run all ELK stack locally just use the default one which is ['https://localhost:9200'].
4. Just run kibana by simply go **bin** folder and `kibana.bat`
5. In my case Kibana will takes around 5 minutes to be fully up and running which is quite long time I guess. Your experience might differ.

### Configuriing Spring Boot Log and Filebeat
Ok now all three ELK services already up and running. It's time for the best and challenging part. Using Filebeat to integrate with Logstash and integrate it with your Spring Boot log file. Follow below steps on the configuration needed to set up the integration.
1. Go to your filebeat folder
2.

### Viewing Logs in your Kibana


