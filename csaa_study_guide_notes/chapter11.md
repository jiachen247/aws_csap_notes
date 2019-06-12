# Additional Key Services

4 cats
1. Storage & Content Delivery
2. Security
3. Analytics
4. DevOps

## Storage & Content Delivery

### Amazon CloudFront
Amazons CDN

transparent to end user

> A Content Delivery Network (CDN) is a globally distributed network of caching servers that
speed up the downloading of web pages and other content.

Really good integration with other aws services

supports video streaming oer http and rmtp

**Distributions**
collection of resources
d11111.cloudfront.net

uses route53 to work the cdn

**origin**
a distribution must have an origin
can be a s3 bucket, ec2 instance or an ELB or website

**TTL**
can be customized

For web resources,
best practice to use versioning for files

`assets/v1/css/narrow.css`

due to old versioning living in the cache

**Dynamic Content** & **Multiple Origins**
cdns are normally used for static content
route *.php and *.jpeg to different origins
using cache controls headers

**Private Content**
signed urls - time limited
authorizaton vis cookies
Origin Access Identities (OAI) restrict access to an S3 bucket only through a cdn

anti patterns
not useful if
all users in a single geo location
or all connect vn

### Amazon Storage Gateway
> AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-
based storage to provide seamless and secure integration between an organization’s on premises IT environment and AWS storage infrastructure

industry standard software

download as a VM and run in your on premise location 

does caching 


### Type of volumes
https://docs.aws.amazon.com/storagegateway/latest/userguide/StorageGatewayConcepts.html

**Gateway-Cached Volumes**
> By using cached volumes, you can use Amazon S3 as your primary data storage, while retaining frequently accessed data locally in your storage gateway.

expand on premise storage into S3
all data stored in this volumes are copied over to S3

PIT snapshots can be taken

max size of 32TB per volume
up to 32 volumes per gateway
total => 1 PiB

**Gateway-Stored Volumes**
used for backup of data
> By using stored volumes, you can store your primary data locally, while asynchronously backing up that data to AWS

1 GiB to 16 TiB per volume
up to 32 volumes
total => 0.5PiB

**Gateway Virtual Tape Libraries (VTL)**
Cost Effective
up to 1,500 tapes (1 PB)

Stored in Glacier

Virtual Tape Shelf (VTS)
allowed one a region
stores tapes
multiple gateways in the region can share a VTS

tapes can only be accessed through the Storage Gateway/API

save up to 75% of cost
can be used with tape system
mounted as if it is a real tapedrive
once ejected, tapes are stored in glacier or deep archive

#### Fail Over
cannot be access using s3 tools but only through the gateway
since data cant be accessed
a new instance is created with a storage gateway and then attached to the backup

## Security
highest priority in AWS

### AWS Directory Service
managed servcice
> provides directories that contain
information about your organization, including users, groups, computers, and other
resources.

used for identity management within an org

supports 3 ADs
1. Microsoft AD Enterprise 
2. Simple AD
3. AD Connector

multi az deployment with health checks
data replcation and fallback
snapshots nd backups

**Microsoft AD**

**Simple AD**
ms compatiable
powered by Samba 4

**AD Connector**
Connects an on premise microsoft ad to the cloud
can be used for local MFA


Can use IAM roles to work with AD - so cool

Enterprise vs Simple AD
microsft standard version supports about up to 5000
anything more shd use enterprise

must remember 5000

AD Connector - use when you have an on premise AD you want to access in the cloud

### AWS Key Management Service (KMS)
used to manage and rotate and secure encryption keys
USED FOR SYMMETRIC KEYS

keys can never be exported. can be used to encrypt and decrypt.

1. Customer Managed Keys 
   1. Customer Master Key
   
2. Data Keys
3. Envelope Encryption
4. Encryption Context

Customer Master key never leaves KMS unencrypted
can be used to generate data keys up to 4 KB
that can leave unencrypted that can be used to encrypt large objects

Envelope Encryptin
- step1 : a key is created 
- step2 : key is encrypted with CMK an return plain text key and encrypted key
- step3 : use plaintext key to encrypt whatever you want 
- step4 : key is stored in the app along side encrypted data
- step5 : app calls KMS to decrypt when it needs it
- step6 : decrpts with the key
- step7 : wipe key from memory

**Encryption Context**
optional
must be the same during encryption and decryption for it to pass
dont really know use cases for this

