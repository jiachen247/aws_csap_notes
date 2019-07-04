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