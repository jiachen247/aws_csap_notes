# Chapter 3: Networking

Learn the OSI model

AWS disables multicastvand broadcast

Ephermeral Ports - above 1024

source dest check - aws only allows an ec2 to send traffic if it is the source or dest
need to disable this for NAT gateway

physical AZ locations are assign to a user when created
can differ from other
US-EAST-1 != someone elses US-EAST-1

**AWS Managed VPN**
- runs over IPSec
- can be used as a redundant connection to aws 
- dependent on internet connection
- static routes or BGP for dynamic routing

**Direct Connect**
Consistent and predictable
up to 10 GB
on site router to aws outer
work with your networking provider
connect to your aws resources via Virtual Interfaces (VIF)

Secondary connection to reduce SPF
can run an additional IPSec

CloudHub
use the VPG to route between customers

**Software VPN**
eg OpenVPN
run on EC2

**Transit VPC**
a hub where everything meets
allows for other cloud providers

**VPC to VPC**
everything above applies
+ VPC Peering


**VPC Peering**
uses the AWS backbone
not transitive

process - 
server 1 sends a peering request
server 2 accepts the peering request
add routes to both servers

Cross Region peering is allowed

**Private Link**
exposing VPC endpoints over the aws backbone - S3 

Interface Endpoint vs Gateway Enddpoint

Interface Endpoint
- ENI with a private IP
- dns to direct traffic
- API Gateway Cloud Formation ...
- security groups for securing


Gateway Endpoint
- S3 and DynamoDB
- gateway that is a target for a specific route
- prefixes in the route table
- secured using VPC Endpoint Policies

## Internet Gateway
- redundant
- highly avaliable
- no risk or bandwidth

IPv4 andIPv6 support

what does it doe?
1. route target for internet bound traffic
2. NAT for public IPs (not for instances that only has private IP)

Egress Only Gateway
- only outbound is allowed

**NAT Instance**
- ec2 instance with a special AMI
- translate many private IP to a single public IP

does not support IPv6
use egress only

add default route to the private subnet

**NAT Gateway**
fully managed
create multiple NAT gateways in multiple AZs
Cant use a NAT Gateway to do VPC Peering, VPN or Direct Connect

NAT Gateway vs NAT Instance

|Gateway|Instance
-|-
Availability|Highly avaliable within AZ|on your own
Bandwidth|up to 45 Gbps|depends on bandwidth of instance type
maintenance|managed by aws|you
performance|optimized for NAT|AWS AMI
public IP|elastic IPcannot be detached|eip can be detached
security group|cannot be associated with NAT gatway|can use security group
bastion Server|no|yes

## Routing
- every VPC has a implicit route table
- local router for the cidr block
- most specific route wins

Broadcast address not supported

**Border Gateway Protocol**
- allows dynamic routes
- required for direct connect
- optional for VPN
- BGP Community tagging
- TCP 179 + ephemeral ports
- Autonomous System Numbers (ASN) unique endpoint identifier
- higher weights are prefered

can weight your connections

## Advance Networking

**Additional Drivers**
Intel VF Interface - 10Gbps
Elastic Network Adapter - 25 Gbps

**Placement Groups**
put resources close to one another
being intentional about placing our instances

clustered vs spread

clustered - place in a low lat group in a single az. low latency and high through put
finite capacity - launch all upfront

spread - HA, placed on different hardware
7 instances in a group per AZ

A spread placement group can span multiple Availability Zones in the same Region. You can have a maximum of seven running instances per Availability Zone per group.

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html

## Route 53

Policies

1. Simple 
2. Failover
3. Geolocation (by continent)
4. Geoproximity (by distance)
5. Latency
6. Multivalue Answer
7. Weighted

## CloudFront
- CDN
- static to 4k live streaming
- edge location concept
- certificate manager
- supports SNI

custom ssl cert 
- wildcards
- host key verification

Option 1: private EIP on each cloudfront edge
Option 2: SNI checks host headers

can choose the SSL/TLS version

## Elastic Load Balancer (ELB)
layer 7 load balancer

Application LB vs Network LB vs Classic LB

Application LB
path based load balancing

Network Load Balancer
layer 4
based off protocol
fast io

**Sticky Sessions**
LB remembers which server it routed to.

## Additional Resources

https://d0.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf

https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf

https://d1.awsstatic.com/whitepapers/Security/Networking_Security_Whitepaper.pdf

https://www.youtube.com/watch?v=KGKrVO9xlqI

https://www.youtube.com/watch?v=8gc2DgBqo9U

https://www.youtube.com/watch?v=z0FBGIT1Ub4