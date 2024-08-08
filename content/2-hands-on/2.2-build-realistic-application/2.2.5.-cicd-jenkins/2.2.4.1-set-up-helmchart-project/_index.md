---
title : "Set up HelmValue Project"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.5.1 </b> "
---
## 4. Setup project helmvalues
On the project, we will setup helmchart value project, it contains deployment, service of kubernetes object.

Donwload my project helmvalues at https://github.com/daotq2000/aws-zero-downtime-jenkins

### Explain for this project
The purpose of project are packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources
Setup environment secret, scale config, network type and limit CPU used like image below: 
![copy-private-key.png](/images/2.5-helm-values/explain.png)
Config Domain, Health check like image below:
![copy-private-key.png](/images/2.5-helm-values/Monitoring.png)

### 4.1 Config credential for Git
#### 4.1.1. Create ssh key for jenkins-git user to pull repository from github
Now, we need to ssh to Bastion host, where installed jenkins server and generate ssh key allow each time run build, jenkins server can pull latest change commit from github.

Follow these steps to add ssh key to github: 

![gen-ssh-key.png](/images/2.2-jenkins/gen-ssh-key.png)
Copy private key
![copy-private-key.png](/images/2.2-jenkins/copy-private-key.png)
#### 4.1.2. Go to Jenkins server, click to **Jenkins -> Management Jenkins -> Credentials -> System -> Global Credentials(unrestricted)**
Fill ID with **jenkins-git**
![selected-plugsin.png](/images/2.2-jenkins/create-ssh-key-jenkins.png)
Next, paste your private have been copy to this
![patseToSSh.png](/images/2.2-jenkins/patseToSSh.png)
#### 4.1.3. Copy public key and Go to **GitHub Preference** add public key
Go to github, click to right side bar and click **setting**
![github-preferences.png](/images/2.2-jenkins/github-preferences.png)
Get public key using command:

       $cat ~/.ssh/id_rsa.pub

Go to SSH and GPG keys, and paste your public key to this.
![public-key.png](/images/2.2-jenkins/public-key.png)
After successfully, we receive result below
![create-public-key.png](/images/2.2-jenkins/create-public-key.png)
#### 4.1.4. Config pipeline to fetch project
Create new project pipeline and config the pipeline.
![config-pipeline.png](/images/2.2-jenkins/config-pipeline.png)
Use pipeline below:

         def gitUrl = 'git@github.com:daotq2000/aws-spring-sqs-queue.git'
         def gitBranch = 'main'
         def gitCredential = 'jenkins-git'
         pipeline {
            stages {
                  stage('Checkout project') {
                  steps {
                  git branch: gitBranch,
                  credentialsId: gitCredential,
                  url: gitUrl
                  }
                  }
            }
         }
#### 4.1.5. Execute build
If you got result below, you have built success.
![success-build-helm.png](/images/2.2-jenkins/success-build-helm.png)
#### 4.1.6. Troubleshoot issue
Issue related build failed, may be have some reason below
1. Missing variable on pipeline
   ![issue-build1.png](/images/2.2-jenkins/issue-build1.png)
   Solution: check carefully pipeline and error message
2. Verify host failed
   If you get error message like `Verify host failed`, please following solution below:

Solution: **Go to Jenkins -> DashBoard -> Manage Jenkins -> Security -> Git Hot Key Verification and Configuration**  and choose **No Verification**
![issue-build1.png](/images/2.2-jenkins/verify-host-solution..png)
3. Git doesn't install yet on bastion host.
   Solution: Check git version on bastion host using

         git -v
if you doesn't install, let install git

      sudo yum install git -y

After we will pull repository by `git clone` command

      git clone {your_git_repository}
If you get result like image below, you are successfully pull repository to server.
![issue-build1.png](/images/2.2-jenkins/VerifiedGit.png)


