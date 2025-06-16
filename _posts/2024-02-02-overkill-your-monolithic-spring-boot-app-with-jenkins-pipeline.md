---
title: Overkill Your Monolithic Spring Boot App With Jenkins Pipeline
created: '2024-12-07T06:51:36.809Z'
modified: '2025-02-01T04:17:06.115Z'
categories: [Programming, IT, coding, Java, devops]
tags: [coding, IT, devops]
date: '2025-02-02'
---

You can't declare yourself as a software developer if you didn't overkil your monolithic application with less than 10 active users per hours with sophisticated tools to make your simple application become ultimately complex. Yes so if you not yet overkill your application with Jenkins to automate your simple deployment, this article might be useful for you.

In this writing, I will share with you on to achieve below pipeline stages in Jenkins:
- making options on which environment to deploy
- clone your repository
- Build your project (in my case, it's Java Spring Boot project using Maven)
- Migrate your artifacts to server (in my case Windows server)
- Deploy/run your artifacts in the server via ssh


### Pre-requisites
1. Make sure you have Jenkins installed. If you're using Ubuntu, you may refer tutorial online here:
2. OpenSSH installed in your Ubuntu OS and Windows Server. In my case I'm using public key authentication rather username and password. There's a lot of tutorials on it online. You may refer one of it here.
3. Your Build Tools or your project framework installed. In my case since I'm using maven and Spring Boot, so I already installed Maven and Java in my Ubuntu OS.


### The Scripts
If you are well setup, now let's proceed with the scripts.

1. Choosing which environment to Deploy
For example you can choose whether you want to execute the pipeline on the UAT environment or PROD environment.
In your Jenkins web, go to
2. Cloning your repository
3. Build your project
And now I' building my Spring Boot project where the fat jar artifact by defaults will be locatied in woorkspace_dir/target.
4. Migrate artifacts to server
Here I'm migrating the artifact to the server via scp. For my case I need to stop the existing services that is running and then only copy the jar file to the server.
5. Execute your deployment commands in your server
Now time to run your deployment script. For me it simple as running the jar file, make sure the log is coming and there is no error thown in the logs. By default if all good the script will automatically stop the pipeline with success status after 1 minute. as shown below.

The full script of this can be found in my gists here,

### Are you done overkilling your application
This script is only useful if you just starting up building your pipeline and gonna make things work right away. I know most of you are using Linux Server to host your application, then you may tweak this script to cope with your environment. There's a lot more things to consider such as using Jenkin Agents for better security and there's a lot other useful features and plugins you want to explore for you to overkill your monolithic application. Till then, Happy Scripting !!