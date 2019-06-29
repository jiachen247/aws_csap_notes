# Networking
Virtual Private Cloud
- private networking
- one default vpc
- gotta create as a default 
- beanstalk and lightsail uses the default vpc

cidr range
- one primary (set up creation)
- multiple secondary
- ipv6 cidr block
- tenancy - default and dedicated
- dhcp avaliab;e

Subnets
- belong to a vpx
- specific AZ
- subset of the cidr range

cidr range = buffer + app tier * AZs
usable cidr range = range - 5

can share vpcs cross account via resource share
-   reduced flexibility in config
-   you still pay
-   cannot change vpc level config
-   Amazon Aurora Serverless
-   AWS CloudHSM
-   AWS Glue
-   Amazon EMR
-   Network Load Balancer
-   with other accounts in your org*
-   cannot share default vpc

1a != 1a in another account
use az id instead

owner and participant


**VPC Routing**
- Router
- NACL


## Router
- every vpc has a router
- virtual
- network + 1 addr
- used as the default gateway
- route tables
- custom or main route table
- local route directs trafic within the vpc
- proccessed via prefixed
- higher prefix -> higher priority
- only one route table per subnet

## NACLS
- subnet level
- function like a firewall
- **stateless** network filtering
- default subnet acl
- allow all by traffic inbound and outbound
- SG over acls
- implicit deny
- proccessed from lowest to highest
- stops processing once it finds a match
- can explicitly deny
- works for internal services as well
- cannot reference aws resources
- only works with ranges of ip addresses
- network level
- EXPLICITLY DENY TRAFFIC
- some services cant use SGs

## Security Groups
- network interface level
- applies to the network interfaces
- vpc comes with a default SG
- inbound and outbound
- STATEFUL!!!!!!!!!!!!!
- cannot explicitly deny traffic
- implicit deny rule
- all rules are evaluated
- can reference other AWS logical resources
- by default allows all traffic within the same SG
- can apply multiple SGs to a given interface

## Gateway Types
1. NAT Gateway
2. Internet Gateway
3. Bastion Host
4. Egress Only

Internet Gateway
- private address to a single public ip
- a type of nat
- swaps private for public
- resources are only given a private IP
- conversion is done on the IG

NAT Gateway
- requires a elastic IP Address (static)
- IP address are tied to regions
- not HA
- can have multiple NATs in different AZ for HA

Egress Only
- used for one way traffic for IPV6 address
- similar to nat for IPV4


DNS in a VPC
- network +2 address
- not allowed outside VPC
- dns resolve to the private address when pinged internally
- internal hosted zone

## Advance VPC Networking

### VPC Endpoints
- access to public zone services without internet access
- only want access to public zone services
- can be pointed to in the route table
- regional service

Type of Interfaces
- Gateway Endpoints
- Interface Endpoints

**Gateway Endpoint**
- route base
- secured using resource policy
- eg S3 bucket policy - allow only from a specific vpc
- only supported for S3 and DynamoDB

**Interface Endpoint**
- no entries on the routing table
- physical enterties in the vpc
- specific to AZ
- uses Private Link
- secured using SGs
- routed on the dns level
- resolves public endpoints to private ones
- supports all other public zone services

## VPC Peering
- the problem -> connecting vpcs
- make sure no overlap in cidr block
- IMPORTANT
- vpc peer id
- works for cross region and cross account
- not transitive
- mesh style
- considering using transit gateway instead for multiple vpcs intercnnectivy
- enable private dns to route over private address
- fully encrypted over aws backbone

Steps
- Create Peer - Invite/Accept
- Add Route to table. Both ends
- If still dosent work check SGs and NACLs

Same Region
- no overlaping cidr
- not transitive
- can reference SGs
- IPV6 supported
- private dns feature

Diff Region
- IPV6 not support
- SG logical ref does not work
- does not extend to VPN and direct connect
- cant use network load balancers and EFS

## Connecting On Prem to AWS

### Site to Site VPN
- over ipv4
- supports static and dynamic
- try to go for dynamic
- private asn range 64512 65534
- create a private virtual gateway
- linked to a single VPC
- routable through vpc route tables
- use a pre shared key
- propogated route - 
- EASY TO SET UP / IN MINUTES
- CHEAP / per hour cost + data charge
- use for sperodic
- end to end encrypted
- compute overhead
- no HA by default

Multiple tunnels for HA

Full HA
- two customer gateway to two tunnel endpoints
- must use BGP

**Route Propegation**
Top priioty
1. Local Route
2. Longest Prefix
3. Static Routes
4. Direct Connect
5. Static VPN
6. Dynamic BGP routes

## Direct Connect
- NOT ENCRYPTED
- can use vpn over DC

**Pricing**
- per hour for the dx port
- data transfer

**Speed**
- < 1 Gb use a direct connect partner
- < 10 Gb
- Single Mode Fiber
- support BGP
- BGP MD5
- 802.1q
- LOA CFA

Usecase
- takes time to setup
- cheaper for transfering large amounts fo data
- better and more consistent latency


Public vs Private Interfaces
- name
- vlan 1 to 494
- private or public

Virtual Interface (VIF)

public vs private VIF

public VIF: public zone services in all regions / over the aws private backbone
private VIF: VPG - a single vpc. one to one connection. only operate in the same region.

**Direct Connect Gateway**
- global service
- used to connect to multiple regions
- only maintain one private VIF
- does the advertising
- still ot transitive

## AWS Transit Gateway
- Gateway Object
- transitive vs vpc attachments
- Full Hub and spoke architecture
- compatible with RAM
- can link different resources in different accounts
- multiple routing tables
- super useful
- Works like a router with multi acct support
- Supports VPC, VPN 
- and DC for certain regions
- enable support for each AZ
- Single Region only
- HA by default
- cannot do logical referencing
- must use cidr ranges
- 1.25 Gb
- full IPV6 support
