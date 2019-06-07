## Chapter 5: Elastic Load Balancing, Amazon CloudWatch, and Auto Scaling

> **Elastic Load Balancing** is a highly available service that distributes traffic across Amazon Elastic Compute Cloud (Amazon EC2) instances and includes options that provide flexibility and control of incoming requests to Amazon EC2 instances.

> **Amazon CloudWatch** is a service that monitors AWS Cloud resources and applications running on AWS. It collects and tracks metrics, collects and monitors log files, and sets alarms. 

> **Amazon CloudWatch** has a basic level of monitoring for no cost and a more detailed
level of monitoring for an additional cost.
Auto Scaling is a service that allows you to maintain the availability of your application.


### ELB
Managed load balancer solution
always a solution to run your own load balancer like nginx
achieve high availability

avantages over running your own
+ route to healthy ec2s
+ scales automatically based on metrics
+ integrates with auked in with VPCs
+ supports SSLto scalling
+ security ba termination
  
always reference by dns name and no ip address
ip addr may change

always reference by dns name and no ip address
ip addr may change

Types of Load balancers

### Internet facing
facing the internet

### Internal
for multi tier orgs
can route to private VPCs

### HTTPS balancer
ssl termination can be done here

no SNI

cannot have multiple domains behind the elb

**Listiners**
we can actually listen for TCP connections
layer 4 upwards

some cool features

**Idle Connection Timeout**
60 seconds by default
keep alive > idle timeout

**Cross-Zone Load Balancing**
no need to have the same number of instances in each subnet/az

**Connection Draining**
closes connections to un healthy instnaces


**Proxy Protocol**
added info to the backend instance if this is turned on eg. source dest ip
might be useful

**Sticky Sessions**
make sessions sticky

by default routes user to the least used
with sticky session user is directed to the same instance via a session cookie or AWSELB

**Health Checks**
checks for health of instances perodically


### CloudWatch
> Amazon CloudWatch is a service that you can use to monitor your AWS resources and your applications in real time. With Amazon CloudWatch, you can collect and track metrics, create alarms that send notifications, and make changes to the resources being monitored based on rules you define.

its configurable
scales instances based on metrics from cloudwatch

basic -> every 5 mins
details -> every min

can GET request to get logs

does not aggreegate across regions but across Axa

can be persisted to S3 if you want

5000 alarm limit per account

### AutoScaling

> Auto Scaling is a service that allows you to scale your Amazon EC2 capacity automatically byscaling out and scaling in according to criteria that you define.

### Maintain 
set min and max

### Manual
increase min and max manually

### Scheduled
for recurring events

### Dynamic Scaling
rule based on cloud watch
parameter/policy based

### Componenets of AutoScaling
1. Launch Configuration
2. Auto Scaling Group
3. Scaling Policy


**Launch config**
> aws autoscaling create-launch-configuration -–launch-configuration-name myLC --image-id ami-0535d66c --instance-type m3.medium --security-groups sg-f57cde9d --key-name myKeyPair

used for spinning up new instances
one per autoscaling group


**Auto Scaling Group**
Name: myASGLaunch 
configuration: myLC
Availability Zones: us-east-1a and us-east-1c
Minimum size: 1
Desired capacity: 3
Maximum capacity: 10
Load balancers: myELB

**Scaling Policies**
using CloudWatch alarms to trigger scaling

> > aws autoscaling put-scaling-policy --auto-scaling-group-name myASG --policy-nameCPULoadScaleOut --scaling-adjustment 1 --adjustment-type ChangeInCapacity --cooldown 30  > aws autoscaling put-scaling-policy --auto-scaling-group-name myASG --policy-name CPULoadScaleIn --scaling-adjustment -1 --adjustment-typeChangeInCapacity --cooldown 600

best practice
scale out fast
scale in slowly

**Patching**
change ref to ami in launch config
the slowly terminate instances which will result in new ones being launched