# Accounts

services involved:
1. IAM
2. STS 
3. Cognito


## AWS Identity Basics
IAM - Identity Access and Management Service

Building blocks in IAM
- users
- groups
- roles
- policies
- auth attributes

Long Term Access Credentials
- username/password
- access keys

can link ssh keys for code commit
user and roles are real identities
group are an org construct

- Implicit Deny
- Explicit DENY overwrites ALLOWS
- by default no permission granted
- 5000 iam user limit
- use STS to federate identities

## Policies
How this might look in the exam
- debug policies
- write policies
- 
json policy

{
    "Version": "2012-10-17"
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "*",
            "Conditions": {}
        }
    ]
}

Identity vs Resource Policy
Identity Policy - attached to a user or role

Inline Policies
- used for overrides
- tied to a single user group or role
- never best practice to use this

AWS Managed Policy
- admin rights
- best to use if you can
- are in themselves resources
- not tied to any user

Customer Managed Policy
- you manage and define the permissions


Conditions
- adds flexibility
- powerful
- json object
- can contain multiple coditions

IAM variables
${aws:username}
will come out for the exam

Resource Policies
- attached to resources instead of user/groups/roles
- Identity and Resouce policies must agree for any action
- statement contains a mandatory `principal` field. This is assumed in an identity policy
- a quick way to see if its a principal or resource policy
- can be used to apply a policy to many users

**This is so impt**
^
Explicit Deny
Explicit Allow
Implicit Deny

**IAM Roles**
- roles vs users
- a roles isnt a user
- you cannot login with a role
- dosent have a username or password
- a temp identity
- a role in a functional 
- token and expiration
- can refresh token
- keys are dynamically created

role delegation
- can assumes roles

Trust Relationship
- who can assume the account
- sts:AssumeRole
- trust relationships are only tested when AssumeRole is called

Permission Policy
- what can i do
- are checked against every request

Revoke STS Sessions
- only affect previous sessions
- add a permission policy to explicit deny based on tokens assume time

be careful with object ownerships with cross account roles

- aws services uses IAM roles to interact with one another
- important for cross account access

## Cross Account Access 
Resource Permission vs Cross Account Roles
- aws canonical user id
- globally unique user id
- for cross account grant

S3 - ACL, Bucket Policy and Assume Role
ACL and bucket policy suffer from the fact that objects create will belong to the cross account
can be fixed by a policy to explicit deny if user b creates an object in user a bucket without giving user a full control over the object.

prefer assume role

# AWS Organizations
- previously there was no multi account 
- one master account per org
- the rest will be member accounts
- OUs

Service Control Policy
- policy inheritance downwards

Orgs Mode
- Consolidated Billing Mode (subset just for consolidated billing)
- All Features Mode (must be in this mode for SCP)

# Service Control Policies
- impact any downstream accounts inclusive
- SCPs applied to the master account doesnt do anything
- allow or deny

effective permission 
SCP intersect IAM permissions

multiple SCPS
- an intersection/overlap of all SCP
- explicitly deny is always prefaced

## AWS Account Limits
groups 300
roles 1000
managed policy on a user 10

hard limits - cant be change
**iam users in one account 5000**

## AWS Support Tiers
4 support plans

1. Basic - free
2. Developer - testing or hobby
3. Business - for production workloads
4. Enterprise - mission critical workload

Trusted Advisor
- biz and enterprise has full sets

Enhanced Technical Support
- one named contact for dev

AWS Support API
Proactive Programs
- biz and enterprise
  

Enterprise
Technical Account Manager (TAM) coordinates access to programs and other AWS experts as needed.

## AWS Config
- simple to setup
- tracks all the changes made to the config
- standards and compliance
- looking at the product and resources over time
- can integrate with CloudTrail

Key Components in Config

Config Stream
- integration with sns topic

View relationships between the different pieces

**Rules**
evaluate the state of config to see if they are compliant


## AWS Service Catalog
what is a service catalog
- package and document services offering
- internally between OUs
- or to external customers
- steps involved and cost and other information
- per region service
- used for SELF SERVICE deployment

Product - cf templates
Portfolio - can have a single or multiple products
Self service deployment via service Catalog

Constraints
- give launch permissions

## Cost Optimization
default - on demand the most flixible

Startup priority
- reserved -> on demand -> spot 

EC2 = 12 or 36 months
committing being billed at this rate

can choose a region or a specific AZ

https://github.com/open-guides/og-aws#billing-and-cost-management

https://aws.amazon.com/blogs/aws/amazon-ec2-update-streamlined-access-to-spot-capacity-smooth-price-changes-instance-hibernation/

https://aws.amazon.com/blogs/compute/new-amazon-ec2-spot-pricing/


# Advanced Identity Management

Identity Federation
- trust and extenal identity vendor

- AssumeRole
- AssumeRoleWithWebIdentity
- AssumeRoleWithSaml

assumerole with web identity has no access to the aws cli
user saml instead

**IAM Permission Boundary**
- aws orgs - service control policy - account level boundary
- maximum of permissions avaliable
- can be applied to a account or a role
- taken as a union for effective permissions
- used as a guard
- used to allow permission delegation
- allowing them to manage but not change their own permissions

## Policy Evalutaion
- really important!!!!

EXPLICIT DENY -> EXPLICIT ALLOW -> IMPLICIT DENY


flow
- Org boundaries
- user or role boundaries
- role policy (only applies to sts assume role)
- permissions (inline on managed policy of user or group)
- must match with resource based permission

**in a single account**
SCP AND (IAM OR RESOURCE)

**cross account**
SCP AND (IAM AND RESOURCE)
