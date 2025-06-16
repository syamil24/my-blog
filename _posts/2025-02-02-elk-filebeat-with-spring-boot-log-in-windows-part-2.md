---
title: Getting Started With ELK, Filebeat With Spring Boot log in Windows Part 2
created: '2025-02-02T06:51:36.809Z'
modified: '2025-02-02T04:17:06.115Z'
categories: [Programming, IT, coding, Java, devops]
tags: [coding, IT, devops]
date: '2025-02-02'
---

This post is a continuos post from the Part 1 of "Getting Started With ELK, Filebeat With Spring Boot log in Windows" post. If you already read and tried that one, now it's time to scale up and manage your multiple log files using just one of instance of ELK. Don't worry this post won't take you more than 4 minutes of reading.

### The Tags
Yes, the tags function in Logstash is one of the key for you to fetch multiple log files into the Elastic Search. 

Firstly we need configure our filebeat to read our different log files and tag them in a specific tags for every log file. Just like before, the configuration looks almost the same, just that you need to add the tags entry into it. I also have different configuration on how to read the log files since my log files format are different with each other.

```

```

Once completed, now configure your Logstash to define each tag to a specific index so that you can view all the logs in a specfic view in Kibana.

```
input {
	beats {
	port => 5044
	}
}

output{
	if "databaseLog" in [tags] {
		elasticsearch {
		hosts => ["https://localhost:9200"]
		#index => "%{[metadata][beat]}-%{[metadata][version]}-%{+YYYY.MM.dd}"
		index => "database-log-%{+YYYY.MM.DD}"
		user => "elastic"
		password => "H3zZH2GmfzEawoJArm=1"
		ssl_certificate_verification => false
		}
	}
	else if "testDemoLog" in [tags] {
		elasticsearch {
		hosts => ["https://localhost:9200"]
		#index => "%{[metadata][beat]}-%{[metadata][version]}-%{+YYYY.MM.dd}"
		index => "filebeat-demo-%{+YYYY.MM.DD}"
		user => "elastic"
		password => "H3zZH2GmfzEawoJArm=1"
		ssl_certificate_verification => false
		}
	}
}
```

As you can see in my picture below I can view multiple log applications in Kibana with that configuration.

### Takeaways
Instead of using tags, there's a lot of other methods to segregate your multiple log files in one single Logstash instance such as `fields.app_name` or `fields.log_type` in your filebeat entry. I tried to make it works using `fields` entry but seems it didn't goes well with me and using tags directly working our for me on the first try. The Elastic docs and forums are very useful for your references if you want to scale up more of your microservices application. Happy scripting then!!

