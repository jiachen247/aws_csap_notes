# Architecting to Scale

Loosely Coupled Architecture Benefits
- layers of abstraction

Horizontal vs Vertical Scaling

Scale in vs Scaling down

4 Scaling Options
1. Maintain
2. Manual
3. Schedule
4. Dynamic
   
**Launch Config**
- first step to auto scaling
- vpc and subnet
- can attach to ELB
- health check grace period -defines how long to get settled before its subject to a health check
- scale based on different factors


**Scaling policies**
1. Target Tracking Policy - eg cpu utilizations exceeds 70%
2. Simple Scaling Policy (cool down period)
3. Step Scaling Policy - more gradular controls (warm up period)

**Scaling Cooldown**
- come up to speed and absorb load
- scale down should hae a smaller cool down
- time taken for it to setup and stabilize

cooldown vs grace period
- grace period is time subjected for a health check
- cooldown is time taken before another scalling event can take place
  
## Kinesis
- Collection of services
- made up of shards
- each shard can inject 1000 records per second
- default shard limit is 500
- record is what comes in
- record contains partition key, sequence number and data blob
- transient data store
- write out to a database or S3 or process it

**Kinesis Consumer Library**
recommended way to console data from kinesis

**Kinesis Producer Library**
recommended way to push data into kinesis

**Video Streams**
**Firehose**
**Real Time Analytics in real time**

shards - lane in a highway


## DynamoDB
- 400KB limit per item
- unlimited item

**Partition Calculations**
By Capacity  (read per 3000 writes per 1000)
By Size
Total Partition
partitions = ceil(max(capacity, size))
rw capacity spread evenly


partitioning is based on hash

autoscaling for dynamoDB
- increase partitions
- uses target tracking
- no way to scale down if consumption drops to zero - 
  - send minimal request until it scales down
  - manually reduce the max capacity to be the same as the minimum capacity
  - also supports global secondary indexes

## CloudFront
- static and dynamic content
- support flash media RMTP protocol
- supports media and live streaming over http
- multiple origins S3 EC2 ELB or another webserver
- multiple origins can be configured
- Behaviours can be used to filtered based off url


Invalidation
- delete the file from the origin + wait for ttl to expire
- use CLI to send an invalidate request based on resource or path
- third party tools

**Zone Apex Support**
Allows based urls github.com

**Geo-Restriction**
can be used to allowed  only certain countries to view content.

## Simple Notification Service
- pub sub model
- topics are like an outbox
- subscriptions to topics 
- can be used with device messaging
- can be used to decouple applications

**Fanout Architecture**
- multiple listeners

## Simple Queue Service
- fully managed 
- transient storage
- FIFO support
- 4 days default lifetime
- 14 days max lifetime
- max message size of 256kb
- Java sdk to store in s3 and enqueue the pointer to sqs instead
- scaled by increaseing workers

**Standard Queue**
- no assurance to the order

**FIFO Queue**
- maintained the order in which the messages are rv
- if anything happens to a message itll hold up all the other messages behind it


**Amazon MQ**
- impl of Apache of active MQ
- message broker
- less tightly integrated
- fully managed and HA within a region
- websocket support
- MQTT JMS

## Lambdas
- serverless
- node py go c# java
- stateless
- event basis
- no limit to scaling
- aws handles the scaling

**Dead Letter Queue**
- throw all the errors to the DLQ
- and trigger a notification
  
## Simple Workflow Service
- static tracking system
- supports sync and async
- human enabled workflow
- step functions over simple sws
- long polling

Activity Worker - does the work

Decider - coordinates the tasks

## Step Functions 
- in the context of lambdas
- managed workflow and orchestration plateform

**Amazon State Language**
- json based

**State Machine**


## Batch
spins up scheduled batch jobs on ec2 instances

- manage batch processing on EC2 instances
- managed on unmanaged 
- spot or on demand 
- job queue
- job definition
- 
step over sws for new applications - aws recommends

Step functions vs Batch

use cases

Order processing flow - step
manual steps or intervention required eg load application process - SWS
Store and forward  eg Image Resize process - SQS
scheduled or recurring tasks with out heavy logic - Batch

## Elastic Map Reduce
- isnt a single product
- a collection of open source project
- hadoop, hbase, hivee


Hadoop HDFS and MapReduce 

HDFS - just the format that data is stored to make it easy to process

MapReuce - is the process

Zookeeper - resource coordination

Oozie - workflow framework

Pig - scripting framework

hive - sql interface

mahout - machine learning componenet

Hbase - columner database for storing data

Flume - log collection

Sqoop - import of data from other data sources

Ambari - Management and monitering tool

Enterprise support - HORTONWORKS and CLOUDERA

**Elasitc Map Reduce (EMR)** 
managed hadoop cluster
log analysis or financial analysis

Step - a programmtic task performed on the data eg count words

Cluster - collection of EC2 instances provisioned by EMR

**Setup**
Master Node - 
Core Nodes - hdfs
Task Node - ephemeral storage

Additional Resources

https://d1.awsstatic.com/whitepapers/aws-web-hosting-best-practices.pdf

https://d0.awsstatic.com/whitepapers/aws-scalable-gaming-patterns.pdf

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

https://d1.awsstatic.com/whitepapers/cost-optimization-automating-elasticity.pdf

https://www.youtube.com/watch?v=w95murBkYmU

https://www.youtube.com/watch?v=dPdac4LL884

https://www.youtube.com/watch?v=9TwkMMogojY

https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf

https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf