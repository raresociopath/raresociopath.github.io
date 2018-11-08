---
layout: post
title: "Jenkins pipelines: Best Practices."
description: "Jenkins cool pipelines"
comments: true
keywords: "ansible, devops, tools, automation, system, administration, cloud, aws"
category: jenkins
---
## Jenkins Driven 

Jenkins remains to be the most used CI/CD tool.
Don't really remember a company or project where I didn't have to deal with Jenkins.
That's why I've decided to leave here some of the best things

## Jenkins instance


## The Jenkins Repos ( tjp )
1. <a href="https://github.com/raresociopath/jenkins-pipeline-library.git">jenkins-pipeline-library</a>
2. <a href="https://github.com/raresociopath/jenkins-jobs.git">jenkins-jobs</a>


## Mandatory plugins:

Job Pipeline


## Seed Job



## Jenkins pipeline library
Import the jenkins-pipeline-library

Go to Manage Jenkins -> Configure System

Global Pipeline Libraries
Project repository jenkins-pipeline-libraries


![Jenkins](/pics/jenkins1.png)

## Start the seed job


The seed job is the only job that you will run manually.
It should pick up all the jobs from the jenkins-jobs repository and create all the other jobs.



