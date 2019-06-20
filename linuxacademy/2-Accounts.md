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