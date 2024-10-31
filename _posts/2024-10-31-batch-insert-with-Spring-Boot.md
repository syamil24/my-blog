---
title: Batch Insert With Spring Boot
created: '2024-10-31T06:51:36.809Z'
modified: '2024-10-31T04:17:06.115Z'
categories: [Programming, IT, coding, Java]
tags: [coding, IT]
date: '2024-10-31'
---

Spring Boot allows you to do batch insert into database. I've noticed large difference on the time taken for me to insert around 32 thousand of rows into database at once with 3 foreign table using normal insert and using **sequence**(I Will explain later in this post) in Hibernate.


### The Benchmark Comparison
Since I didn't carefully benchmark yet to compare the time taken for both process, I'll continue this post once the benchmark was successful. I'm trying to use https://github.com/openjdk/jmh and let see if I manage to sum it up quickly

### The config
In your application.yml or application.properties, include below two lines.

spring.jpa.properties.hibernate.jdbc.batch_size=250
spring.jpa.properties.hibernate.order_inserts=true

What does these two lines is doing? I'll explain more in the future once I get more time for this. Thanks...🙏🏻

### Entity With Sequence
