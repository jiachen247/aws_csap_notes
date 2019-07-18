# Migration and Hybrid Architectures

## Data Pipeline
severless based workflow product used for large scale migration of data

Pipeline
- a set of tasks in order

Used to move super alot of data around for backup or whatever
- can integate with EMR
- can be run on create or on a recurring schedule

vs Step function
- for smaller workloads

## Disaster Recovery
1. Backup and Restore - hours/days
2. Pilot Light - hours
3. Warm Standby - min
4. Multi Site - near real time

Considerations
- cost
- RTO RPO

Pilot Light
- minimal standby


## AWS Snow
can migrate in or out

1. AWS Snowball
2. AWS Snowball Edge
3. AWS Snowmobile

considerations
- size
- compute

Size
- useful for anything more than 10TB

GUIDELINE => 10TB!!!!
anything smaller use storage gateway

Snowball
- a simple storage device
- use the snowball client
- create a job via console
- encrypted at rest and in flight via kms

Snowball Edge
- snowball with compute
- used if you need local computing
- comes as storaged optimied, compute optimized or compute optimized with GPU

Snowmobile
- same usecase as the snowball
- contact sales team
- large quanties of data
- 10PB of data or more
- used for migrating data centers
- up to 100PB 
- can have multiple


< 10 TB => use storage gateway
> 10 TB => snowball (edge)
> 10 PB => snow mobile

