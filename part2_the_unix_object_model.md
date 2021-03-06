# The Unix Object Model (as seen from user space)

If you're a Rubyist, you may already be familiar with object-oriented designs.  A running Unix/Unix-like kernel exposes two primary types of objects to userspace:
- 1) Processes
- 2) Open files

### Processes
Processes are instances of running applications.  Ruby programs typically run as their own process[1] in userspace.  Kernels expose a Process IDentifier (PID) as positive integers to userspace programs to uniquely identify processes.

Each PID represents one (private) process object in the OS kernel.  The PID is only a reference or pointer to that object.  Two running processes cannot have the same PID, and two PIDs cannot refer to the same process, but PIDs can be recycled and reused over time.

PIDs are global in scope to each running Unix system.

Processes are created from its parent using the fork(2) system call (which is wrapped by the Kernel#fork method in Ruby).

We will cover processes more in later chapters/posts.

>> [1] although experimental Multi-VM implementations exist that allow multiple Ruby VMs within a single process

### Open Files
unning processes may create, open, and release objects known as "files" in kernel space using APIs provided in user space.  Much of Unix systems programming revolves around manipulating "file" objects of various types.

The kernel exposes non-negative integers known as "file descriptors" to userspace.  These are similar to PIDs as they are integers which point to objects within the kernel.

You may have heard the term: "everything is a file" in Unix literature.

Indeed, each integer file descriptor may refer to one of several types of objects within a running kernel.  All of these objects can be called "files" even if they're not stored on hard disks or visible on the filesystem.

File descriptors may be reused and recycled within the lifetime a process.  They are more frequently recycled than PIDs.

Each process has its own file descriptor table (inherited from its parent).

Unlike PIDs, several file descriptors may refer to the same file object inside the kernel.

File descriptors have properties of their own that are not tied to the underlying file object in the kernel.

As open files are the primary interface for processes to interact with the kernel, I expect the majority of my future posts to cover various aspects of file manipulation in-depth.

### Summary
Processes are containers for open files (among other things).  Open files are objects in the Unix kernel that can be accessed from within processes.

## License: GPLv3 (or later, at the discretion of Eric Wong) http://www.gnu.org/licenses/gpl-3.0.txt
