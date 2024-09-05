---
title : "3. The ways optimization Application handle millions request"
description: "Spring Boot Optimization Tips: Ensuring Your Application Is Always Available to Millions of Users"
date : "`r Sys.Date()`"
weight : 3
chapter : false
image: "images/4/OptimizeSpring%20BootPerformance.png"
---
## Architecture Overview
![PMS_Overview.png](/images/4/PMS_Overview.png)

The system is developed based on microservice architecture, each service will perform tasks, responsible based on the business logic domain that it is defined. The system is built on core Java
Application uses Spring Boot Framework, Spring Data JPA, Spring Cache, Hibernate... and classic libraries such as Apache Kafka, Elastic Search...

Dividing the application into different services makes it easier to manage in maintaining, processing data, helping to increase efficiency in the process of scaling up the application without being limited like the old-style mothonic architecture.
The application is currently running on PRODUCTION, because it is an application for all employees of company X, there will be 8,000 - 10,000 active users every day, with the number of requests to be processed up to millions of traffic per day.
## Current status
During a long period of running time, the application grows to tens of thousands of users, generating millions of requests each day, so reading/writing data continuously causes a large amount of data to be stored in the database, CPU Database
often reaches 100%, APIs have slow response times, users often complain about the application freezing, lagging, long waiting time, recording APIs with response times over 10s.

![List of APIs, services with response times > 5s, 10s.](/images/4/elk_full_cpu.png)
List of APIs, services with response times > 5s, 10s.

![CPU database often reaches 100%.](/images/4/BeforeOptimize.png)

CPU database often reaches 100%.

## Causes of overload
### 1. Increased traffic
![Increased traffic](/images/4/traffic.png)
Application development brings benefits but also challenges to ensure the application works well and smoothly, having **100-10000** users per day will cause the number of requests to increase suddenly, related to the timely expansion of the application to meet high traffic

### 2. Unoptimized code
The main reason is due to hot development, features are continuously developed according to the agile model without much time to optimize, code control is still not tight, causing lines of code that are not really good,
not really optimized to be put into production, making the application slow and even slower, APIs are constantly **[503-Service Unavailable](https://fptshop.com.vn/tin-tuc/danh-gia/loi-503-service-unavailable-la-gi-155247), [504-Gateway Time-out](https://cloud.z.com/vn/news/504-gateway-time-out/#:~:text=out%20l%C3%A0%20g%C3%AC%3F-,L%E1%BB%97i%20504%20Gateway%20Time%2Dout%20xu%E1%BA%A5t%20hi%E1%BB%87n%20khi%20c%C3%B3%20s%E1%BB%B1,kho%E1%BA%A3ng%20th%E1%BB%9Di%20gian%20quy%20%C4%91%E1%BB%8Bnh.)**
due to the sudden increase in traffic, the application cannot scale in time, so it will be overloaded.

### 3. SQL Query is not optimized.

The main reason for this overload is that SQL Query has a large [query cost](https://wecommit.com.vn/sql-execution-plan-trong-toi-uu-sql/), taking up a large amount of CPU during execution. Below are some possible reasons:

+ Frequently used queries, large query cost, not optimized.

+ Using too many subqueries makes the query slow.

+ Selecting the entire table, lacking the selection of necessary fields.

+ Query full scan table, not properly indexed, causing queries that are rarely used to re-query, and queries that return large amounts of data are not used or indexed.

+ Not dividing partitions with large data tables, not limiting results according to where queries ....
+ Abusing the use of group by, having.
+ Not understanding, not knowing how to analyze [execution strategy](https://wecommit.com.vn/sql-execution-plan-trong-toi-uu-sql/) of SQL statements, leading to non-optimal queries, high execution costs.

### 4. Improper cache application
![cache.png](/images/4/cache.png)
Using cache is very important in optimization, for data that rarely changes, can be reused, we should use cache to optimize performance, instead of opening a connection to the database each time we get data
and get data, we will use cache, the application will read data from the cache and return it to the end-user, so the latency of APIs will be reduced to milliseconds, helping to increase the performance of APIs to respond to hundreds of thousands, to millions of requests per second.
However, using cache also requires a strict cache invalidation and update strategy because if it is not strict, the data will be incorrect, missing, or redundant compared to the data in the database.

## Solution to the problem
After finding the cause, we will continue to solve the problem. To solve the problem, we need to know the root of why the code is not optimized? Why is the query not optimized then we will find a way to fix it.
And this is the result after optimization, the performance increased significantly, the average processing time of the api ranged from 100-300ms, no database CPU was recorded > 50%, as you can see in 1 day of services. can handle millions of requests without any performance issues,
That is the premise for the application to expand and grow further.
![after.png](/images/4/after.png)
![elk_optimize1.png](/images/4/elk_optimize1.png)
![apis.png](/images/4/apis.png)
Let's learn with us about common mistakes when programming and how to handle each problem through a series of articles on optimizing code for Java Application and SQL (Postgresql).

### [1. Optimize code for Java Spring boot Application](/optimization/optimize-springboot-performance/)
### [2. SQL optimization](/optimization/optimize-sql/)
---
**Keywords**: [aws hands on for beginners](https://auto.io.vn), [aws hands on labs pdf](https://auto.io.vn), [aws hands on project](https://auto.io.vn), [aws hands on training](https://auto.io.vn), [aws hand ons](https://auto.io.vn), [aws hands on exercise](https://auto.io.vn), [aws hands on workshop](https://auto.io.vn),[aws hands on course](https://auto.io.vn), [aws hands on labs](https://auto.io.vn), [aws 100 days](https://auto.io.vn), [what is aws?](https://auto.io.vn/1-introduce-aws)
