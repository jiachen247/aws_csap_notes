# Amazon Private Virtual Cloud (VPC)
> The Amazon Virtual Private Cloud (Amazon VPC) is a custom-defined virtual network
within the AWS Cloud.

max 5 vpc per region

default VPC at each region at 172.31.0.0/16

VPCs will in one region but can span AZs
subnets within VPCs are contained within on AZ.
however VPCs can have multiple subnet spanning different AZs within the same region

default subnet created in every AZ with netmask /20 linked to the default vpc

public and private subnets! no such distinctions but based of the ip routes and security policies

**Route Tables**
nothing new here

all vpcs have an implicit router

**Internet Gateway (IGW)**
the way to the internet
does nat for you

**Elastic IPs**
specific to a region
one to one with instances

**Elastic Network Interfaces**
works like an actual network card

**VPC Endpoints**
VPC allows you to talk to other aws services without going over the internet
such as S3


**Peering**
works by connecting different vpcs under the same account to be connected together.
one to one!
not transitive
cannot peer if cider blocks overlap
cannot peer if different regions
https://aws.amazon.com/about-aws/whats-new/2017/11/announcing-support-for-inter-region-vpc-peering/

**Security Groups**
500 SGs per vpc
50 in 50 out to each SG
up to 5 SGs per ENI/instance (thats like 500 rules)
implicit deny all
stateful!!
evals all rules
cannot deny only allow


**Netowrk ACLs**
not stateful
subnet level
can deny
does not run everyhing
returns first that matches
used to block ip addrs

https://www.quora.com/What-is-the-difference-between-security-groups-and-the-network-access-control-list-in-AWS

NAT Gateway vs NAT instance


nat instance
 amzn-ami-vpc-nat
 ec2 instance

 nat gateway
 fully managed by AWS


**VPN**
VPG (aws side ) and CGW (client side)
to form a vpn connection
vpg is many to one to cgws

2 IPsec tunnels
supports BGP and static routes