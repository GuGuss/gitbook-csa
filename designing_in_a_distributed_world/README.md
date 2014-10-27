# Designing in a distributed world

## Composition

### Load balancer with multiple backends replicas

Load balancers send health check queries and stop sending traffic to that **backend** it the health check fails.
**Round-robin** is a simple scheme to determine where to send traffic. More complex solutions include the **least loaded scheme** (*this implementation can be a disaster: see the CNN case in September 11th 2001 p.13*).

SCHEMA P.12

### Server with multiple backends

Used when the original query can easily be deconstructed into a number of independent queries that can be combined to form the final answer (*ie: search engines*).

Figure B illustrates the same architecture with replicated, load-balanced, backends. The same principle applies but the system is able to scale and survive failures better.

SHEMA P.14 A & B

Advantages:
* Backends do their work in parallel
* Reply doesn’t have to wait for one backend process to complete before the next begins
* System loosely coupled
* One backed can fail and the page can still be constructed (*by leaving the area blank of filling some default information*)

Vocabulary: The term **fan out** refers to the fact that one query results in many new queries. The queries "fan out" to the individual backends, and the replies **fan in** as they’re set up to the frontend and combined into the final result.

### Server tree

Used to access a large dataset or **corpus**, which is larger than any one machine can hold. Each leaf stores one fraction or **shard** of the whole.

Advantages:
Paralle searching of a large corpus

SCHEMA P.16

## Distributed State

In distributed computing, we store state by storing fractions or shards of the whole on individual machines. In addition, each shard is stored on multiple machines (replicas); thus a single machine failure does not lose access to any state.

When one shard is updated, all of the replicas must be updated too. This may be done by having the root update all leaves, or by the leaves communicating updates among themselves.

Dealing with states involves some kind of replication and sharding, which brings about problems of consistency, availability, and partitioning.

## The CAP principle

This principle states that it is impossible to build distributed system that guarantees consistency, availability and resistance to partitioning. Any one or two can be achieved, but not all three simultaneously. You must always be aware of which are guaranteed.

### Consistency

All nodes see the same data at the same time. If there are multiple replicas and there is an update being processed, all users see the update go live at the same time, even if they’re reading from different replicas. Some systems might provide eventual consistency by guaranting that the update will propagate to all replicas in a certain amount of time.

### Availability

Every request receives a response about wether it was successful or failed. In other words: means that the system is up.

### Partition tolerance

The system continues to operate despite arbitrart message loss or failure of part of the system.

SCHEMA P.23

**Split brain** @TODO

## Loosely coupled systems

Distributed systems are expected to be highly available, to last a long time, and to evolve and change without disruption. Entire subsystems are often replaced while the system is up and running. To achieve this a distributed system uses abstraction to build a loosely coupled system.

## Speed

...
