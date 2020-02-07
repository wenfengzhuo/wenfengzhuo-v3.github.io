> Disclaimer: This article is a review of a paper published by Facebook. There are interpretations that are based on my own knowledge, which might or might not be correct and will only represent my personal opinions. Some images and quotes are referenced from the original paper with full credits for them. The article should only be used for study purpose.

### Introduction

Caching layer is a critical component in lots of large-scale systems. It greatly reduces the latency of client requests and load to the lower-level systems. The famous open source solutions are Memcached and Redis. An instance of these caching system is analogous to an in-memory hash table that provides similar semantics of operations around key-value pairs. 

While it's straightfoward to build such caching system in a single machine, there are many challenges in making it distributed in a large-scale environment. Things like consistency, paritioning, replication, failover, concurrent modification, etc imply non-trivial efforts in tackling them and unavoidable trade-offs between different factors.

This article will review a paper released by Facebook - [Scaling Memcache at Facebook](https://research.fb.com/publications/scaling-memcache-at-facebook/) to take a closer look at how the team solves those problems. 

### Why Caching at Facebook

In a large social network like Facebook, there are billions of people using the system every day, which impose great demand on computation, network bandwidth and I/O. The system needs to grab resource from multiple sources and then aggreate them, then serve them to millions of users each second. It's infesible for the system to first talk to multiple databases like MySQL and then return the results back to users. Caching system usually stores the data in memory, so it can very low latency access to data compared with traditional databases, which requires disk operations when fetching data.

Facebook uses Memcached as the main solution to the caching system. They used it as a building block to construct a large distributed key-value data store. To distingush Memcached and the distributed system built on top of it, in the paer, `Memcached` refers to the source code of a running binary and `Memcache` refers to the distributed system. The paper claims the Memcache system at Facebook is the largest Memcached installation, processing more than a billion requests per second and store trillions of items.

### Pattern of cache usage

There are two properties of Facebook system that influence the design of the caching system.

**Read Heavy** <br/>
User consumes an order of magnitude more content than they create. So the workload of the system are dominated by fetching data.

**Heterogenous Data Source** <br/>
The read operations fetch data from a variety of sources such as MySQL, HDFS, and other backend servers. This heterogenity requires a more flexible caching strategy.

### Cache Strategy 

The memcache is primarily used as a demand-filled look-aside cache as shown in the below figure from the original paper. 

![Figure 1: Look-aside cache](/assets/Figure1.png)

One import thing to note in the paper, for the write request, the system will send a delete request to the memcache instead of a update request with following reasons:

* Deletes are idempotent. The worst case is that no key in the cache system, then it will look up the data in persistent data store. 
* Memcache is not authoritative source of the data. Updating data in place will probably incure data inconsistency issues.
* (My Note) It's simple to implement than updating the data because we don't need handling concurrent writes to the system or data inconsistency issues.

Memcache is also used as a general key-value store to store pre-comuted results (e.g. results from machine learning algorithms).

### Overall Achitecture

As memcached is a in-memory hash table running on a single server, Facebook has to add several key components in order to make the caching system distributed that satisfy their workload. Below is the final architecture of the their system related to memcache.

![Figure 2: Overall achitecture](/assets/Figure2.png)

A couple of insights that we can derive from the diagram. 

**Replication is through storage cluster** <br/>
In a distributed system, replication is a way to create multiple copies of the same data and store them in different machines. With it, it greatly ensures the durability of data, and for reading operations, it reduces latency as multiple read replicas can serve requests.

For a distributed cache system, it's natural to think we need to copy data from one cache server to another server for replication. While doable, it's unnecessary as caching system is not the autorative source of data and storage system has to replicate data anyway. Cache servers can directly load data from its own replica when it's ready. So unlike database system, usually the replication in a cache system is achieved by multiple replicas reading the same data from a single source of truth. The diagram can depict this approach.

![Figure 3: Replication in cache system](/assets/Figure3.png)

One unique property of a caching system is that it does not need to understake the responsibility of making data durable. Even some data is missing, it's not a deal-breaker as it can always load the data from persistent layer. This property simplifies the replication strategy of memcache. That said, we should keep the cache miss rate in a resonable low level.

**Leader-Follower Replication Strategy** <br/>

For a distributed data system, there are a couple of ways to replicate the data. The common strategies are having single leader, or multiple leaders, or no leader to achieve replication. 

A single leader system can make it easier to implement the replication logic: the followers can simply reply a write ahead log (WAL) sent from the leader to replicate the data. In a read-heavy system, this is usually a good choice. The downside is all writes need to go to the leader so the updates of data will only be available in the replica once it catches up the replication stream. If there are a huge number of writes, the latency of requests will increase significantly. 

Multiple leaders system allows several instances to be the leaders. Each leader can handle write requests simutaneously. Usually, this setup can be seen in a system deployed to mutiple data centers. Each data center can have one leader to respond to write requests. In this way, we don't need to redirect user's write requests to another data center leader to process, which will reduce the latency significantly as cross-region network connection tends to be more slow. The disadvantange of this approach is the conflicts occured due to the writes to the same data. We need to properly handle the conflicts by using some advanced algorithms like operational transformation, version vectors, etc.

Leaderless system does not have a leader. All the instances in the system can respond to read and write requests. When clients writing data, the data usually be written into multiple replicas. Similarly, when reading data, the data will be read from multiple replicas. To make sure the system correct returns the data, there are some requirements for read and write. If there are n replicas, then if a write must be confirmed by w replicas, a read must be confirmed by r replicas, then must w + r > n. It is called quorum read. This way ensures at least one replica will return the latest updated result. This is a ideal situation and assumes n replicas are always the same machines. So if some machines change, it doesn't guarantee the result. This is caled sloppy quorum. The disadvantage of sloppy quorum is that clients might read stale data.

From the diagram, we can tell Facebook uses the single leader approach for storage replication. As we mentioned earlier, there are significant more reads than writes in Facebook system, it makes sense for that setup.

**Various Cluster Setup** <br/>






<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg3ODE4NTg2LDE4NTM0OTE3NV19
-->