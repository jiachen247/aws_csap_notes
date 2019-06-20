# AWS Essentials

## Accounts
what an account provides
- isolated authentication
- isolated authorization
- separate billing

Isolated Blast Radius
- if the account is compromised
- the blast radius account is only within this account

**Account Root User**
- Only principal that start with permission to manage users
- can create users (by default no access)
- has root accounts

**Principal**
an entity 

**Identity and Access Management Service**
- identity and permission store

## Regions, AZ and Edge Location
why are they important

Regions - HA
AZ - FT - Fault isolation domains / localize errors
Edge Location - smaller distrubted in cities. Storage and distribute content

## HA vs FT vs DR

High Availability
- recovering from failure
- the ability to recover from a failure
- extra backup wheel

Fault Tolerance
- operate through failure
- multiple redundancies
- how a jet has 4 engines when it only requires 3
- can survive faults
- a higher standard than HA
- can work through failure
- more expensive

Disaster Recovery
- designed to work when HA and FT fails

## DR RPO/RTO

**Recovery Point Objective** amount of data lost in terms of time. usually time from the last backup

**Recovery Time Objective** time taken to recovery from a failure

## Data Persistence
1. Ephermeral
2. Transient
3. Persistent

Ephermeral
- temporary
- gone when powered down
- instance stores
- directly attached to the fast 
- fastest IO in AWS
- 100000 IOPS

Transient
- SQS & Firehose
- meant for temp storage
- should not be used to store something permanent

Persistent
- EBS EFS RDS S3

## OSI Layer Networking Model
layers of abstractions

Layer 1: Physical - binary
Layer 2: Data link - mac addr
Layer 3: Network - ip addr
Layer 4: Transport - TCP/UDP
Layer 5: Session - port 
Layer 6: Presentation - encryption 
Layer 7: Application - http

each layer adds additional functionalities