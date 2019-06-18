# Deployment Options

Types of Deployments
1. Big Bang
2. Phased Rollout
3. Parallel Adoption

Rolling Deployment
- new launch config with a new ami
- terminate old ec2 instances

A/B Testing
- using weighted round robin on Route53

Canary Release
- deploy a version two in one instance 


Blue Green Deployment
- play with route53 using route53
- easy rollback option
- or use ELB 
- or use elastic beanstalk to point to new url
- OpsWorks to clone the stack and update DNS

When not to use blud green (Contraindication)
- application is tightly coupled to the data schema
- special upgrade routine or post processing

## CICD

Continuous Integration
- merging back into master frequently testing as we go

Continuous Delivery
- one button deployment

Continuous Deployment
- automatically deploys the entire chain

Consideration
- smaller incremental improvements and features
- strong test automation game
- feature toggle
- gels well with micro services

**CodeCommit**
hosted git repo

 **CodeBuild**
 compile and run test build deployment packages

 **CodeDeploy**
 can deploy deployment packages

 **CodePipeline**
 orchestration tool

 **XRay**
 used for debugging in s serverless and distrubted system

 **CodeStar**
 leverages all the other services and automates

 **Elastic BeanStalk**
- makes deployment dead simple
- deploy docker to PHP and Java
- supports multiple env
- application versioning supported

## CLoudFormation
- IaaC
- Infrastructure as Code
- templates
- stacks - actual resources
- change sets - summary of changes to a template

**Template**
- json or yarm
- only resources section is required
- stack policies (protect resources)
- cannot remove stack policy
- by default denys all

**Best Practices **
- python helper scripts avaliable for ec2 instances
- use cf to make changes to your stack
- make use of changesets to detect errors
- use stack policies to protect resources
- use a vcs to record changes

## Elastic Container Service
ECS and EKS

**ECS**
- build by amazon
- simpler
- leverage aws integrations
- containers are called Tasks
- isolated
- limited extensibility

**EKS**
- pure k8s
- feature rich
- containers called pods
- shared access to each other
- k8s addons

ECS Launch Types
1. EC2 Launch Type
2. FarGate

EC2 Launch Type
- explicitly provision EC2 instances
- responsible for maintaining the pool
- responsible for cluster optimization
- more granular control

Amazon Fargate
- automatically provision resources
- handles cluster optimization
- limited control

## API Gateway
- edge optimized
- supports private availability
- SNI is supported
- does caching

## Management Tools

### AWS Config
- audit and access configuration
- helps with itil
- creates a baseline
- and tracks changes over time
- can set rules to check for complience
- eg is backup enabled on RDS?

### OpsWorks
integrate with chef and puppet
from upgrades to deployment

1. Chef Automate
2. Puppet Enterprise
3. OpsWorks Stacks (compatible with chef recipies) - can be cloned but only within the same region

### Systems Manager
centralized console and toolset for a wide variety of system management tasks

- SSM Agent 
- installed on morden AMI

1. Inventory Service
   1. determine which instances have old versions of software
2. State Manager
   1. creates and manage states
3. Logging
   1. aws recomends using CloudWatch instead
4. Parammeter Store
   1. integration with KMS
   2. eg rds credentials
5. Insights Dashboard
   1. account level view of CT config and TA
6. Resource Groups
   1. another way to get to the resouce groups page
   2. organize through tagging
7. Maintenance Window
   1. define matainince windows
8. Automtation
   1. run scripts
   2. run commands without ssh/rds
   3. on multiple machines concurrently
9. Patch Manager
   1.  automated patch
   2.  apply during a matainence windows
   3.  baseline for patches
   4.  aws have predefined
   5.  7 day timer

**Documents**
- ssm documents
- json or yaml
- steps and params
- stored as version

1. Command Document
2. Policy Document
3. Automation Document


## Additional Resources

https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf

https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf

https://d1.awsstatic.com/whitepapers/overview-of-deployment-options-on-aws.pdf

https://www.youtube.com/watch?v=01hy48R9Kr8

https://www.youtube.com/watch?v=Qik9LBktjgs

https://www.youtube.com/watch?v=GEPJ7Lo346A


  


