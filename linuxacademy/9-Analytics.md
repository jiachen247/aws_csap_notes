# Analytics, IoT, and Streaming

## Amazon EMR - Elastic Map Reduce

MapReduce
- good for data taht can be broken up
- map and shuffle and then reduce and then combine
- efficient method to proccess big chunks of data in parallel

Splits - chunks

Creating a cluster
- create a long running cluster
- creating a targeted cluster
- clusters in a VPC
- in a single subnet  in side a single AZ
- for low latency and high throughput

Hive - run SQL like quries

Master Node
- runs in a vpc
- manages and coordinates the cluster
- moniter the health of all the nodes
- connect here for hive
- runs on EC2

Core Nodes
- zero or more
- worker nodes
- act as data node
- manages HDFS data
- can have dataloss if core node fails

Task Node
- even more optional
- no HDFS
- can use spot instance
- can afford to fail

S3 -  can store input and output to S3
- take note of storage option
- HDFS or EMRFS (backed by S3) is the only internal filesystem that can be used
- can tolerate failure of one core node
- master node cannot fail
- EMRFS has more HA


Software Level
- core haddop set of applications
- Hadoop vs HBase vs Presto vs Spark
- read more about emr

Scaling
- uniform groups - master task core
- instance fleet - optimize task - alot more flixible


Performance and Cost Optimization
- comes out for the exam

1. make sure you provision emr cluster enar to the data source
2. select the right EC2 instances - is workload compute intensive


## AWS Kinesis
a famliy of services to proccess large sets of 

Accept alot of messages from producers
- eg IOT Sensors or mobile apps tracking

Sharding
- has at least one shard
- grows with the input data

Streams
- Video Stream
- Data Stream

24 Hour Retention period

shards required = max(injest in mb, read in mb)

distributes load across all shards

Supports SSE at rest with KMS

can configure cloudwatch metric


public space sevice
- public zone endpoints

Enhanced Fan out
- dont share bandwidth
- give consumers dedicated bandwidth

## Firehose
- what to do with the data
- take data from a kinesis stream and do something with it
- deliver and manupilate data to a destination
- option to put data stright in
- can use SSE at rest if using direct put
- used to take data out of kinesis
- OR need to take IOT product data and store them on S3

Record Transformation
- lambda function - can maintain original copy
- convert record format - apache parquet to orc

Destination Services
- S3
- Redshift
- Elastic Search
- Splunk - machine intelligence tool

## Data Analytics
- running SQL queries on real time data
- you need a source

in application input stream
- virtual tables
- good for dashboarding
- real time monitering


## Redshift
- rapid deployment of data warehouse
- up to 2 petabyte of data
- can build and teardown in about 30 minutes

Column based database
- good for aggregate functions


Node type and Size - determines the laod the cluster can handle
- leader node comes free


Space Requirement
- DC2 and DS2
- Dense Compute
- Dense Storage

Slices
- data is split into slices
- and processed and then aggregated
- can deploy in a custom VPC
- VPC level resource

Resizing a cluster
- classic resize - change number and type of nodes
- elastic resize - only change the number of nodes

Does backups into S3

Enhanced Netowrking 
- for accessing aws public zone service

Integration 
- S3
- Kinesis
- Data Pipeline

**RedShift DR**
- automated snapshots by default
- can do cross region replication of the snapshots
- can restore only certain tables
- HA already built in
- has 3 time more space that is listed
- additional space used for HA to store other nodes
- Use multiple nodes always for goot HA
- always always always prefer multiple nodes  
- cause free leader node and auto replication

## IOT Suite
- wont feature that much
- but still got to know to eliminate options
- regional service


IOT Device Gateway
- MQTT Topics via X509 certs
- communicates via the Device Gateway

IOT Shadows
- aws matains a shadow copy
- other process can read and write
- when IOT device connects it is then updated

IOT Rule
- event based trigger
- really felxible

Basic Injest
- allows iot devices to trigger rules directly 
- bypassing the topic
- saving costs

## QuickSight
- not a primary product
- BI service
- visualization
- take raw data and present it to users
- its not free
- authors and readers
- standard and enterprise
- can integrate with athena and all the DBs and redshift

## Elastic Search
- know when and where to use
- works in clusters
- NOSQL database
- near realtime or preditive search
- can integrate with other products
- like cloud watch
- can use reserved instances
- master node and data nodes
- runs on EC2 as well
- encryption in flight via tls
- at rest using sse
- can be put into a vpc
- running in an seperate network
- exposed via vpc endpoint
- recomened 3 master eligible nodes
- prefer multicore over speed

ELK Stalk

E -> Elastic Search
L -> Log Stash 
K -> Kibana (log visualization)
