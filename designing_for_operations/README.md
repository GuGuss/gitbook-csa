# Designing for operations

## Operational requirements

Software is usually designed based on requirements related to what the ultimate user will see and do.
The best strategy for providing a highly available service is to build features into the software that enhance once's ability to perform and automate operational tasks.

**Designing for operations** means taking into account the normal functions of an infrastructure.

### Configuration

The system must be configurable by automated means. This includes initial configuration and changes made later. It must be possible to perform the following tasks:
* Make a backup of a configuration and restore it
* View the difference between one archived copy and another revision
* Archive the running configuration without taking the system down

These goals are easily achieved with text files in a well-defined format.

### Startup and shutdown

The service should restart automatically when a machine boots up. Ensuring that a service restarts after a reboot can be as simple as installing a boot-time script, or using a system that monitors processes and restarts them.

### Queue draining

A **drain** occurs when the service is told to stop accepting new requests but complete any requests that are "in-flight". This is sometimes called **lame-duck mode**.
