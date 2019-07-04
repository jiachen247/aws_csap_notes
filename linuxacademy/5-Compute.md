# Compute

## EC2
- key service

**Fundamentals**
- created from an AMI
- not HA by default
- attach SG to network interfaces
- only larger instances have instance store support

### AMIs
- metadata
  - arch an os
  - block device volumes

creator is owner

AMI are actually just metadata with points to EBS snapshots

AMI market place - dont have to pay for EBS snapshot storage

**Instance Store base AMI**
- no volume snapshots
- used files and store on S3
- takes longer to provision


**Virtualization**
hvm
hardware assisted virtualization
SRIOV for enhanced networking
emulated to hardware virtualized
AWS Nitro - near bare metal level
for exam assume nitro is the correct answer

**Instant Type**

General Purpose 
- use this is no requirement
1
M Class

1. General Purpose
   1. M Class - normal/default
      1. M5a - amd
   2. T Class - Burstable
      1. earn cpu credit
      2. deal with burst/sporadic load
         1. standard - blocked after qouta
         2. limited - charged for extra
   3. A Class
      1. ARM 
      2. up to 10 GBPS
2. Compute Optimized - C Class
   1. C5 
   2. C5N
      1. enhanced networking
3. Memory Optimized
   1. more mem
   2. instance store value avaliable
   3. R Class / default mem class
      1. a - amd
   4. X Class - even more powerful
      1. e - even more powerful
      2. H Class
4. Accelerated Computing
- P Class - general purpose GPU
- G Class - graphic intensive workloads
- F Class - FPGA

5. Storage Optimized

* Important for the Exam to look over again


## EC2 Storage
EBS vs Instance Store

**Instance Store**
- Not always avaliable
- price included on the EC2 instance
- temp
- restart will preserve data
- not elastic


EBS Optimized by default
- always turn this on
- dedicated channels for io

IOPS * IO Size = throughput

EBS Types
1. gp2 - ssd general purpose use this by default. credit base for burst
2. io1 - ssd provisioned IOPS - sustained load
3. st1 - hdd throughput optimized - burst capacity. streaming sequential data. cannot be a boot volume
4. sc1 - hdd cold storage - burst capacity. cannot be a boot volume

Not the most HA by default
- if AZ goes down ebs goes down

EBS Snapshots
- stored on S3
- can be cross az and region

MAX IOPS via EBS
- 80,000 IOPS
- 1,750 MiB/s

## Profile and Roles
instance > instance profile > IAM Role

how it works - use metadata service to get credentials

When to use: 
always prefer IAM Roles

## HPC and Placement Group
when you want the maximum level of performance
one placement group per resource

**Clustered Placement Group**
- performance benefit
- clusters resources near to one another
- faster io
- cant modify placement groups
- have to do capacity planning
- better outcome if you use the same instance types
- not all instance types are supported
- can span VPC peers

**Partition Placement Group**
- use for 1 resilience and HA
- attempts to distribute instance over different partitions within the AZ
- used for HDFS / Cassandra / HBASE 
- cant select on the console
- middle ground

**Spead Placement Group**
- used for max HAs
- can add to the spread pg later
- insist on physical speration

### Monitering with CloudWatch
- basically you can use cloudwatch with ssm agaent to do logging
- can extend metrics

## Containers (ECS)
- write code that will always work 
- lock deps
- running copy of a docker image

ECS Architecture
- Cluster - a collection of compute resources
- Service
- Task
- Container

Deployment Options
1. EC2 - you manage the ec2 container hosts
2. Fargate - manages the instances for you


Level 1: Container Definition - Base Limits
Level 2: Task Definition - higher level. networking options. can contain one or more container definitions, launch type
Level 3: Services - run one or many tasks
Level 4: Cluster - a group of tasks or services

Network Mode
1. None - no networking at all
2. Bridge - connection to the container host
3. Host - maps directly to the host network (be careful of overlaping ports)
4. AWS VPC maps ENI to the container (Fargate only supports this)
5. NAT - only option for and supported by windows.

When running on EC2, theres a limit on too many ENI per host.

### Security Implications of EC2 vs Fargate

EC2
- Normal EC2 Role 
- Task Role - finer grain - on a task basis
- Task Execution Role: ECS agent uses this to interact with other aws service

## Severless
- overused
- serverless still uses servers
- event driven 
- super scalable
- only pay for what you use


use case 
- save money
- need to scale fast

### Lambdas
- FaaS
- timeout - 50 mins
- Cold Start - new sandbox
- Target Warm Start
- Sandboxes are the boxers that functions run in
- can use env vars
- cpu grows with memory
- can turn on vpc level - but takes longer to start due to dedicated ENI

**Lambda Layers**
- used to handle deps
- add runtime support for any language
- immutable
- but can do versioning
- can be used to store commonly used libs
- functions can use up to 5 layers
- cannot exceed 250 mb
- can be used to shrink function zip sizes
- super powerful

**API Gateway**
- managed API in a serverless way
- WEBSOCKET and rest support
- regional service
- can use edge locations
- can put into vpcs
- api has version and stages
- supports Mock/Test integration for static responses
- integration with lambdas
- can do caching
- can put waf infront
- webacl

<url>/stage/methods

deployment
1. in a private vpc 
2. edge location