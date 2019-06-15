# Security

Shared Responsibility Model
- client has alot of responsibility

Principle of Least Privilege

Concepts
1. Identity
2. Authentication
3. Authorization
4. Trust

Identity Store
Identity Providers
Service Providers

Federated Identity Protocols
SAML vs OAuth vs OpenID

SAML - original protocol. xml based. user group and member information for enterprises.

OAuth - handles authentication.

OpenID Connect - built on top of OAuth. SSO for public customers

https://www.gluu.org/resources/documents/articles/oauth-vs-saml-vs-openid-connect/

**Multi Account Management**

AWS Tools for account management
1. AWS Organizations
2. Service Control Policy (SCP)
3. Tagging
4. Resource Groups
5. Consolidated Billing

Account Structure
1. Publishing Account
2. Identity Account
3. Logging Account Account
4. Information Security Account
5. Central IT Account Structure
   
Consolidated Billing
Consolidated Security

SCP - Cascade Down -> affects all sub accounts

**Network Controls and Security Groups**
Security Groups - firewalls

NACLS
- subnet level

SG vs NACL
https://medium.com/awesome-aws/aws-difference-between-security-groups-and-network-acls-adc632ea29ae


**Directory Service**

Cognito - offload sign up and sign ins
AWS Cloud Directory - cloud native directory
AWS MS AD - fully managed ms AD running enterprise version
AD Connector - connect local ad
Simple AD - Samba impl. simple user directory plus ldap compatibility

AD Connector vs Simple AD


AD Connector
- must have existing on premise AD
- Existing AD users can access AWS assets via IAM roles
- Supports MFA via existing RADIUS based MFA infrastructure

Simple AD
- Stand alone AD based on Samba
- supports user account and groups, group policies and domains
- keboeros based SSO
- MFA not supported 
- No trust relationships


**Credential and Access Management**
json used in policy definitions

**STS**
temp credential access to users
https://web-identity-federation-playground.s3.amazonaws.com/index.html

**Token Vending Machine**

Anonymous TVM
Identity TVM 

**AWS Secrets Manager**
- to store passwords or keys
- automatically rotates our RDS passwords
- non encrypted

**Encryption**
At rest vs in transit

**AWS Key Management Service**
tight integration
encrypted key manager

**CloudHSM**
- dedicated hardware device
- single teneted
- dosent integrate nativily
- custom write your applications 
- issue CA
- Customer manages the root of trust

AWS Certificate Manager
- sign ssl/tls certificates
- supports wildcard domain
- does renewals automatically
- can create private certificates

**DDoS**
- PLP - lock down nacls and sgs
- cloudfront!!
- static content on s3
- aws sheild
- aws firewall
- baseline triggers alarms
- route 53 based on geography

**Intruder Detection System**
- less invasive
- fail open
- detect not block
- SIEM

**Intruder Prevention System**
- can block traffic
- blacklist
- Log collection cloudwatch or S3

Security Information and Event Management System (SIEM)

**Bastion Server**
- single entry point in the DMZ
- only have to secure this

CloudWatch vs CloudTrail

CW - operations - alarms
Ct - activities - API

**Service Catalog**
- prevent messes
- framework for admins to create pre defined packages
- granular control
- makes used of adopted IAM roles

Constrains 
1. Launch Constraint - allow end user to deploy without having the proper perms
2. Notification Constrain - sns topic
3. Template Constraint - wizard like
4. Shared Portfolio - cascades down
5. by default - the launch role is inherited from portfolio


Additional Resources

https://d1.awsstatic.com/whitepapers/aws-security-best-practices.pdf

https://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf

https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf

https://aws.amazon.com/whitepapers/#security

https://www.youtube.com/watch?v=gjrcoK8T3To

https://www.youtube.com/watch?v=aISWoPf_XNE

https://www.youtube.com/watch?v=tzJmE_Jlas0

https://www.youtube.com/watch?v=71fD8Oenwxc