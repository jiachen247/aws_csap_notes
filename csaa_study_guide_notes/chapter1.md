# Chapter 1: Introduction to AWS

## Overview
1. what is the CLOUD?
2. advantages of cloud computing
3. cloud deployment models
4. aws fundamentals
5. accessing the platform
6. services
   1. compute and networking
   2. storage and content delivery
   3. database
   4. security and identity
   5. application


## 1. what is the CLOUD?

> Cloud computing is the on-demand delivery of IT resources and applications via the Internet
with pay-as-you-go pricing.

> In its simplest form, cloud computing provides an easy way to access servers, storage,
databases, and a broad set of application services over the Internet.

## 2. advantages of cloud computing

#### 2.1 variable vs capital expense
no huge upfront cost when starting a business!
#### 2.2 economy of scale
because AWS does this on such a big scale it is able to provision servers to you at a lower cost!
#### 2.3 just the right capacity
no more guessing how many servers you might or might not need
#### 2.4 increased speed and agility
provisioning of servers are a matter of clicks.
#### 2.5 focus on the business
allows business on growing their business instead of the nitty grtty details of managing a server
#### 2.6 go global
deploy globally in a matter of minutes

## 3. cloud deployment models

#### 3.1 All In
All in solutions have the entire application resources in the cloud
#### 3.2 Hybrid
Hybrid solutions are a mixture of resources in the cloud as well as resources hosted locally or in a data center

## 4. aws fundamentals
> Using AWS resources instead of your
own is like purchasing electricity from a power company instead of running your own
generator, and it provides the key advantages of cloud computing: Capacity exactly matches
your need, you pay only for what you use, economies of scale result in lower costs, and the
service is provided by a vendor experienced in running large-scale networks.

#### 4.1 global infrastructure
aws is world wide serving over 1 million active users over 190 countries. 

**region**: completely independent and isolated. spread out geographically.  eg us-east-1

**availability zones** -  isolated but connected through low latency links. eg us-east-1a

![](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/aws_regions.png)

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html

regions are made up of multiple interconnected isolated availability zones.

#### 4.2 security and compliance
security is a number one priority to aws.

> Organizations leveraging AWS inherit all the best practices of AWS policies, architecture, and
operational processes built to satisfy the requirements of the most security-sensitive
customers.

> AWS provides a wide range of information regarding its IT control environment to help
organizations achieve regulatory commitments in the form of reports, certifications,
accreditations, and other third-party attestations.

## 5. accessing the platform
* command line interface (cli)
* software development kits (sdk)
* web management console

## 6. services
### 6.1 compute and networking
##### 6.1.1 Elastic Compute Cloud (EC2)
flag ship product! gives resizable compute capabilities in the cloud.

##### 6.1.2 Lambdas
run functions in the cloud with zero administration. no paying for idle time.

##### 6.1.3 Auto Scaling
allows you to scale EC2 instances based on demand

##### 6.1.4 Elastic Load Balancing
does what it says. Load balances!

##### 6.1.5 Elastic Beanstalk 
makes life easier for devs by provisioning all the needed resources

##### 6.1.6 Virtual Private Cloud (VPC)
provision logically isolated section of the cloud. a private network of resources in the cloud.

##### 6.1.7 Direct Connect
allows orgs to establish a dedicated network connection from their data center or office to AWS.

faster than going through vpn over the internet.

##### 6.1.8 Route 53
DNS service. even works as a dns registrar! 

fun fact: why 53? cause 53 is the port number for dns resolution service :-)

### 6.2 storage and content delivery
##### 6.2.1 Simple Storage Service (S3)
aws flag ship product
oldest product. stores anything!

##### 6.2.2 Amazon Glacier
super low cost but hidden in a glacier somewhere!
read time of a several hours.

##### 6.2.3 Elastic Block Store (EBS)
persistent block storage. automatically replicated within the availability zone.

what is block storage?

##### 6.2.4 Storage Gateway
on premise software that brings the cloud down and acts as a gateway into AWS storage. 

has a front cache.

##### 6.2.5 CloudFront
amazon's cdn

### 6.3 database
##### 6.3.1 Relational Database Service (RDS)
fully managed relation database supporting many popular db engines.

##### 6.3.2 DynamoDB
nosql key value json datastore. fast lookup.
##### 6.3.3 Redshift
fully managed data warehouse 

##### 6.3.4 ElastiCache
in memory storaged used for caching. supports memcached and redis.

### 6.4 management
##### 6.4.1 CloudWatch
gather and view metrics and log files
##### 6.4.2 CloudFormation
makes devops easier by automating the provisioning of the stack. 
##### 6.4.3 CloudTrail
used mainly for monitoring api calls

[whats the diff from CloudWatch?](https://www.quora.com/What-is-the-difference-between-CloudTrail-and-CloudWatch)

##### 6.4.4 Config
stores aws configuration and changes like an audit trial.

### 6.5 security and identity
##### 6.5.1 Identity and Access Management (IAM)
default authorization mechanism in aws! use this to manage permissions.

##### 6.5.2 Key Management Service (KMS)
used for storing sensitive keys

##### 6.5.3 Directory Service
allows orgs to run ms ad on aws or inegrate with an on premise ad.

##### 6.5.4 Certificate Manager
provision and manage ssl/tls certs

##### 6.5.5 Web Application Firewall (WAF)
does what it says

### 6.6 application
##### 6.6.1 API Gateway
front door to the stack! uses a http endpoint to trigger lambdas or tasks.

##### 6.6.2 Elastic Transcoder
highly scalable media transcoding.

##### 6.6.3 Simple Notification Service (SNS)
pubsub message queue via topics.

##### 6.6.4 Simple Email Service (SES)
used to send and receive emails.

##### 6.6.5 Simple Workflow Service (SWF)
helps to run, build and scale background jobs.

##### 6.6.6 Simple Queue Service (SQS)
fully managed messaging queue.

## Additional Resource
https://www.youtube.com/watch?v=TkT4iFRkaZk&t=1731s

![](https://blogs-images.forbes.com/janakirammsv/files/2018/01/list-of-aws-2018.jpg)