# Chapter 2: Amazon S3 and Glacier

## Introduction
**S3**: Simple Storage Service
provides secure, durable and highly scalable cloud storage.

## Object Storage vs Block and File Storage

Storage|Block|File|Object
-|-|-|-
level|low level, device level as addressable blocks|high level/OS filesystem level|high level object (data + meta data)
protocols|iSCSI/FibreChannel (FC)|CIFS/NFS|http
examples|SAN|NAS|S3

## S3
###### *s3 > buckets > objects*

object storage (s3)
- independent from physical servers
- accessed over http
- flat file system (no directory structure)
- s3 storage is organized with buckets
- 
### Basics

buckets:
- a container
- flat file system
- no dirs
- top level namespace
- bucket names are unique
- can contain an unlimited number of files
- automatically replicated within different AZs in a region
- is not replicated to other regions by default
- auto-scaling by default

---

Limits: 
- up to 100 buckets per account
- bucket names can have up to 63 lowercase chars number hyphens and periods
- objects can be up to 5TB

---

Regions:
- data never leaves the region by default
- important for security and compliance

objects:
- are contain in buckets
- has a unique key made up of (bucket name, id and version id)
- contains data and metadata
- data is treated as a byte stream
- metadata -> key value pairs
- System metadata and user metadata

object-keys:
-  unique key
-  bucket + object key + optional version id

`http://{bucket}.s3.amazon.com/{key}`
note that slashes are allowed but at the end of the day a bucket is still a flat file system

s3 operations: CRUD + list

durability -> will my data still be there in the future (does not cover user level mistakes)
availability -> can i access my data rn

pricing?

**Data Consistency**

Adopts an eventual consistent model due to replication within the region
data will eventually be consistent

except for puts to new objects ->  which has read after *write consistency*. what this means is that data is propagated to all instances before a 200 is returned. will not hit inconsistent state here.

EC:
put on an existing object (update)
delete an object

all operations are atomic -> never get a mix of old and new data

**Access Control**

access control enabled by default
default is no access
two ways to do access control
1. Access Control Lists
2. IAM

ACLs:
- more coarse grain
- legacy support

IAM:
- fine grain controls
- can use policy to defined allowed operations
- and even allow or disallow base of ip address
- more in chapter 6

**Static Web Site Hosting**
no server side rendering
storage|durability|availability|pricing
-|-|-|-
standard|9.99999999%|9.99%|more expensive
reduced redundancy storage|99.99%|99.99%|cheaper
we should try this out!!
can map dns record to 
`http://{bucket}.s3-website-{region}.amazonaws.com`
  
### Advanced Features

**Prefix and Delimeters**
fake directory access controls vis prefix and delimeters

use case -> user home directory

**Storage Classes**

class|durability|availability|pricing|use case
-|-|-|-|-
standard|9.99999999%|9.99%|expensive|low read frequent access
infrequent access (Standard-IA)|9.99999999%|9.99%|less expensive|long lived + less frequently accessed data
reduced redundancy storage (RRS)|99.99%|99.99%|cheaper|derived data that is reproducible
glacier|-|-|cheapest|no real time access

**Lifecycle Management**
hot -> warm -> cold -> delete

can use lifecycle configuration to move data across different storage classes

created in Standard
moved to Standard-IA after 30 days
moved to Glacier after 90 days
deleted after 3 years

**Encryption**
encrypt anything sensitive when storing on S3

encryption in **flight** and in **rest**

flight using ssl/tls
rest using Server Side Encryption (SSE)

**SSE-S3 (AWS Managed Keys)**
- fully managed 
- just tick a box
  
aws has the decryption key tho i rlly dont know how this makes sense

**SSE using KMS**
fully managed as well using AWS's KMS to handle key management. a little more secure i guess. additional access controls using IAM and logged failed decryption attempts

**SSE using Customer Provided Keys**
AWS promises to delete your keys after encrypting or decrypting. lol i have trust issues.


**Client Side Encryption**
the most secure but the hardest to pull off
if you lose your keys its gg

so basically you store and retrieve encrypted data and make sense of it client side.

**Versioning**
versioning is done on a bucket level
not turned on by default

once enabled it cannot be turned off only suspended

**MFA Delete**
requires an additional otp via another auth method before allowing a delete
prevents accidental deletes

**Pre-Signed URLs**
time limited download link
prevents scraping

**Multipart Upload**
allows large files to be uploaded in parts
should use for files more than 100MB
must use if more than 5GB

this is done automatically if you use the CLI or the SDKs provided. 

funfact: sls uses this to upload lambda code

**Range GETs**
super dangerous to use lol
if you like living on the edge you'll love this

makes it possible to get portions of a file via a range of bytes from S3 or glacier

only use if you know what youre doing

**Cross Region Replication**
a little tricky

- replicate buckets to another region
- replicates the ACLs and metadata as well
- updated for eventual consistency as well
- *versioning must be turned on for both source and destination
- must have IAM permission for s3 to do the replication
- used to move data closer to users -> like a cdn
- or keep a backup faraway
- existing objects will not be replicated only new ones once cross region replication is turned on
- you can copy the existing objects to the new region by another command
 
**Logging**
- logging is turned off by default
- can choose to store logs in a target bucket

what happens when you store a bucket logs in the same bucket?
will you get into recursive hell?

**Event Notification**
can used events to trigger lambdas, add to SQS/SNS or run any other workflows.

eg transcoding a video file that comes in

**Best Practices**

popular use cases
1. backup for on premise data over the internet
2. blob storage that is reference n dynamoDB for fast search and lookup.
3. use random distribution of keys
4. <100 rps 
5. consider cloud front for get intensive use cases

## Glacier
> Amazon Glacier is an **extremely low-cost** storage service that provides durable, secure, and flexible storage for data archiving and online backup. To keep costs low, Amazon Glacier is designed for infrequently accessed data where a retrieval time of **three to five hours** is acceptable.

Glacier > Vaults > Archives

**Archives**
- stored in archives
- are like objects in S3
- up to 40 TB
- archive id based on time of creation
- automatically encrypted
- immutable

**Vaults**
- something like buckets in S3
- up to 1000 vaults per account
- access control implemented via IAM
  
**Vault Locks**
- eg. Write once Read Many (WORM)
- once locked the policy can no longer be changed
  
**Data Retrieval**
- can retrieve up to 5% of data for free each month
- daily prorated
  
**Glacier vs S3**

|Glacier|S3
-|-|-
file size limit|40TB|5TB
naming|system generated|friendly naming
encryption by default|yes|no
price|cheap|expensive
Recovery time objective|3-5 hours|instant

## To Remember

1. S3 bucket names must be globally unique
2. S3 objects have a limit of 5TB
3. S3 bucket have no size limit
4. Once versioning is turned on in a bucket it cannot be turned off only suspended
5. cross region replication can only be done if versioning is turned on for both source and destination
6. existing objects in the bucket will not be replicated
7. logging is turned off in S3 by default
8. Glacier has a limit of 1000 vaults per account
9. Each glacier vault can have unlimited archive
10. Each glacier archive has a limit of 40 TB


## Additional Resources
https://www.youtube.com/watch?v=VC0k-noNwOU

https://www.youtube.com/watch?v=BZACBJ6OvCs