# Route 53

## DNS

Records

Start of Authority (SOA) Record


**A Record** - fqdn to IPV4

**AAAA Record** - fqdn to IPV6 

**Canonical Name (CNAME) **-> A/AAAA to A/AAAA

**Mail Exchange (MX) **- used for main. fqdn to A/AAAA

**Name Server (NS)** just stores ip addr of ns

**Pointer (PTR)** - reverse of an A/AAAA. A/AAAA to fqdn

**Sender Policy Framework (SPF)** - 
used for main spam protection. 
tells a recieving mail server if the originating IP is allowed to send on behalf of this email 
stores an IP

**Text (TXT)**
does nothing but store text info

**Service (SRV)**
can be used to give server and port numbers for resources in the zone

## Aws Route 53
Does 3 main things

1. Domain Registration
2. DNS Resolution
3. Health checking

Domain Registration
option to import existing domains form other registrar
supports regristation of new domains with tlds

When you register a domain a *Hosted Zone* created and you can route it where ever you want.


Hosted Zone - 
A hosted zone is a collection of resource record sets hosted by Amazon Route 53.
can be public and private

CNAMES are not allowed in hosted zones in route 53 use alias instead
CNAME equvilent 


dont use A records for subdomains


Surpported Record Types
all listed above + routing policy


**Routing Policy**
Routing policy options are simple, weighted, latency based, failover, and geolocation.

**Simple**
one to one mapping
simple

**Weighted**
weighted round robin
a cheap load balancer

weight / sum of all weights

**Failover**
really impt for HA
failover will take place if healthcheck fails
routes to secondary instance

**Geolocation**
maps to nearest instacne using reverse geo lookup of ip addrs
may not be that accurate 
need a catch all

**Latency-Based**
only for ec2 instances
based off latency info that aws posses
the best!


### Health checks
can configure alarms with CloudWatch

## Additional Resources
whitepaper
https://d1.awsstatic.com/whitepapers/hybrid-cloud-dns-options-for-vpc.pdf
use ec2 to forward dns requests

https://www.youtube.com/watch?v=PVBC1gb78r8
v good

Best practice:
lower ttl
make critical changes
up ttl again
