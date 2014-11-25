# Designing for operations

## Operational requirements

Software is usually designed based on requirements related to what the ultimate user will see and do.
The best strategy for providing a highly available service is to build features into the software that enhance once's ability to perform and automate operational tasks.

**Designing for operations** means taking into account the normal functions of an infrastructure.

### Configuration

The **system must be configurable by automated means**. This includes initial configuration and changes made later. It must be possible to perform the following tasks:
* Make a backup of a configuration and restore it
* View the difference between one archived copy and another revision
* Archive the running configuration without taking the system down

These goals are easily achieved with **text files in a well-defined format**.

### Startup and shutdown

The **service should restart automatically when a machine boots up**. Ensuring that a service restarts after a reboot can be as simple as installing a boot-time script, or using a system that monitors processes and restarts them.

### Queue draining

A **drain** occurs when the service is told to stop accepting new requests but complete any requests that are "in-flight". This is sometimes called **lame-duck mode**.

This is important when using a load balancer with multiple backend replicas. Software upgrades are implemented by removing one replica at a tim, upgrading it and returning it to service. If each replica is simply "killed", any "in-flight" requests  will be lost. It is better to have a draining mode, where the replica continues to process requests but intentionnally fails the load balancer's health check requests.

### Software upgrades

It must be possible to **upgrade softwares without taking down the service**. Usually, the software is located behind a load balancer and upgraded by swapping out replicas as they are upgraded.

Nonreplicated systems are difficult to upgrade without downtime. Often, the only alternative is to clone the system, upgrade the clone and swap the newly upgraded system into place. Clones of production systems tend to be imperfect copies because it's hard to assure that the clone was made precisely when no changes are in progress.

### Backups and restores

It must be possible to **backup and restore the service's dara while the system is running**.

**Live backups** are often performed on a read-only replica of the database.

**Live restores** are often done by providing a special API for inserting data during a restore operation. The architecture should allow for the restoration of a single account, preferably without locking that user or group out of the service.

### Redundancy

Many reliability and scaling techniques are predicated on the ability to run multiple, redundant replicas of a service. Therefore, services should be designed to support such configurations.

### Replicated databases

Systems that access databases should do so in a way that supports database scaling. The most common way to scale database access in a distributed system is to create one or more read-only replicas. The master database does all transactions that mutate (*make changes to*) the database. Updates are then passed to the read-only replicas via bulk transfers. Services can access the master database as normal, or if a query does not make any mutations, it is sent to a read-only replica.

Most database access is read-only, so the majority of work is off-loaded to replica. The replicas offer fast, though slightly out of date, access. The master offers full-service access to the "freshet" data, though it might be slower.

### Hot swaps

Service components (*also physical components like disk drives or power supplies*) should be able to be swapped in or out of their service roles without triggering an overall service outage.

### Toggles for individual features

A configuration setting, or **toggle**, should be present to enable or disable each new feature. This allows roll-outs of new software releases to be independant of when the new feature is made available to users.

### Graceful degradation

**Graceful degradation** means that 
