# Security

## Account and Service Security

### KMS
- part of IAM
- manages security keys
- base64 encoded

Customer Master Key (CMK)
- en/decrypt up to 4k
- has a arn
- regional service
- enables role seperation

can be used to generate data encryption key
(DEK)
aws does not store the DEK

KMS knows which CMK was used to create and DEK

Envelop Encryption
- encrypt progressively
- AWS managed keys - rotated once every three years

rencrypt - re associate with new CMK

- FIP140-2 level 2 compliant

### Cloud HSM
- more secure than KMS
- Hardware Security Module
- can do asynmetric
- support PKCS#11, Java Crypto Extensions, Microsoft CryptoNG
- Truly dedicated HSM service
- injected into your VPC
- Not HA by default
- multiple CloudHSMs over multiple AZs
- less integration with AWS Services
- FIP140-2 level3
- cannot give aws a physical server

### AWS Certificate Manager
- create X509 V3 SSL/TLS Certificate
- used for https
- layer 6 - presentation level
- does not support running on EC2
- certs are free
- recommended to be used with route 53
- automatically renewed if associated with a resource
- currently supports cloudfront, api gateway, S3, ELB
- must validate ownership through cname records

### AWS Directory Service
Types v IMPT
Group 1
1. SimpleAD
2. Microsoft AD
3. AD

Integration with:
AWS console, AWS connect quick sight WorkDoc WorkMain Workspaces

Group 2
1. Cognito
2. AWS Cloud Directory

**SIMPLE AD**
- runs samba 4
- manage user groups and computer
- small and medium network
- open source
- Small - 500 users or 2000 objects
- Large - 5000 users or 20000 objects
- subset of MS AD
- does not support trust relationship
- for simple SSO apps
- EG SSO from the aws console
- HA by design

**MS AD**
- VPC level
- auto HA
- can implement domain trust
- support MS workloads
- supports radius MFA
- authentic MS AD
- HA by design

**AD Connector**
- directory proxy
- on prem ms AD
- HA by design

**Cognito**
- identity federation
- user pools
- identity pools
- sync
- used for web and mobile

**AWS Cloud Directory**
- graph based information store
- used to store relationships

## Network Security

### AWS WAF
- Layer 7 firewall
- API Gateway
- CloudFront
- ELB
- Web ACL
- either global or regional
- can blacklist or whitelist ip addrs
- can do ratelimiting here
- specific and target
- used to deny traffic

### AWS Shield
- designed for DDOS
- works super well with Route53 and cloud front
- brings security out to the edge

Standard vs Advanced

**Advanced**
- 3000 a month + data transfer fee
- waf included
- additional protection
- more visbility and reporting
- network level + level 7 
- ddos response team
- cost protection due to ddos
- can protect elastic IPs via nacls at the edge

## AWS Guard Duty
- inject and moniters
- output to cloudwatch or console
- can respond to cloudwatch events
- master guard duty account
- can add memeber account
- cannot be a member and a master
- generate findings
- requires service roles permissions
- trusted IP list for seperate aws accounts
- threats models are global
- almost realtime
