---
title: Definition of Distributed File System
description: 
published: true
date: 2021-08-23T13:54:25.032Z
tags: 
editor: markdown
dateCreated: 2021-08-10T19:19:08.235Z
---

# What
Distributed file systems exist primarily for two purposes:
- Increased data storage (the disks on one node are not enough)
- Resiliency (protect against data loss or provide failover to ensure service-level)

## **NB:** Beware Online Research
The term ***distributed*** depends on perspective:
- A hardware manufacturer placing multiple SSD chips on a single controller may use the term ***distributed***. From an OS driver perspective, this is a single device.

The warning is: when researching online, understand who wrote the content and what their perspective is before diving in.

For Qbiz purposes, ***distributed*** means ***over network***, and ***network*** means over TCP/IP, which implies user-space, not kernel-space.
- ***Network distance*** may be storage and compute on the same rack. This is common in on-premise data centers.

Regardless of how ***distant***, user-space application processes using TCP/IP have certain physics.

## Metadata vs. Storage
In any file system, metadata is always accessed first. This lookup retrieves the actual block addresses to use. The block addresses may then be used to retrieve the actual physical blocks.

On distributed file systems, metadata is usually centralized in a known place. This way, storage nodes may be elastic - in elastic architectures, new nodes have their own network addresses, which have to somehow be made known to user applications/processes. Centralizing the metadata avoids the storage nodes needing to broadcast to the users; this allows the users to be ignorant of storage node addresses.

In peer-to-peer architectures, everything is distributed and protocols for elasticity (including broadcasting new node network addresses) are a focus.

## Data Loss
Protecting against data loss usually requires saving multiple copies of the same data. In distributed file systems, this means multiple copies of the same blocks saved on different storage nodes.

The number of copies is typically known as the ***replication factor***.

# Journaling
DFSs may optionally use journaling to improve recovery times.

[This paper from Stanford's Systems Research Center on Frangipani describes at a high-level the algorithms involved in journaling and recovery on a peer-to-peer DFS.](https://www.scs.stanford.edu/nyu/01fa/sched/frangipani.pdf)

# Versioning
DFSs may optionally provide versioning features.

[This lecture from University of Auckland's School of Computer Science course CS340 does a halfway decent job of describing at a high-level the design considerations of versioning on DFSs.](https://www.cs.auckland.ac.nz/courses/compsci340s2c/lectures/lecture20.pdf)

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Clustered_file_system#Distributed_file_systems)
- [Journaling: Frangipani paper: Stanford's Systems Research Center](https://www.scs.stanford.edu/nyu/01fa/sched/frangipani.pdf)
- [Versioning: CS340 lecture: University of Auckland's School of Computer Science](https://www.cs.auckland.ac.nz/courses/compsci340s2c/lectures/lecture20.pdf)
