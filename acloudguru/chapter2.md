# Data Stores

## Concepts

Persistent | Transient | Ephemeral
-|-|-
Glacier,RDS|SQS SNS Kinesis| EC2 Instance Store, Memcached

IPOS vs Throughput

**IPOS** - read write speed

**Throughput** - how much data can we move at one time

Consistency Models

**ACID** atomic consistent isolated durable (RDS)

**BASE** basic availability even if its not always consistent - eventually consistent (S3 dynamoDB)

## Storage
### S3
- One of the first services after SQS
- used in alot of aws services
- max object size 5 TB
- max PUT request is 5GB
- use multipart upload for more than 100MB

URL IS NOT A PATH ITS A KEY

think of S3 as a multi AZ database

#### Consistency
Read after Write Consistency for new PUT - waits until new object is replicated then return 200

Eventual Consistency for HEAD or GET of exisitng objects - may be state

Eventual Consistency for PUT and DELETES

Updates to a single key are atomic

#### Security
1. Resource based - Object ACL and Bucket Policy
2. User based - IAM Policies and role
3. MFA before Delete

#### Data Protection
1. MFA before Delete
2. Versioning
3. Cross Region Replication

#### Life Cycle Management
Apply life cycle rules for your resources

#### Analytics
Data Lake Concept w Redshift 
IOT Streaming data w Kinesis Firehose
ML and AL Storage w Lex
Storage Class Analysis w S3 Management Analytics

#### Encryption

At Rest

Option|Implementation
-|-
SSE-S3|use S3's key
SSE-C| upload your own key
SSE-KMS| use a key from KMS
Client-Side|store encrypted objects

In Flight - SSL

#### Cool Tricks
**Tranfser  Acceleration** Reverse CloudFront
**Requester Pays** - requester pays rather than th bucket owner
**Tags** tags are especially useful in S3
**Event** can emit and trigger events to other services
**Static Web Hosting** host with massive scalability
**BitTorrent** can use S3 to torrent files woah

### Glacier
- achiving service
- cold/offline service
- Used by VTL(Virtual Tape Library)
- Faster retrival if you pay more
- can be integrated with S3 lifecycle management
- **IMMUTABLEEEEEEEEEEEE**

Terms:
- Vaults => S3 Bucket
- Archive => S3 Object
- Vault Lock => enables contraints eg no delete

Access is granted via IAM

Limits: 40 TB

Vault Locks ->  initiate -> complete the lock within 24 hours -> after that it cant be undone

### Elastic Block Storage
- virutal harddrives
- can only be used by EC2
- tied to a single AZ (impt)
- pay for the entire volume

can optimize by IOPS and throughput and cost

EBS vs Instant Stores

Instant Store: fast IO but temp
EBS: snapshots and persistence

Snapshots
- backup
- share volumes with others
- migrate volume to new AZ
- convert unencrypted to encrypted
- only changes are changed (like a commit history) makes it cost effective - IMPT aws magic

### Elastic File Service
- NFS - Network File Service
- Implements NFS
- elastic - only pay for what you use
- 3x more expensive than EFS
- 20x more expensive than S3
- Configure multiple mount points
- Can be mounted on premise only with Direct Connect

**EFS File Sync Agent**
sync files in the background

Checkout not supported NFSv4 features...

### Storage Gateway
run on
- onsite as a VM
- EC2

sync on premise data to the cloud
useful in cloud migration

Modes

New Name|Old Name|Interface|Function
-|-|-|-
File Gateway|None|NFS,SMB|Allow on premise to store files on S3 via a NFS mount point
Volume Gateway Stored Mode|Gateway-stored volumes|iSCSI|sync on site data with S3
Volume Gateway Cached Mode|Gateway Cached Mode|iSCSI|only stored in S3 and cached locally
Tape Gateway|Gateway Virtal Tape Library|iSCSI|supports tape machines backup to glacier

**Bandwidth Throttleing**
use caching to throttleing

### WorkDocs
- secure fully managed file collaboration service
- Can integrate with AD for SSO
- complient

## Databases

### EC2 Database
- if you need a DB is there is no AWS s service
- can run any kind of DB you want

use case: SAP HANA not a managed service on AWS but can run on an EC2 instance

### Relaional Database Service (RDS)
RDS Anti-Patterns
- large binary or blobs
- automated scalability
- name valued pair
- not well structured/no fixed schema
- other db not supported eg HANA or DB2
- complete control over the databsae -> use EC2


supported databases -> MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server DB engines

**Replication** only supported by innodb engine. (all)

Master Slave replication

**Speical Note**
Multi AZ mater/slave -  replication is sync
Cross Region read replica - replication is async (eventually consistent)


Multi AZ Master/ Cross Region Read Replica
-|-
automatically promoted to a master if the master goes down|can be promoted manually
replication is sync|replication is async (eventually consistent)

### DynamoDB
- multi AZ, massivly scalable noSQL key value store
- usually consisitent under a sec (eventual consistent)
- cross region replicaitons
- can request a strongly consistent read -ve HA
- BASE over ACID
- but also can do ACID transaction
- priced on throughput and tranactions rather than compute
- autoscalling -> scales up first but scale down slow


**DynamoDB Transaction**
now got overlap with RDS

**Special Values**
collection -> tabls
table -> store items
item -> consists of attributes
attribute -> key value pair
primary key -> hashed to get a unique key (must be unique)

index -> sort key  + partition key
sort key -> stored in this sorted order (used for indexing)
partition key -> has be unique

Secondary Indexes
global - partition and sort key can be different
local - partition key must be the same

Limits 
- 5 local and 5 global secondary indexes
- max 20 attributes
- indexes take up storage space

why bother with keys
- speed up lookup

global - fast query of attributes outside the pk
local - 

secondary indexs -  like a view from rds

dont really understand...

### RedShift
- fully managed clustered peta byte scale data warehouse
- postgress SQL compatible
- can work with SQL tools

**RedShift Spectrum** allows you to work with data files directly from S3

**Data Lake** large collection of data. 

Redshift is the framework to make sense of large set of data using SQL to query it.

### Neptune
Fully managed Graph Databases
relationships between objects
vertices and edges

dont think its tested

### ElastiCache
Fully managed in memory data stores
redis and memached
rally really fast 
not persitence (by and large)
priced on node size and hours of usage

use case 
- store session data for scability
- leaderboards
- landing spot for dashboards


Redis|Memcached
-|-
Complex|Simple
cant scale horizontally|can scale horizontally
can do read replicas and snapshot | cant do anything
single threaded| multi threaded by deafult

auto discovery...

### Aurora
automated failover to replicas


## Comparing Databases

EC2
- ultimate control
- when prefered db no on RDS
RDS
- need a traditional db for LTP
- well formed data
DynamoDB
- name value pair
- in memory performace with persistence
RedShift
- massive amounts of daa
- primary for OLAP workloads
Neptune
- graph database to model relationships between objects
Elastic Cache
- Fast temp storage for small amounts of data
- highly volatile data

read whitepaper AWS Storage Options and take note of anti patterns

## Pro Tips

Cheaper to run DB on EC2
but must consider intagibles

performace hit when RDS backups run in a single AZ deployment
youll want to backup off a standby or a replica

## Challenge 1
DBAC 
ans:DACB
## Challenge 2


## Additional Resources

https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf

https://d1.awsstatic.com/whitepapers/Multi_Tenant_SaaS_Storage_Strategies.pdf

https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf

https://www.youtube.com/watch?v=SUWqDOnXeDw

https://www.youtube.com/watch?v=TJxC-B9Q9tQ

https://www.youtube.com/watch?v=9wgaV70FeaM