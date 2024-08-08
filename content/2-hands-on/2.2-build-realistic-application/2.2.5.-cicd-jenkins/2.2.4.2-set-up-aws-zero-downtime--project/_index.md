---
title : "Set up AWS Zero Downtime Project Jenkins Pipeline"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.5.2 </b> "
---

## Introduce 
The e-commerce project is an integration with AWS Infrastructure and microservice  application written in Java, we are using Spring Boot
Develop API to retrieve data from Client to save data into AWS Aurora database, retrieve data from database to return to customer
and update existing data, delete data, and more.

There is project structure:

![project_stucture.png](/images/2.3_project/project_stucture.png)

Refer my Github reposittory: https://github.com/daotq2000/aws-spring-sqs-queue.git

In this pipeline project, we will complete these categories:

1. Check out git repository and pull for jenkins server
2. Using [maven](https://maven.apache.org/) command to build spring boot application 
2. Docker command to compress application as container image and push it to Amazon Container Registry (ECR) 
3. Leaverage project helm-chart configure at [previous step](../2.2.4.1-set-up-helmchart-project) to pull environment variable secret key and helm chart template.
4. Using helm chart command to apply for create kubernetes object on Elastic Kubernetes Service(EKS) Cluster
## Getting Amazon Container Registry Push Command
To get ECR push command, we are follow these step:
1. Login to Amazon Console
2. Go to Amazon Container Registry
3. Click to detail project
   ![zero-downtime-pipeline.png](/images/2.2-jenkins/ecr.png)
4. View push command
   ![zero-downtime-pipeline.png](/images/2.2-jenkins/view-push-command.png)
   ![zero-downtime-pipeline.png](/images/2.2-jenkins/push-command.png)
   
### Setup pipeline
Using pipeline below:

      def isJava = true;  
      // git repo
      def gitUrl = 'git@github.com:daotq2000/aws-spring-sqs-queue.git'
      def gitBranch = 'main'
      // project
      def projectGroup = 'aws'
      def projectName = "aws-zero-downtime"
      def namespace = "ciaws"
      def helmValues = "project-values/aws-zero-down-time/values-dev.yaml"
      /// DO NOT CHANGE BELOW
      // key login
      def gitCredential = 'jenkins-git'
      def deployDirectory = '/var/lib/jenkins/workspace/AWS/AWS-DEV/helm-values/helm-values'
      def mavenBuild = 'echo 1'
      dockerBuildCommand = './'
      def version = 'dev-0.0.${BUILD_NUMBER}'
      if (isJava == true) {
      mavenBuild = '/bin/bash ./mvnw -DskipTests clean package'
      }
      pipeline {
      agent any
      tools {
      jdk 'jdk11'
      }
      environment {
      DOCKER_REGISTRY = 'https://846338211683.dkr.ecr.us-east-1.amazonaws.com'
      DOCKER_IMAGE_NAME = "eks-project"
      DOCKER_IMAGE = "846338211683.dkr.ecr.us-east-1.amazonaws.com/${DOCKER_IMAGE_NAME}"
      }
      stages {
      // stage('Update helm  value') {
      //   steps {
      //     build job: "AWS/AWS-DEV/helm-values", wait: true
      //   }
      // }
      stage('Checkout project') {
      steps {
      git branch: gitBranch,
      credentialsId: gitCredential,
      url: gitUrl
      }
      }
      stage('Build And Push Docker Image') {
      steps {
      script {
      sh "git reset --hard"
      sh "git clean -f"
      sh "${mavenBuild}"
      sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 846338211683.dkr.ecr.us-east-1.amazonaws.com"
      sh "docker build -t ${DOCKER_IMAGE_NAME}:${version} ./"
      sh "docker tag ${DOCKER_IMAGE_NAME}:${version} ${DOCKER_IMAGE}:${version}"
      sh "docker push ${DOCKER_IMAGE}:${version}"
      sh "docker rmi ${DOCKER_IMAGE_NAME}:${version} -f"
      sh "docker rmi ${DOCKER_IMAGE}:${version} -f"
      }
      }
      }
      stage('Apply k8s') {
      steps {
      script {
      sh "helm upgrade $projectName $deployDirectory/chart-template/ -f $deployDirectory/$helmValues --namespace=$namespace -i --atomic --cleanup-on-fail --wait --timeout=5m --set fullnameOverride=$projectName,image.repository=${DOCKER_IMAGE},image.tag=${version}"
      }
      }
      }
      }
      } 
![zero-downtime-pipeline.png](/images/2.2-jenkins/zero-downtime-pipeline.png)

### Testing pipeline
After done all, we have received result below:
![zero-downtime-pipeline.png](/images/2.2-jenkins/build-success.png)

### Monitoring service on Kubernetes Cluster
#### Install K9s monitoring client tools
[What is K9s?](https://github.com/derailed/k9s)
- K9s provides a terminal UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your applications in the wild. K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.

[Install K9s] (https://github.com/derailed/k9s)
After done, we can monitoring kubernetes cluster any where.
![k9s](/images/2.3_project/k9s.png)

