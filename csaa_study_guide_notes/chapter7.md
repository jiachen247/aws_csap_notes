# Databases

3 Types

1. Relational Databases
2. NoSQL Database
3. Data Warehouse

OLTP vs OLAP
Online tranaction processing
Online analytical processing

Data warehouse
complex queries
batch updates
Amazon Redshift

## Amazon Relation Database Service (RDS)
fully managed db instance
no shell access

>  AmazonRDS currently supports the following database engines: MySQL, PostgreSQL, MariaDB,Oracle, SQL Server, and Amazon Aurora.

instance classes borrowed from ec2
db.t2.micro

DB parameter group
> each default DB parameter group contains database engine defaults and Amazon RDS system defaults based on the engine, compute class, and allocated storage of the instance. 

DB options group
> An option group can specify features, called options, that are available for a particular Amazon RDS DB instance. 

Migration
with existing tools
AWS Database Migration Service, for a GUI and to convert between engines

**Operation Benefits**
consistent - underlying impl cannot be changed


**Licensing**
License Included
license held by ms and included in the price

Bring Your Own License (BYOL).
bring your own
you manage and track your licenses

Amazon Aurora
redseign of MySQL to be more service oriented
one primary for read/write
can have many replicas in mutiple AZs

**Storage Options**
built upon EBS
general purpose vs provisioned IOPS

**Backup and Recovery**
supports automated and manual snapshots

Recovery Point Objective (RPO)
agreed upon goal on how much data can be lost

Recovery Time Objective (RTO)
amount of time recovery can take

by default midnight utc

> RPO is defined as the maximum period of data loss that is acceptable in the event of a failure or incident. For example, many systems back up transaction logs every 15 minutes to allow them to minimize data loss in the event of an accidental deletion or hardware failure.

> RTO is defined as the maximum amount of downtime that is permitted to recover from backup and to resume processing. For large databases in particular, it can take hours to restore from a full backup. In the event of a hardware failure, you can reduce your RTO to minutes by failing over to a secondary node. You should create a recovery plan that, at a minimum, lets you recover from a recent backup.

Automated Backups
one day of backups will be retained by default
when you delete a DB all snapshots are deleted as well manual snapshots are not deleted

Manual Snapshots
can be used to restore a DB
kept until explicitly deleted

use Multi AZ when doing backups
to reduce latency when IO is suspended for the duration of the snapshot

**High Availability with Multi-AZ**
master and slave architecture
master is for read write
slaves are replication only
in different AZs
sync

automatically fail over with out admin intervention by updating the dns CNAME record

**Scaling Up and Out**

Vertical Scalability
by upgrading the instance type for more compute power
can also increase storage

scaling horizontally is harder
more a use case for nosql databases

Horizontal Scalability with Partitioning
by sharding / partitioning

Horizontal Scalability with Read Replicas
only can be used with the open source engines
use read replicas for heavy read usecases
blogs

can be cross region



Multi AZ|Read Replica
-|-
synchonus, writes are replicated to all instances |async
only th primary is active|replicas are active for read requests
different az same region| anywhere
upgrades happen on the primary| upgrades happens independently
automatic failover|manual promotion


**Security**

private subnets

use encryption in flight and at rest

at rest -> kms for open source, TDE for oracle and microsoft
in flight -> ssl/tls by default

create users with strong passwords

## Amazon Redshift
data warehouse designed for OLAP

table design a little different
perfomant optimization


data compression
reduce IO -> faster
COPY command to automatically compress


distribution key


use VACUMM
follows mvcc in postgress
does not actually delete rows

**Clusters and Nodes**
> A cluster is composedof a leader node and one or more compute nodes
> Your application or SQL client communicates with Amazon Redshift usingstandard JDBC or ODBC connections with the leader node, which in turn coordinates queryexecution with the compute nodes.
>
applications interact only with the leader node

one data warehouse has many clusters
each cluster contains compute nodes and a leader node
compute nodes are made up of many slices

each cluster is divided into slices
2 to 16 depending

**Resizing Clusters**
new cluster created and migrated
becomes readonly during migration


**Table Design**
supports `CREATE TABLE` also takes in compression encoding, distribution strategy, and sort keys.

Compression Encoding
- reduced space required to store files
- reducing IO

**Distribution Strategy**

Distribution significantly affects the speed of queries


Types of distribution for each table

1. Even Distribution
   default, distributed uniformly independent of the data
2. Key Distribution
   clusted around a certain key
3. All Distribution
    replicated table in all nodes

**Sort Keys**
enables fast range predicates in queires

 compound or interleaved

 compound used for prefixes
 interleaved is used for comparing any substring
 i think

 **Loading Data**
 supports insert and update
 use `COPY` for batch and mass updates or inserts
 `VACUM` to reclaim space casue it uses lazy deletion or mvcc like postgress to be more accurate

 `UNLOAD` to export data

**Querying Data**
can use `SELECT` command

can use CloudWatch and Redshift console to monitor performance

use WLM to queue manage and optimize queries
> Workload management (WLM) optimizes query performance by allocating cluster resources to query queues. You can specify Auto WLM to let Amazon Redshift actively manage concurrency and memory allocation, or you can create a custom configuration with multiple queues

**Snapshots**
- create PIT snapshots
- can be used to restore
- stored on S3
- same rules apply (manual snapshots must be deleted explicitly)

**Security**
- IAM
- VPC
- NACL
- Database user
- encryption at rest via KMS

## Amazing Dynamodb
fully manage NoSQL service
fast low latency
scales v well

**Data Model**
- Tables Items and Attributes
- 400KB item size limit
- name value pair
- no predefined schema
- schemaless
- only requires a pk
- exposed over HTTP(S)

**Data Types**
super flexible
Scalar, Set, or Document

**Scalar**: String, Number, Binary, Boolean and Null
**Set**: unique list of scalars, String set Number set and Binary Set
**Document**: List or Map (json objects



dont read write on the same primary key
use CloudWatch for detailed metrics and provision capacity

Secondary Indexes
just an additional index for sorting and searching

PutItem does UPSERT


PutItem vs UpdateItem
https://stackoverflow.com/questions/43667229/difference-between-dynamodb-putitem-vs-updateitem


conditionals via expect

**Secondary Indexes**
Global Secondary Index
can be created at any time
can have as many as you want

Local Secondary Index
only can have one
must be created when the table is created

**Consistency**
eventually consistent or strongly consistent (consistency guaranteed)

**Batch Operations**
BatchGetItem and BatchWriteItem
(up to 25 writes per request)

**Searching Items**
Query and Scan

Query just takes the data you filter
Scan takes all the data then filters

**Scaling and Partitioning**
Amazon DynamoDB stores items for a single table across multiple partitions, as represented in Amazon DynamoDB decides which partition to store the item in based on the partition key.

distribute keys evenly to take advantage of throughput

**Security**
use ec2 instance profile with a role fr access control

fine grain controls available what table to read and write too.

**Streams**
intreasting usecase
changes are streamed as shards in real time
can be integrated with the kinesis adapter
can integrate with lambdas
still abit effy with the exact use cases

# Additional References
https://www.youtube.com/watch?v=GgLKodmL5xE

https://www.youtube.com/watch?v=lQEMV_Qgjrw

https://www.youtube.com/watch?v=tDqLwzQEOmM
