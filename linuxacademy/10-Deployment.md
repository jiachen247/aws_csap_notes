# Deployment and Operations

## Cloud Watch
- metric - a time ordered set of datapoints
- cloudwatch does some aggreation down
- depending on the publishing rate
- can integrate with IOT

Namespace
- a grouping of metrics
- based on services
- custom namespaces avaliable

Alarms
- reacts to metrics
- can define actions
- use to work on events

Log Group
- a group of logs
- access control 
- set configurations eg retention and monitering

Log Event
- timestamp + message

Log Event > Log Stream > Log Groups > namespace

## Cloud Trail
- turned on by default
- log api events
- 90 day record
- activity against an account
- lifecycle/event history by default 90 days
- can be pushed to S3
- free to use apart from storage

Trail
- can be used to integrate and customize

Event Types

Management Events - include login
Data Events - includes s3 putobject
sperize and Diagnostics
￼
Finalize Submission
Max Grade: 16
Max XP: 1200

sperize and Diagnostics
￼
Finalize Submission
Max Grade: 16
Max XP: 1200

sperize and Diagnostics
￼
Finalize Submission
Max Grade: 16
Max XP: 1200
e encrpytion
sperize and Diagnostics
￼
Finalize Submission
Max Grade: 16
Max XP: 1200


Can integrate with cloudwatch events

## Route 53 Logging
- only for public dns zones
- query logging
- logs to cloudwatch
- since route 53 is a global products logs are centralized
- point is you have access to really detailed route53 logs

## S3 Logging
- 20 to 30 mins
- delivery group


## AWS System Manager
- manage ec2s or VMs
- manages ec2 or vms on premise or in the cloud

SSM Agent
- systems manage agenet
- installed on all mordern AMIs

Inventory
- can gather inventroy data from managed machines


Can use to run actions/documents to manage multiple servers

Userful Feature
- Automations
- Run Command
- Patch Manager

Parameter Store
- share password and config files
- part of SSM
- can use to share keys
- works with KMS
- can be used to distribute keys to multiple lambdas or ecs instances
- public service endpoint
- global service
- an integrate with CF ECS lambdas EC2 CodeBuild and Deploy

## Cloud Formation

**Stacks**
- created by a cf template
- can create, update and teardown stacks
- used to manage the lifecycle of the logical resouces in the stack

Stack Updates
- cf will detect the differnce
- an then create delete or modify the stack
- no interuption, some interuption or replacement

Changesets
- takes less permissions to create changesets
- but more permissions to execute them!!

Template Portability and Reuse
- apply templates anywhere and everywhere
- avoid hard coding things
- use parameter store to store values
- use default values when not set
- use sudo parameters - meta info returned by aws that you can refer to in the template
- use intrinsic functions
- dont give names if you dont have too
- eg s3 bucket names are optional 
- aws will create a globally unique one for you

Stack References and Nested Stacks
- group resources together by lifecycle


Cross Stack References
- allows you to reference resources in another stack
- useful for grouping resources based on lifecycle
- outputs and exports
- use by active to active stacks

Nested Stacks
- reference templates on s3
- create new completely resources

Stack Roles
- permission based on the iam user executing the cfs
- option to use a stack role
- allow junior admins to manage resources

Stack Sets
- deploy into multiple accounts

CloudFormation for DR
- use cf automate DR stategies
- you can use cf to do this!!

Custom Resources
- automation outside aws
- for hybrid infrastructure
- extend CF to on premise
- define a custom based service
- sents data to lambda or sns topic
- event is triggered on create update or delete or whatever reallys

## AWS Elastic Beanstalk
- plateform as a service
- created for developers
- `application` is the base resource for elastic beanstalk
- applications can contiain multiple envs
- env can contain application code
- application code is versioned
- can deploy databases as well
- dosent give you gradular fine grain control

Supported envs: Go, Java SE, Tomcat, .NET, Node.js, PHP, Python, Ruby

envirnment
- worker tier
- web server tier
- endpoint url

Blue green deployment
- can use swap env url for bg deployment
- beanstalk can be used to do this

Deployment types
- immutable
- in place

EB Extentions
- can use EB files to provision other resources
- in yaml or json
- similar to cf scripts

## AWS OpsWorks
- where does OpsWork fit in
- CloudFormation > OpsWorks > Elastic BeanStalk
- full control - simple
- used to only support chef
- now supports pupput enterprise and chef automate
- cookbook - can use custom cookbooks
- ops work specific permissions

Stack
- top level constuct
- prod dev or test

Layer
- application tier
- stack contain layers
- can register an ECS cluster as a layer
- auto healing
- elb at this layer
- impements SGs

Instances
- time based - good for known spikes
- load based - use for safeguard

