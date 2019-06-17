# Business Continuity
and disaster recovery

> Everything fails all the time therefore we need to architect for everything to fail. - Werner Vogels

**Business Continuity**
seeks to minimize business activity disruption when something fails

**Disaster Recovery**
the act of responding and recovering from an event that threatens business continuity


Fault Tolerance vs High Availability
- similar
- FT is concern with not allowing users o know that a fault happened
- HA leaves some room for downtime

Service Level Agreement (SLA)
- can claim service credits if disrupted


**Recovery Time Objective (RTO)**
time it takes to get back to usual

**Recovery Point Objective (RPO)**
acceptable amount of data loss measured in time

**Business Continuity Plan**
- defines RTP and RPO
- defines HA investment

**Categories of Disasters**
1. Hardware Failure
2. Deployment Failure
3. Load Induced
4. Data Induced
5. Credential Expiration
6. Dependency
7. Infrastructure
8. Identifier Exhaustion
9. Human Error

**DR Architectures**
1. Basic Backup and Restore - offsite backup
2. Pilot Light - minimal standby instances on aws. manual intervention
3. Warm Standby - standby already running.
4. Multi Site - auto fail over
   
## Storage Volumes
### EBS
- 0.2% failure rate
- single az replication
- snapshot - multiAZ

**RAID**
RAID0 - no redundency / aka striping / good read and writes

RAID1 - mirroring / complete replication / duplicate / slower on read and writes

RAID5 - 3 drive min, one can fail. capcity is n-1 drives

RAID 6 - 4 drives min 2 can fail. n -2 capacity. writes suffer.

AWS dosent recomend RADI 5 and 6 due to EBS high IOPS for house keeping

RAID0 - striping can impove throughput

### S3
- eleven 9s of durability
- standard - multizone avability
- single zone has sinle az durability

### EFS
implementation of NFS
multi AZ redundency

### Storage Gateway 
for offsite backup

## Compute Options
- ALWAYS KEEP AMIs UP TO DATE
- prefer horizontal scaling
- route53 to do health checks and failover


## Database considerations for HA
- DynamoDB has built in HA
- prefer Dynamo db for HA
- if need RDS use Aurora
- If cannot Auror then multi AZ RDS
- use frequent snapshots from the standby or read replication
- if running on EC2 youre on your own
  

### RedShift
- no multi AZ deployments
- best to just use multi node cluster and use replication

### ElasticCache

Memcached
- no replication
- use multiple nodes in each shard
- launch multiple nodes across availiable AZs

Redis
- use multi shards per node
- multi AZ replications
- backups of redis cluster

## Networking
- subnets in multiple AZs
- redundant connection in VPG
- DirectConnect is not HA by default
- Route53 can do failover
- use elastic IP without dealing with dns
- spin up a nat gateway in each AZ with routes for private subnets to use the local gateway

**Additional Resources**
https://d1.awsstatic.com/whitepapers/Storage/Backup_and_Recovery_Approaches_Using_AWS.pdf

https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf

https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf

https://www.youtube.com/watch?v=xc_PZ5OPXcc

https://www.youtube.com/watch?v=RMrfzR4zyM4

https://www.youtube.com/watch?v=a7EMou07hRc