# Migrations
Best practices for taking on premise servers to the cloud


Strategirs
1. Rehost



## Strat 1: Re Host
just move all on premise servers to the cloud
eg move mysql instance to mysql instance running on the cloud

## Strat 2: Re Platform
similar to s1
but leverage RDS instead

## Strat 3: Re Purchase
Drop and Shop
eg on premise crm to salesforce

## Strat 4: Rearchitect
redseign in a cloud native manner
traditional backend to lambda

## Strat 5: Retire
get ride of lagacy applications

## Strat 6: Do Nothing
does what it says

**TOGAF**
The open group architectural framework
defacto standard of enterprise architecture

Cloud Adoption Framework
1. Business
2. People
3. Governance
4. Platform
5. Security
6. Operations

## Hybrid Architectures
- Both on premise and cloud resources
- first step in a migration
- VMWare


eg
- S3 with storage gateway
- SQS with middleware
- VMWare vCenter plugin


## Migration Tools

1. Snowball Service
2. Storage Gateway
3. Server Migration Service
4. Data Migration Service
5. Schema Conversion Tool
6. Application Discovery Service


## Server Migration Servic
- automates migration of VMsto AWS
- replicatesVMs to AWS syncing volumesand creating perodic AMIs
- Linux and Windows VMs

## Schema conversion tool (SCT)
can convert schemas to other databases
used for more complex usecases
eg data warehouses

## Database Migration Service
smaller and simpler conversions
supports mongo and dynamoDB
replication function for on prem to AWS

## Application Discover Service
- discovers all the services running
- can help in predicting cost for running the same setup in the cloud
- runs agentless in an vmware env via discovery connector
- or agent based in a non vmware env
- supports only linux and windows

**Migration Hub**
consolidated tooling


## Network Migrations

cutovers
- check for no overlaps in IP addressing
-  5 reserved IPs
- /16 to /28 cidr block
- no broadcast address

VPN is good first step
then Direct Connect with VPN as a redundency

VPN to DC is easy
- enable BGP routing or static route
- and weight DC higher
- AWS always prefers DC over VPN route

## Snow Family
- Evolution of the AWS Import/Export process
- as fast as youre willing to pay
- encrypted at rest and transit


Option 1: **AWS Import/Export**
- ship a harddisk to aws and they load it to S3

Option 2: **Snowball**
- aws ships it to you 
- up to 80TB
- send snowball back to aws
- aws copies data over to S3

Option 3: **Snowball Edge**
- like a snowball
- but with compute power like lambda and clustering
- portable AWS
- process data in a really remote location

Option 3: Snowmobile
- container truck
- up to 100PB of data


Tech is a minor aspect when planning a migration to the cloud

## Additional Resources

https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf

https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf

https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf

https://d1.awsstatic.com/whitepapers/AWS-Cloud-Transformation-Maturity-Model.pdf

https://www.youtube.com/watch?v=Y33TviLMBFY

https://www.youtube.com/watch?v=9wgaV70FeaM