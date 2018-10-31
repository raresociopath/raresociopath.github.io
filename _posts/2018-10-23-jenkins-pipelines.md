---
layout: post
title: "Jenkins pipelines: Best Practices."
description: "Jenkins cool pipelines"
comments: true
keywords: "ansible, devops, tools, automation, system, administration, cloud, aws"
---

Jenkins... Jenkins.. Jenkins..
Such a simple tool that has changed everything.

One of the most important part in the CI/CD architecture.

Let's see what we can do about jenkins.

This best practices are for big projects.

I guess the first best practice with Jenkins is that you should not edit anything directly from Jenkins, not create jobs or folders.
Everything should be automated. 

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



