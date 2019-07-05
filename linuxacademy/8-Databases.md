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


