# Storage

## S3
- Simple Storage Service
- object storage
- flat structure
- object -> data + meta data
- store unlimited number of objects
- finite number of buckets
- storage class - levels of optimization
- complex set of permissions - identity / acl / bucket policy
- lifecycle policy
- can use static files and websites - offload content
- can serve as an origin for cloudfront
- used for storage gateway
- global service but stores regionally 
- buckets have to be globally unique
- 100 buckets soft limit
- UP TO 5TB per object
- can subdivide bucket
- names need to conform to dns naming standards
  - no upper case letters
  - no special chars
- CORS
  - cross origin resource sharing
  - must be configured on S3
- no concept of folders
- key value set 
- simple vs complex keys
- version id
  - unique
  - distinguish different objects
- sub resources
  - torrent information?
- one of the cheapest storage options in aws

**Permissions**
1. Identity Policy - user/role level
2. Bucket Policy - bucket level
3. ACL - object level

Public Endpoint over IPv4 or IPV6
- global service
- inventory reporting avaliable
- athena can be useful here
  

Storage Class
- durability
- avability
- cost
  
Pricing
- injesting is free
- transfer out cost
- GB per month charge 
- small price on put and get

Blockers
- by default turn on
- can veto bucket policies
- bucket and account level

## Storage Gateway
File Gateway
Volume Gateway
Tape Gateway

used to extend or migrate on prem and aws

1. File Gateway
- SMB or NFS
- file level
- extend or migrate data into aws
- one to one mapping
- puts to S3
- filename is used as the object key
- you dont pay for data in
- can migrate files over time

2. Volume Gateway
- volumes you can mount
- like ebs
- over iSCSI
- supports cached and stored mode (S3 for backup)

3. Tape Gateway
- fully manage tape backup solution over glacier
- uses a tape protocol only works for exisiting tape 
- iSCSI endpoint
- S3 to glaicer via lifecycle