### AWS CloudHSM
USED FOR ASYMMETRIC KEYS
hsm does the crypto operations as well in contrast to KMS

### AWS CloudTrail
logging capabilities


ClouldTrail vs CloudWatch

CT - 
concerned with who did what
governance compliance
user level
audit trail
logs all interactions by iam users with aws

logs to S3 bucket by defalt
can send logs to cloudWatch as well
can have sns to message when a new log file is added to S3


**All Regions**
default
**Single Region**
can be created


all encrypted with SSE on S3 by default

updated within +15 mins of api call
used for compliance
check for unauthorized access

## Analytics
How to handle so much data

### Kinesis

use kinesis if you need things in real time

> Amazon Kinesis is a platform for handling massive streaming data on AWS, offering powerful
services to make it easy to load and analyze streaming data and also providing the ability for
you to build custom streaming data applications for specialized needs.

#### Amazon Kinesis Firehose
stream massive amounts of data into aws like a firehose

need to configure a destination
- Elastic Search
- Redshift (written to S3 first then COPY over)
- S3

put data through API calls
play 
#### Amazon Kinesis Streams
build and manipulate streams for complex analysis in real time

**Shards** are used to scale kinesis automagically

runs in close to real time


#### Amazon Kinesis Analytics
easily analyze streaming data in real time with standard SQL

Data Ingestion
consume large amounts of data reliable in real time

Real Time processing of Massive Data  Stream

### MapReduce
Fully managed Hadoop instance
elastic
can use spot instances to save cost

 Hadoop MapReduce
 used to process and parallize data processing

 Nodes run on EC2 instances

 choose version
 1. Apache Hadoop
 2. MapR Hadoop

Storage
1. Hadoop Distributed File System (HDFS)
2. EMR File System (EMRFS)

* can choose a mixture of both

EMRFS allows you to store data in S3 and is persisted

**Persistent clusters**
runs 24/7
can take advantage of fast io with instance storage running HDFS

used for
1. Logs processing
2. Click Stream analysis
3. Genomic and Life science applications

### AWS Data Pipeline
not in real time
used to process transform and transfer alot of data
used for ETL (Extraction, Transform and Load) 

runs on a schedule

can launch and tear down instances needed

retry 3 times by default if an activity fails

used to move data around

IMPORTANT

**AWS Data Pipeline is best for regular batch processes instead of for continuous data streams;
use Amazon Kinesis for data streams.** 

### AWS Import/Export
moving data is hardworking due to network IO

**AWS Snowball**
50TB and  80TB
availability depends on region
encrypted with KMS

this is how it looks like
https://www.youtube.com/watch?v=kF7c_5sCCNU
i think the names might have changed abit

**AWS Import/Export Disk**
you buy and manage the storage mediums

import export from the cloud

optional encryption

upper limit of 16TB

you buy and maintain the hardware

## DevOps

### AWS OpsWorks

AWS OpsWorks is a **configuration management service** that helps you configure and operate
applications using Chef

including package
installation, software configuration, and resources such as storage.

works with linux and windows
for new or existing ec2 instances

configure multiple servers

**Stack**
Opswork component
container for AWS resources
can be launched inside VPCs

stacks, layers, and apps

can moniter with cloud watch

### AWS CloudFormation

> AWS CloudFormation allows organizations to deploy, modify,
and update resources in a controlled and predictable way, in effect applying version control to
AWS infrastructure the same way one would do with software.

launch servers thorugh templates
save templates in S3

can update the stack by updating the template using changesets


### AWS Elastic Beanstalk
meant as simple devops for devs and not ops

> AWS CloudFormation allows organizations to deploy, modify,
and update resources in a controlled and predictable way, in effect applying version control to
AWS infrastructure the same way one would do with software.

meant for dev to launch their application level code quickly without worrying about devops

**Components**
environments, versions, and environment configurations. 

conceptually a folder

### AWS Trusted Advisor
youre trusty advisor that makes recomendations based on best practice

RED YELLOW AND GREEN

can check service limits

security configs

### AWS Config
> AWS Config is a fully managed service that provides you with an AWS resource inventory,
configuration history, and configuration change notifications to enable security and
governance.

one stop shop for aws configurations
moniter changes to config

Discovery - discover all resources in the account

Change Management - log changes made to resources

Continuous Audit and Complience - helps with standards

Troubleshooting - 

Security and Incident Analysis




// Side note - lambdas not mentioned and SageMaker and Neptune which seem to be quite big aws services. Maybe it was before its time.

