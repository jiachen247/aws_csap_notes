# AWS Identity and Access Management (IAM)

## what IAM is not
its not an AD
its not an OS identity management system

IAM is used to manage AWS specific resources

## Principals
represents a user
can be a human or an app

### Root Principal
created by default
has access to all the resources in the account

best practices is to create a IAM user to carry out everyday task just like in a nix env

### IAM User Principal
users are persistant


> Users are an excellent way to enforce the principle of least privilege; that is, the concept of allowing a person or process interacting with your AWS resources to perform exactly the tasks they need but nothing else. Users can be associated with very granular policies that define these permissions.

### Roles/Temp Security Tokens
tokens are granted vis STS
AWS Security Token Service

from 15 minutes to 36 hours

use cases
1. Amazon EC2 Role
2. Cross-Account Access
3. Federation

#### \#1 Amazon EC2 Role
removes the need to securely store api keys
using roles generate temp keys on the fly

#### \#2 Cross-Account Access
This is used to grant accts outside of your org temp access via roles used for contractors and supplies.

#### \#3 Federated accounts
can leverage 3rd party identity providers


eg google facebook via oauth

external -> OpenID connect (built upon oauth2)
internal -> LDAP
must be SAML compliant


Principal|Traits
-|-
Root User|Cannot be limited Permanent
IAM Users|Access controlled by policy, Durable, Can be removed by IAM administrator
Roles/Temporary Security Tokens|Access controlled by policy Temporary. Expire after specific time interval

## Authentication

Username/Password
can set password policy
web portal uses this

Access Key
Access key id 20 chars
secret 40 chars
sdk and cli uses this

Access Key/Session Token
used with roles
via STS

by default no access key or password

## Authorization

level of authorization is done via policies

effect ->  allow or deny
service -> which services does this apply to
resource -> the arn that this rule applies to

`"arn:aws:service:region:account-id:
[resourcetype:resource"`

action -> what actions do this policy apply to

condition ->  really powerful
eg check ip belongs to a range

```
{    
    "Version": "2012–10–17",    
    "Statement": [        
        {            
            "Sid": "Stmt1441716043000",            "Effect": "Allow",  <-  This policy grants access            
            "Action": [ <-  Allows identities to list                
                "s3:GetObject", <- and get objects in                
                "s3:ListBucket" <-  the S3 bucket
            ],            
            "Condition": {           
                "IpAddress": {   <-  Only from  a specific                      
                    "aws:SourceIp": "192.168.0.1" <-  IP Address                
                }            
            },            
            "Resource": [                               
                "arn:aws:s3:::my_public_bucket/*" <-  Only this bucket            
            ]        
        }    
    ]
}

```

**Associating Policies with Principals**

User Policy
attached to a user
created in the iam user tab

Group Policies
attached to a group

Managed Policy
created in the iam policies tab
independent of users
many predefined policies

Best practice
create a admin group
create a admin user add to he admin group
log off as root and use this account instead

Can associate a role with a policy

## Other Key features

### Multi Factor Auth (MFA)

> Multi-Factor Authentication (MFA) can add an extra layer of security to your infrastructure by adding a second method of authentication beyond just a password or access key. WithMFA, authentication also requires entering a One-Time Password (OTP) from a small device.T


you can turn this on if you want to enforce your users to use MFA

**Rotating Keys**
AWS tried to help by allowing two active keys at any one time
best practice to regularly rotate them

**Resolving Multiple Permissions**

1. Denied by default
2. check for explicit deny
3. check for explicit accept
4. default deny

# Additional References

https://www.youtube.com/watch?v=YQsK4MtsELU&t=1629s







