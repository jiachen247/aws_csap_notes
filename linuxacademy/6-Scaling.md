# Scaling and Resilience

## IAM
- global

## EC2/EBS
- regional service
- ebs is replicated within the AZ
- take regular snapshots which are saved regionally via S3

## S3
- regional level
- across 3 or more AZ
- unless you use one zone as the storage class
  
## Route 53
- edge locations
- should withstand AZ failutre

## ELB
- region level
- best practice is to have an ELB at a AZ level
- each AZ should have their own AZ

## VPC
- regional service
- spans the entire region
- subnets cant span AZs

## NAT Gateway
- Hardware level resilience
- if AZ fails NAT Gateway fails

## Auto Scaling Group
- regional basis
- can do failover

## VPN
- VPG are attached to a VPC
- multiple endpoints setup
- can tolorate AZ failure

# Scaling Method 1: Stateless Architectures
- enables horizontal scaling

## On Demand vs Reserved vs Spot

on demand - default
reserved - consistent base load / cost saving / need to guarantee uptime
spot - can tolerate interruption / disruption / tolerate failure of a single node

on demand capacity reservation - reserve capacity without a full commitment
scheduled reserved - define start time and end time rather than months and years


spot fleet - spot instance pool. pool is a set of ec2 instance types

## Auto Scaling Groups
- a level of HA and scale (in and out)
- elasticity

AMI Baking
- create an ami 
- used to clone instances
- less flexible
- speedy

Bootstapping vis UserData
- execute setup commands via userdata
- more flexible
- less speed
  

Auto Scaling Group vs Launch Config
ASG - where and when
LC - what to deploy (type - spot or on demand) IMMUTABLE -> but can create new ones
LC are obsolete

use Launch Template instead
- supports all LC features
- + additional features
- not immutable
- supports versioning
- prefer templates to lcs

ASG
- can choose to choose the latest LTs
- controls when and where
- can choose multiple subnets in a vpc
- can integrate with ELB
- built in health checks
- health checks grace period -> 300s
- increase if you got complex bootstrapping
- notification is supported via cloudwatch or sns
- desired, min, max
- it does subnet balancing for HA
- can suspend/protect instacnes
- can set instance to standby
- can detach instances

Scaling Policies
1. Step Policy
   1. based of metrics
   2. steps taken proportional to the load increase
2. Simple Scaling
   1. similar to step without step

**Cooldown**
use cooldown time before more steps can be taken

## Multi AZ 
- how to select how many instance to provision in a given AZ
- over provision if you really cannot handle failure

