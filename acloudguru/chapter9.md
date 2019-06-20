# Cost Management

Types of expense
1. Capital Expense
2. Operation Expense

One of the benifits of cloud computing is Operation expense


**Total Cost of Ownership**
- A comprehensive look at the entire cost model of a given decision

**Return on Investment**
- amount and org can expect to recieve back
- may not always be positive

be skeptical of TCO and ROI

Cost Optimization Strategies

Strategy 1: Appropriate Provisioning
- turn off not needed instances
- consolidate where possible
- eg one big dynamo db database
- cloudwatch can help with this
- scale in if unused

Strategy 2: Right Sizing
- use the lowest cost resource that meets the specs
- use sqs to decouple

Strat 3: Purchase Options
- used reserved instaces for permanent
- spot instances are best for temp horizontal scaling
- EC2 Fleet target mix of on demand reserved and spot instances

Strat 4: Geographic Selection
pricing can vary from region to region
sa-east-1 is rlly ex

Strat 5: Managed Services
use managed services whenever possible
rds over mysql on ec2 
RDS Redshirt fargate EMR
lower complexity and cost

Strat 6: Optimized Data Transfer
- transfer in is free
- transfer out has costs

use direct connect might be more cost effective in the long run

## Tagging
- the number one best thing to manage aws assets
- metadata
- arbitary key value names
- can be used for cost allocation security automation and many more
- can enfore tagging standaization with AWS config

## Resoure Groups
- makes use of tags
- to group resources together

**Reserved Instances**
- only garduring DR
- significant discount
- automatic when launching instances of the same type
- can be shared across multiple accounts with consolidated billing

types
1. Standard
2. Convertible (can change instance family, OS, tenancy, payment option)
3. Scheduled

can be sold on a secondary marketplace

40-60% discount

RI Attributes
- Instance ype
- Platform
- Tenancy
- Availability Zone (only apply to the az)

Other resources in the same family.
instace size flexibilty only available for linux regional RI
m4.2xlarge = m4.xlarge times 2

**Spot Instances**
1. create a spot request
2. specify AMI, VPC , instance type
3. define a highest price

types 
- one off
- maintain (can set to hibernate)
- duration-based (based on run time)

Consideration
- cost depends on az and instance type
- look at the pricing history of instace and az

**Dedicate Instance**
- shared within your instances
- a little more expensive
- still a vm

**Dedicated Host**
- dedicated hardware
- - server bound software 
- dedicated host reservation
- if you need sockets 
- can only run one type of ec2 instance
- but resides on hardware you dont share with anyone


## Cost Management Tools

### AWS Budgets
- set pre defined limits

## Consolidated Biling
- one account to pay for many accounts
- eg S3 tiered pricing
- achieve some economy of scale

## Trusted Advisor
- can check if we are wasting money
- core checks (for anyone)
- more extensive testing avaliable for enterprise plans

## Additional Resources

https://d1.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf

https://d1.awsstatic.com/whitepapers/total-cost-of-operation-benefits-using-aws.pdf

https://d1.awsstatic.com/whitepapers/introduction-to-aws-cloud-economics-final.pdf

https://www.youtube.com/watch?v=CcspJkc7zqg

https://www.youtube.com/watch?v=XQFweGjK_-o

https://www.youtube.com/watch?v=1Z4BfRj2FiU