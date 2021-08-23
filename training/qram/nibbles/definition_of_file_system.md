---
title: Definition of File System
description: 
published: true
date: 2021-08-23T13:27:48.477Z
tags: 
editor: markdown
dateCreated: 2021-08-10T18:56:21.241Z
---

# What
A system to control how data is stored and retrieved.

## Components
- Logical file system: provides an API to a user/process to interact with the file system
- Virtual file system (optional): allows multiple physical file systems to be interacted with as if belonging to a single file system
- Physical file system: physical operation of the storage medium
  - Blocks
  - Buffering
  - Memory management
  - Placement of data to blocks
  - Interacts with device drivers or channels to drive the storage device
  
## Blocks
Storage devices typically do not provide addressing at the byte-level. Therefore, they are grouped into blocks. Also, random-access of bytes is typically against the physics of many types of storage devices. Blocking helps provide performance guarantees.

## Metadata
File systems maintain a **table of contents** mapping **file names** to the specific *physical file system blocks* that make up the file contents.

The metadata system may attach metadata to the file.

### Organizational
- Higher-level constructs like directories/folders are common, and these constructs may have their own metadata.
- Other higher-level constructs exist, such as tags.

Most file systems use the Unix file name and directory conventions.

# Journaling
A journaling file system keeps track of pending writes to improve fault tolerance.

## Workflow
1. A write request is received by the FS.
2. Record the write request before actually fulfilling the request.
3. Mark that the request is being fulfilled.
4. Begin fulfilling the write request in some way that is staged.
5. **&lt;Crash!!!&gt;**
6. **&lt;Power on!!! Recovery!!!&gt;**
7. On start, examine the journal.
   1. See the write request in process.
   2. Throw away the staged write.
   3. Restart the staged write.
8. Complete the staged write.
9. Atomically:
   1. Move the staged write into actually readable write.
   2. Mark the write request complete.
10. Delete the write request from the journal.

# Versioning
A versioning file system provides a form of revision control, allowing multiple versions of the same file to exist at once. In addition:
- A VFS maintains some notion of timeline to make sense of the order and wall-time of versions.
- Often, VFSs maintain logs of write operations.
- Often, VFSs allow for individual file versions to be named stably and therefore read in a ***[consistent](/training/qram/nibbles/definition_of_consistent)*** way.
- Optionally, for FSs that have directories, tags, or other organizational structures, VFSs maintain the above for those organizational structures.

## ***git***
Technically speaking, ***git*** is a versioning file system.
- git tears files apart into lines.
- git stores those lines in its own internal block format, which are hosted on the underlying OS file system.
- git maintains metadata organizing lines into versions of files.
  - This is how git achieves compression across versions of files - it reduces duplication of content by storing each unique line only once.

# References
- [Wikipedia](https://en.wikipedia.org/wiki/File_system)
- [Wikipedia: Blocks](https://en.wikipedia.org/wiki/Block_(data_storage))
- [Wikipedia: Journaling](https://en.wikipedia.org/wiki/Journaling_file_system)
- [Wikipedia: Versioning](https://en.wikipedia.org/wiki/Versioning_file_system)
