---
title : "CICD Auto Deploy Static Page With GitHub"
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 2.1.8 </b> "
---


#### Why we need CICD Auto Deploy Static Page With GitHub
Continuous Integration/Continuous Deployment (CI/CD) and automated deployment of static pages with GitHub provide several benefits:

+ Efficiency: Automating the deployment process reduces manual intervention, which saves time and effort for developers. They can focus more on writing code and less on deployment logistics.

+ Consistency: Automated deployments ensure that each update to the static page is deployed consistently. This reduces the risk of human error that can occur with manual deployments.

+ Faster Time to Market: CI/CD pipelines can automatically trigger deployments as soon as changes are pushed to the repository. This allows for quicker release cycles and faster time to market for new features or updates.

+ Reliability: Automated deployments with CI/CD pipelines are typically more reliable than manual deployments. The process is standardized and can include automated tests to catch bugs before they reach production.

+ Version Control Integration: GitHub provides version control for the codebase, and integrating CI/CD with GitHub allows for seamless deployment of changes. Developers can easily track changes, roll back to previous versions if needed, and collaborate more effectively.

+ Scalability: As the project grows, automated deployment scales effortlessly. Whether it's a small personal project or a large enterprise application, CI/CD pipelines can handle deployments of any scale.

+ Cost-Effectiveness: By automating the deployment process, organizations can save costs associated with manual labor and reduce the risk of downtime or errors that could result in financial losses.

+ Visibility and Monitoring: CI/CD pipelines provide visibility into the deployment process, including logs and metrics. This allows teams to monitor deployments in real-time, identify issues quickly, and troubleshoot effectively.

In summary, CI/CD auto-deployment with GitHub for static pages streamlines the development workflow, improves reliability, and accelerates the delivery of updates to end-users.
#### Hands-on
![CICD Auto Deploy Static Page With GitHub](/images/2/CICD2.jpeg?featherlight=false&width=80pc)
![CICD Auto Deploy Static Page With GitHub](/images/2/CICD3.jpeg?featherlight=false&width=80pc)
+ Go to yours repository github, click Settings and Click to Pages.
+ Next, clik to Static HTML
![CICD Auto Deploy Static Page With GitHub](/images/2/CICD4.jpeg?featherlight=false&width=50pc)
+ Next, enter yours commit message and click Commit Change
![CICD Auto Deploy Static Page With GitHub](/images/2/CICD7.jpeg?featherlight=false&width=50pc)
+ Now, back to Code tab, view deployment history and click to detail
+ Awesome, that’s deploy at Url: https://daotq2000.github.io/note-app/
![CICD Auto Deploy Static Page With GitHub](/images/2/CICD5.jpeg?featherlight=false&width=50pc)
+ After open ours website

**Almost done, we have successfully create serverless application with Lambda Function and API gateway, DynamoDB and Integration Auto Deploy with Github Page. Next we will create CI/CD for auto deploy lambda function when has commit change from source lambda**