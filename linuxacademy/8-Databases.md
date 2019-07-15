# Databases

1. Self Managed EC2
2. RDS
3. DynamoDB
4. Neptune
5. Elastic Cached

## EC2 Self Managed Databases
- very specific use case
- should consider using IOS
- ONLY USE WHEN YOUR DB is not supported as a manged db by aws
- no HA by default
- yours responsible
- root level access
- rapid provisioning as compared to rds
- more control all round

**DB Models**
SQL vs NoSQL
ACID vs BASE

Column based  - Red shift -> good for data warehouse, analytics and reporting

## RDS
- fully managed SQL based databases
- HA by default
- subnet group - where the rds goes
- auto HA when deployed for prod

Supported DBS
- Aurora
- MySQL
- MariaDB
- PostgreSQL
- Oracle
- MS SQL Server

**RDS**
- get to select multi AZ HA or single AZ deployment
- HA - cname to master can failover to slave
- maste and slave setup in differnt AZs
- with dedicatd EBS volume backing in each AZ
- cant access the slave directly (unlike aurora)
- can select instance type
- prefixed with db for dedicated rds
- can choose ebs storage type as well
- can associate with SGs
- TDE - transparent data encryption
- EBS Encryption - no need for TDE just encrypt EBS volume

- Option group and Parameter Group

**Backup**
- 2 types of backup

- automatic backup = has a default retention rate
- also store tranaction log
- can do point in time restore via tranaction logs

- snaphoting
- cannot do true point in time recovery

**Logs**
- cannot integrate with cloudwatch
- can pipe all kinda db logs to cloudwatch

**Setup**
- dees take sometime to provision and setup

**Payment Model**
- on demand or reserved
- can vary how much you pay upfront
- you pay for backups as well in S3
- can only restore to a new DB

**Read Replica**
- not fully consistent
- multi tiered set of Read Replica
- can be used for read but not writes
- useful for DR strat
- no auto failover avaliable
- can be configured to log in with iam
- read replica of a read replica

## Aurora
- fastest growing service 
- diffenrnt from RDS multi AZ mode
- share the underlying clsutered storage
- pooling read replicas endpoint
- automatic failover
- a replica in every AZ
- can use MySQL cross region read replication
- legit point in time restore
- clonning does a diff instead of a full clone
- really fast
- multi master support cmg soon
- add auto scaling read replicas

**Parallel Query**
- favour long running queries
- massive performance benifits

RDS VS AURORA
- Slave can now be access for reads
- ro endpoint pooling
- auto failover to read replicas
- shared underlying storage
- supports backtract to a previous poin in time state
- without creating a new db
- auto scaling for read workloads
- cmg soon- multi master

Aurora Global Database
- extention of a cross region replica
- replicates with less lag
- and improved performance
- read scalability
- promote region for failover

**Aurora Severless**
- expose aurora api via rest
- no need to declare storage
- based on aurora capactiy unit
- shared proxy layer
- warm instance pool
- cost benifit
- can power of when no connections
- query over standard api
- still got the normal endpoint

limitation
- no multi az architecture
- slightly longer failover as compared to aurora
- RDS > aurora serverless > aurora

## Amazon Athena
- serverless 
- sql-like query on S3
- create a schema and run queries against it
- schema on read
- overlay schema ontop of data
- pay for amount of data you process
- define schema first
- without the need for ERT
- dosent alter the source data

supported format
1. XML
2. JSON
3. CSV
4. TSV
5. AVRO
6. ORC
7. PARQUET

Basically can query most of he aws logs
- cloudtrail
cloud front 
all the ELBs
vpc flow logs

## DynamoDB
- true database as a service
- regional service
- HA by default
- key value store
- schemaless
- mandatory -> primary key

Item -  a row in rdbms
cannot have duplicate primary key
attributes - 

Limits
- 400 kb per item

Primary Key
- partition pk + optional sort key
- primary key has to be unique

- read the entire item
- take note for billing

2 consideration
- total sum of data
- performance demands (read write capacity) roundeed up

Scan vs Quries

Querires
- efficient
- based on partition key
- use sort keys to reduced data quried 
- filters do no reduced the items scanned
- ONLY SORT KEY CAN BE USED TO REDUCE READ CAPACITY
- always prefer this unless you need to query multiple partitions

SCAN
- capacity is entire table
- dont have to restrict to a partition key
- requires full capacity of the table

IMPORTANT

Pratition
- 10 GB of data
- 3000 read capactiy unit
- 1000 writes capacity unit
- cannot be removed

Performance
- try to spread keys out
- performace is capped per partition for read write

Indexes 2 types
- global secondary index
- local secondary index

LSI
- can  only be create at the time of creation
- declare another sort key
- share capacity with the table

GSI
- declare another pk and sort key
- has its own read write

Consistency model
- eventual consistency

Backup
- full backup and restore into S3
- can restore from a backup
- restore indexes as well

Encryption
- not by default
- can be turned on

Transaction
- now supports atomic transaction

Full metrics with alarms support

Can purchase reserved capacity

Can apply read and write capacity to a table
- one read is 4kb
- one write is 1kb

Autoscaling
- like ec2
- on demand
- upper and lower limit

On Demand
- if you dont want to treshold
- important tables

Adaptive Billing
- like burstable billing


DynamoDB Streams
- primary key
- Stream changes
- can choose if you want old and new changes
- triggers -> must enable streams 
- can be used to react to new entry
- ttl - expire items in a table in epoch - can be quite useful

Global Tables
- replicated tables in multiple regions
- start with a single
- Streams needs to be enabled
- cannot add regions once you have data
- can remove regions

Fine Grain Access Control
- use vars in IAM policy

DAX
- in mem acceleration product
- something like elastic cache
- in mem cache


## Neptune
- Fully managed graph database as a service
- Very niche
- relationship based data set
- access via Gremlin and SPARKQL 
- accessed via a vpc
- HA by default
- Highly performant

## Amazon Quantum Ledger Database (QLDB)
- verifiable ledger
- append only ledger
- acid document db
- SQL is supported
- uses cryptographic hashes
- block chain tech

## DocDB
- MongoDB
- you have to manage the cluster
- security group
- primary instance - read write
- the rest will be the read replicas, used to scale read heavy workload
- uses aurora underlying storage
- single storage layer
- seperating compute and storage
- cant restore in place
- restore to a new one
- auto failover
- tls by default
- similar to mongo DB

## Elastic Cache
- low latency temp
- fully manage in memory store
- Redis and Memchached

Redis
- rich set of features
- format - list, sorted set , tuples
- advance data structure
- backup and restore
- HA by deault - auto failover
- read scaling
- acid transaction

MemchaceD
- simple use cases
- format - basic key value set
- scales verticallyQ