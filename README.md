# PES-VCS — Version Control System from Scratch

## Student Details

- **Name:** Varsha Patil  
- **SRN:** PES1UG24CS516  
- **Course:** Operating Systems Lab  

---

## Objective

To build a simplified version control system similar to Git that:
- Tracks file changes
- Stores snapshots efficiently
- Maintains commit history

---

## Platform

Ubuntu 22.04

---

## Build Instructions

```bash
sudo apt update
sudo apt install -y gcc build-essential libssl-dev

make
make all
make clean
Commands Implemented
pes init              → Initialize repository
pes add <file>        → Stage files
pes status            → Show file status
pes commit -m "msg"   → Create commit
pes log               → Show commit history
Phase 1: Object Storage
Description

Implemented content-addressable storage using SHA-256 hashing.
Each object is stored based on its hash.

Key Concepts
Blob storage
Hashing
Deduplication
Atomic writes
Screenshots

 Screenshot 1A: test_objects output
./Screenshots/Screenshot from 2026-04-20 15-17-55.png

Phase 2: Tree Objects
Description

Implemented tree structures to represent directories and nested files.

Key Concepts
Directory representation
Recursive tree structure
File modes
Screenshots


Phase 3: Index (Staging Area)
Description

Implemented staging area using a text-based index file.

Key Concepts
File tracking
Metadata comparison
Atomic index writing
Screenshots



Phase 4: Commits and History
Description

Implemented commit creation and linking of commits using parent references.

Key Concepts
Commit objects
Parent linking
Snapshot creation
Screenshots


Analysis Questions
Q6.1 Garbage Collection and Space Reclamation

To remove unused objects, a mark-and-sweep algorithm is used.

First, all branch references in .pes/refs/heads/ are taken as starting points. From these commits, we recursively traverse all reachable objects including commits, trees, and blobs. While traversing, each visited object hash is stored in a hash set for fast lookup.

After marking all reachable objects, the system scans the .pes/objects/ directory. Any object not present in the reachable set is considered unreachable and is deleted.

For a repository with 100,000 commits and 50 branches, the number of visited objects depends on how many objects are shared. In the worst case, it may involve visiting all commits, trees, and blobs, which can be several hundred thousand or more objects. However, each object is visited only once due to the use of a hash set.

Q6.2 Why concurrent garbage collection is dangerous

Running garbage collection while a commit is in progress can cause a race condition.

During commit, new objects are created and written to the object store, but the branch reference is updated only at the end. If garbage collection runs before the reference update, it may not see these new objects as reachable and may delete them.

After deletion, when the commit completes, it may reference objects that no longer exist, leading to corruption.

To prevent this, real systems like Git avoid deleting recently created objects, delay garbage collection, and ensure that object creation and reference updates are handled safely.

Conclusion

This project helped understand:

How Git internally stores data
How version control systems manage files efficiently
