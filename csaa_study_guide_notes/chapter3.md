# Chapter 3: Elastic Cloud Compute (EC2) and Elastic Block Store (EBS)

## EC2

### Basics
#### Instance Types

instance families
https://aws.amazon.com/ec2/instance-types/

compute, memory, gpu and storage optimized

enhanced networking available for better network performance

#### Amazon Machine Images (AMI)
AMI are a snapshop of the OS to boot into.

4 sources
1. published by AWS
2. AWS Marketplace
3. Generate from existing EC2 instances
4. Import from virtual servers

#### Securely Accessing an Instance

addressing an instance
1. public dns name
2. ip addr
3. elastic ip

elastic IP are elastic network interfaces () is a configurable address by AWS to decouple caller and callee

**Initial Access**
is done using key submitted when creating the instance
default security policy is deny all

for windows admin password is accessed through decryption of an encrypted string using the private key associated with the instance


```
// always remember to change the root password
$ sudo su
# passwd root
```

**Virtual Firewall**
set to deny all except those in the security group by default

2 types of security groups
EC2-Class and VPC


EC2 Classic is used for legacy support.

Launching

Bootstrapping
use userdata to config the server on startup can use with chef
only for the first time.

VM Import Export
can export VM back to image for running on premise. Only for instance created not from AMI.

Instance metadata
magic ip accessible through the aws instance
`$ curl http://169.254.169.254/latest/meta-data/`

Managing instances using tags

Monitor using CloudWatch

Modify the instance type and security vpc policy when the instance is still running

Termination protection available

#### Options
##### Pricing
pricing tiers from most ex to cheapest

1. On demand instance
2. Reserve instance
3. Spot instance

On demand
spin an instance up on demand

Reserve pay for 1 or 3 years
Either all upfront, partial upfront or no upfront

switch az and instance family type.
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-modifying.html

##### Tenancy
for compliance, licensing and security concerns

Shared Tenancy
deployed anywhere by aws
shared physical hardware as other users
sandboxed

Dedicated Instance
physical used by a single org
can still have multiple instance that that belongs to the org running on it 

Dedicated Host
one to one mapping between hardware and instance

##### Placement Groups
PGs are a logical group of instances within an a single AZ with low latency 10Gbps speed

#### Instance Store
aka ephemeral storage
is stored tied to the instance
its tmp and gets destroyed on shutdown

used for temp storage.
not reliable

## Elastic Block Store (EBS)
> Amazon EBS provides persistent block-level storage volumes for use with Amazon EC2
instances. 

## Types
### Type 1: Magnetic Volumes
cheapest and the slowest
good for low and sequential read writes
avg 100 iops
1 gb to 1 tb

### Type 2: General-Purpose SSD (gp2)

1 gb to 1 tb
3 iops per gb capped at 10k

charged by the total volume not data saved

### Type 3: Provisioned IOPS SSD (iops)
most ex

4 GB to 16 GB
use case large database workload

this is a little outdated
https://ap-southeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#CreateVolume:

**Amazon EBS-Optimized Instances**
check for this to fully leverage the IOPS power
comes by default on more powerful instance types


**Protecting Data**

> You can back up the data on your Amazon EBS volumes, regardless of volume type, by taking
point-in-time snapshots. Snapshots are incremental backups, which means that only the
blocks on the device that have changed since your most recent snapshot are saved.

point in time with some sort of funky version control

can schedule snapshots

snapshots are stored on S3 and you have to pay for storage there

cant manipulate them as your own buckets
have to 

can manipulate them through he EBS interface

You can create volumes from a snapshot.

**Encryption**
native encryption for all volumes at rest
snapshots of encrypted volumes are themeselves encrypted too.



## Additional Resources
https://www.youtube.com/watch?v=jLVPqoV4YjU