# Elastic Cache
has many intreasting use cases
caching is only one use case

supports redis and memcached


EC is an in memeory cache

Fully managed service fully compilient with redis and memcached 

Comparisn of memcched and redis
https://aws.amazon.com/elasticache/redis-vs-memcached/

memcached is simple and multithreaded by default (good for parallel operation) 
redis has more FT and data structures. allows snapshoting and read replication


Smaller nodes but more of them them for FT

**Nodes**
An elasticache deployment contains a cluster of nodes
nodes store the data on ec2 instances

> A single Memcached cluster can contain up to 20 nodes. Redis clusters are always made up of a single node; however, multiple clusters can be grouped into a Redis replication group.

**Memcached Auto Discovery**
allows your client to use the cache without knowing the schema
The Auto Discovery client is available for .NET, Java, and PHP platforms.

**Multi-AZ replication** avaliable for redis


**Scaling**

Horizontal
just add more instances for memcahaced

Vertical
can only have one primary (write)
can have 5 read replicas
cannot change type when running
have to spin up a new cluster
new memcached insteance will be empty
while reds can be created from a backup


 replication groups with redis
 5 read one write
 application logic must route accordinly

 In redis if a primary node fails
 a read replica is promoted to primary via dns manipulation


 Understand That Replication Is Asynchronous
follows the eventual consistent model
a bit trickier for caches



**Backup**
Best practice to set up a replication cluster and snapshot a read replica instead.

