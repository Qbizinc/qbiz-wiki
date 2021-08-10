---
title: Definition of Distributed File System
description: 
published: true
date: 2021-08-10T19:19:28.211Z
tags: 
editor: markdown
dateCreated: 2021-08-10T19:19:08.235Z
---

# What
Distributed file systems exist primarily for two purposes:
- Increased data storage (the disks on one node are not enough)
- Resiliency (protect against data loss or provide failover to ensure service-level)

## Metadata vs. Storage
In any file system, metadata is always accessed first. This lookup retrieves the actual block addresses to use. The block addresses may then be used to retrieve the actual physical blocks.

On distributed file systems, metadata is usually centralized in a known place. This way, storage nodes may be elastic - in elastic architectures, new nodes have their own network addresses, which have to somehow be made known to user applications/processes. Centralizing the metadata avoids the storage nodes needing to broadcast to the users; this allows the users to be ignorant of storage node addresses.

In peer-to-peer architectures, everything is distributed and protocols for elasticity (including broadcasting new node network addresses) are a focus.

## Data Loss
Protecting against data loss usually requires saving multiple copies of the same data. In distributed file systems, this means multiple copies of the same blocks saved on different storage nodes.

The number of copies is typically known as the ***replication factor***.

# References
- [Wikipedia](https://en.wikipedia.org/wiki/Clustered_file_system#Distributed_file_systems)