## Elastic Load Balancer
- Classic LB (CLB)
- Application LB (ALB)
- Network LB (NLB
  
distribute traffic to backend instances or services

**Integration with ASG**
- can integrate it nicely with ASG

**Cross Zone Load Balancing**
enabled by default
cross zone spread


### Classic Load Balance
- Old
- its old dont use it
- just dont
- only if youre using ec2 classic network
- only has round robin strategy
- does layer 4 and layer 7
- supports TCP HTTP HTTPS SSL
- they dont understand layer 7
- do not assign public IP
- always use A record

SSL Offload/Termination
- a big plus

Healthchecks
- can sync ASG and ELB health check

### Application Load Balancer
- layer 7 balancer
- IPV4 and/or IPV6 support
- dual support
- its a CLB++ 
- lambda support
- EKS and ECS support
-  http2 servers / http2 termination
- websocket support
- sticky server
- WAF support
- higher performance

**Target Groups**

**Rules by host or path**
- can route differently based on url path
- can route by fqdn as well via routing rules
- can host multiple sites with many ssl certs
- ALB supports SNI with multiple certs

### Network Load Balancer
- supports static IP address to front
- the fastest
- accept TCP connections
- layer 4
- end to end encryption - use case

https://aws.amazon.com/elasticloadbalancing/features/#compare

## CloudFront
- distribution is central
- Types - 
  - WEB / used this for everything else
  - RTMP (only when using adobe flash media) need to store the files on s3
- waf integration avaliable
- cname given
- 

Origin 
- where the content comes from

Origin Fetch
- origin to edge server
- sensible defaults

Viewer Protocol
- how do users connect
- http/https/get/post
- has option to encrypted at the edge location

SNI is now supported
- but if you need to support old browses then use dedicated cloudfront ip addr

Behaviour
- can control access via path
- restrict access is a per behaviour setting

Regional Cache
- if miss it checks this cache first
- then only does it do a full origin fetch

Invalidation
- can manually invalidate
- can take a while
- nothing is really immediate in CF
- up to 45 minutes

## Working with distributions
- can configure multilple origins
- matched using behaviours
- can do ssl termination at this level also

Security Policy
- TLSv1.1_2016
- recommended
  
Should support HTTP/2 support 

RTMP
- ONLY WHEN YOU USE ADOBE FLASH MEDIA PROTOCOL
- it is limited

Distrubtion type cannot be changed late on
Always try to stage changes early
- eg associate waf even if empty first
- distribution takes time

### Custom Origins
- custom headers
- can be used to track which edges
- can change specifiy SSL protocols
- for normal origins protocol is infered
- can point to on premise 
- on prem have to be public addressing

### CloudFront Security
- comes out quite often during the exam
- must use public trusted cert on the viewer side
- if youre using ec2 as your origin you need a public trusted cert
- does not by default restrict access your origin directly - use OAI to do this

**Edge Filtering**
- malformed url service
- layer 7 ddos attack
- can use waf webacl at the edge
- filter out bad actors before they hit the edge

**Origin Access Identity**
- can associate with a CF distribution
- then use a bucket policiy to ensure all traffic goes through CF
- distributions can share OAIs
- very important

Signed URLs and Cookies
- follows S3 model - gets to assume the role and permission of the user who generated
- behaviour level

Signed URLS vs Cookies
- RTMP only allows SignedURL
- are single entities used signed urls
- if reference multiple resources than use Cookies

Build in Geo Restriction
- distribution level
- whitelist or blacklist
- only by IP address

If you want any fancy restriction use compute or 3rd part to generate signed url on the fly

Field Level Encryption
- encrypt decrypt at the edge
- stored encrypt at the edge 
- decrypt on the user side

### Optimizing Caching on CF
- aim for HITS
- 24 hours ttl be default
- can use header to specify ttl
- query string not forwarded by default

Min max TTL
- can be adjusted using headers
- this configure the limits the headers can set the ttl to be

forwarding query string
- forward all and cache
- forward all and whitelist based on a certain query

forward cookies
- all
- or whitelist
- can use headers

Lambda@Edge
- per behaviour level
- functions have to already exist
- can invoke on the edge
- just know that it exist
- must be in the same region
- can trigger on 
  - viewer request
  - origin request
  - origin response
  - viewer response
- use cases
  - inspect cookies
  - for ab testing
  - quality image

Logging Reporting and Monitoring
- hit miss rates under cf
- http status codes
- can log to s3

## Route 53
- can register domain
- or point your ns to the r53 hosted zone
- domain hosting
- point nameservers to aws hosted zone when importing a domain
- can have an internal internal hosted zone just for a VPC

healthcheck
- r53 can do health checks also
- and do automatic failover
- this is done on the record level
- A record as primary or secondary

Alias
- can reference logical aws resource
- cannot use cname on an apex

Resilliance
- does not check individual instances behind ELBs

Inbound and Expound Endpoint
- used for forwarding dns queries
- with on prem systems
- https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-inbound-queries.html

Advanced Routing

**Routing Policy**
1. simple - unique record set with multiple values via round robin
2. failover - private and secondary
3. geo routing
4. geo proximity
5. latency based routing
6. weighted routing

can mix and match policy
- can version control

## Lifecycle Management
- Storage class / tier
- can move or convert between tiers
- determines cost and durability
- and first byte latency
- 11 nines durability - all classes


**CLASSES**

1. Standard
   1. 99.99 availability
   2. 99.9
   3. use as a default
   4. replicated across at least 3 AZs

2. Infrequent Access (IA)
   1. still same replication
   2. same first byte latency
   3. 30 day minimum storage
   4. cheaper access rates
   5. 128k min storage fee

3. One Zone Infrequent IA
   1. no cross az replication
   2. same limits for IA
   3. 99.5 object availability

4. Reduced Redundancy
   1. one zone

5. Glacier
   1. first byte latency - minutes to hours
   2. need to request a restore to S3
   3. 3+ AZ replication
   4. 90 day minimum
   5. 40 kb minimum size

6. Glacier Deep Archive
   1. hours retrieval
   2. tape backup
   
7. Intelligent Tiering
   1. per montly object monitering fee
   2. AWS handles the moving and tiering
   3. you pay a standard fee
   4. no retrival fee
   5. 30 day minimum
   6. per object fees


## Lifecycle rules
- useful
- becaureful some are oneway directions

## Object Locking and Versioning

Versioning
- every object is unique
- current version
- can specify a version id
- previous versions are stored in one zone IA


Deletion does not actly delete
- delete object deletes the reference returns 404 if get without version id
- an still get all the versions
- adds a delete marker

Object Locking
- requires versioing
- for compliance
- retention period - prevents change or delete 
- legal holds - dont have an expiration note
- can only enable for new buckets
- no support for cross region replication with object locks

## Security
- private by default
- only object owner or bucket owner by default

If cross account
- use iam role
- or resource policy/ bucket policy
  - can use this for un auth user / public user
  - can base cond on tags
  - can base cond file path 

Access Control List
- legacy control
- can use to grant public access
- simple permission set
- can be applied to an object or an bucket
- useful for impl object level acl

Pre Signed URL
- the link carries your role
- the user you give it too inherits your access to the bucket or object
- can create pre signed urls to an object that does not exist

## Cross Region Replication
- from src bucket to dest bucket in another region
- effects only take place when CRR is turned on
- requires versionings
- can replicated encrypted via kms
- cant replicate a bucket encrypted with CMK
- can replciate based on prefix and tags
- same ownership
  - can change owner to you
- iam role to perform the replication
  - can be done automatically
- lifecycle events are not replicated
- one way ot retoractive only

## Object Encryption
- client side encrypted
- server side encryption
- sdks can do client side encryption
- objects are encrypted not buckets

**SSE**
- customer provided key (SSE-C)
  - does not support cross region replication
  - can work with on prem HSM
- SSE-S3
  - envelop encryption with S3 master key
  - fully managed
- SSE-KMS
  - envelop encryption with KMS cmk
  - good for role splitting
  - add auditing and logging
  - rotate keys with kms
  - must specifically allow when CRR is enabled
  - aws will auto create a key for each service if you dont specify
  - can set bucket defaults
  - use bucket policy to enforce encryption on a bucket level 

## Optimizing S3
1. upload type
2. tranfer acceleration
3. naming

**Upload Type**
- single part upload by default
- single upload up to 5GB
- single data stream
  - slower
  - if anything fails must restart everything
- multipart via uploadid
  - faster
  - if anything fails just have to retry that part
  - just use this if more than 500mb
  - up to 5TB
  - 10,000 parts limit

Transfer Acceleration
- can enable in the console
- transfer to cf edge location than go over the aws backbone
- alot faster


Object Naming
- can partition bucket for better permformance
  - higher limits since limits are per partition
  - partition is done via prefixes
  - 3500 puts and 5500 reads TPS
  - if you need more than this than consider a good partition
  - try to achieve a spread of prefixes

## Glacier
- values -> bucket
- archive -> objects
- Storage class as well as its isolated product
- one of the cheapest storage
- first byte latency can be hours
- regional service
- up to a thousand vaults per account
- immutable -description
- each vault can have an unlimited number of archives
- async by nature
- even an inventory takes time
- inverntory has no metadata fields
- DO NOT HAVE NAME OR METADATA
- IMMUTABLE
- retrival
  - expedited 1 to 5 mins for small archives
  - standard - default - 3 to 5 hours
  - bulk - large quantity 5 to 12 hours but cost the least
- request specific portions of the archive*** impt range of bytes
- vault lock - 
- no real advantage to using glacier directly but prefer to use with S3

## EFS
File system based storage

- NFS 4 and 4.1
- single AZ
- raw block storage device
- sharing is caring
- highly perfomance file sharing
- shared network file system
- can mount into fs like ebs
- VPC level entity
- best practice
- create a mount taget in each AZ in a subnet
- can have a SG
- does support encryption at rest or in flight
  - just like S3 options 
  - encryption in flight
- amazon utils - aws efs utils
  - sudo yum install amazon-efs-utils
- mount targets are not HA by default but the actual nfs is HA by default
- posix compliant file level permission

**Backups**
- can use aws backup service
- and data sync with other sources

Type
- General Purpose
- Max IOPS - but additional latency - good for parallel lworkloads
- cannot change once efs is created

Throughput mode
- Busting - use this by default
- Provisioned
- can change afterwards

Full Consistency
- as per linux fs

Replications
- HA by default

use cases
- big data
- medium workloads
- content management 
- expensive
- home dir
- parallel workloads
- shared log storage - depending on specific use case
- can scale and high throughput
- can use to connect to on prem

Anti Patterns
- single machine
- object storage
- any integration with cloudfront
- temp storage

## FSx
- like efs
- but allows you to provision a fully manage 3rd part file system 
- eg SMG shares for windows
- eg Lustre optimized for temp storage for high throughput 
- by default not HA
- can integrate with mircosoft AD
- not supported for SimpleAD
- vpc level product
- single subnet
- encrypted by default

Backup
- can backup into S3 over VSS
- or ms dfs for replication - need namespace servers
