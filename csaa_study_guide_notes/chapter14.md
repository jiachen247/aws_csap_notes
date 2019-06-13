# Architecture Best Practices

https://www.youtube.com/watch?v=CeceqWuZ0Cg

tenets:
1. Design for failure and nothing will fail.
2. Implement elasticity.
3. Leverage different storage options.
4. Build security in every layer.
5. Think parallel.
6. Loose coupling sets you free.
7. Don’t fear constraints.

## Design for failure and nothing will fail

> Everything fails, all the time

**Single Point of Failure**
is a bad thing

address all SPF

solved by active or passive/standby redundancy

active redundency replication
ELB to multi AZ

## Implement elasticity
> Elasticity is the ability of a system to grow to handle increased load, whether gradually over
time or in response to a sudden change in business needs.

use any of the services that start with elastic

Vertically vs Horizontally

prefer stateless applications
can scale horizontally

-store session state in dynamo db and keep webservers stateless


**Deployment Automation**
really important 
helps in scaling without human intervention

Bootstrap instances -

## Leverage different storage options
no one size fits all

refer to storage chapter

all are pretty striaght forward
except S3, EBS and EFS
dynamodb vs EC for session... 

## Build security in every layer

use AWS features
AWS Web Application Firewall (AWS WAF)
AWS Identity and Access Management
(IAM) t

Offload Security Responsibility to AWS
aws takes the cloud we take the workloads

Reduce Privileged Access - use IAM fine grain controls
Follow the standard security practice of granting least privilege


Security as Code
implement security as codes that can be reviewed and stored
CloudFormamation scripts

Real-Time Auditing - AWS Config Rules, Amazon Inspector, and AWS Trusted Advisor


## Think parallel
> The cloud makes parallelization effort

design to leverage multi threading
helps with horizontal scallng

## Loose coupling sets you free
> Design system architectures with independent components that are “black boxes.” The more loosely system components are coupled, the larger they scale.

use microservices and lambdas and SQS to decouple systems

>Loose coupling is a crucial element if you want to take advantage of the elasticity of cloud
computing

## Don’t fear constraints

> When organizations decide to move applications to the cloud and try to map their existing
system specifications to those available in the cloud, they notice that the cloud might not
have the exact specification of the resource that they have on premises. F

constraints may not be always bad

everything has limits including traditional infrastructure

1. E / BE
2. C, B / BC
3. C, D /AE
4. C, D /AD
5. D, E, B /BDE
6. A, E ? /ACD
7. C /C
8. D /A
9. D /D
10. B /B
11. C /C
12. A /A
13. B /B
14. B C E /BCE
15. A /ABE
16. B /B
17. A /A
18. B /B
19. A  /A
20. D /C