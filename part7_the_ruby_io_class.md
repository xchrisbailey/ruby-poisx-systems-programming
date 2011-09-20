# The Ruby IO Class
The IO class wraps file descriptors as Ruby objects and provides
instance methods which wrap system calls.  Each IO object wraps one OS
file descriptor[1].  IO also provides userspace buffering to avoid
system calls and utility methods to make a programmer's life easier.

C programmers may find the Ruby IO class analogous to the (opaque)
"FILE" struct in stdio.

IO is often used via subclasses:

 * File class is mostly intended for regular files
 * TCPSocket for TCP sockets
 * UNIXSocket for UNIX domain sockets
 * UDPSocket for UDP sockets
   ...

All of those classes are based on the IO class and wrap an integer
file descriptor.


### Layers of Buffering

There are at least four distinct layers of buffering within a machine.

    process[0]  | process[1]  | process[2]  | ... process[N]
    ------------+-------------+-------------+---------------
    App buffers | App buffers | App buffers | ...
    Lib buffers | Lib buffers | Lib buffers | ...
    ------------- kernel-user space boundary ---------------
                    Kernel software buffers
                       Hardware buffers

User space buffers are not shared between different processes in Ruby.

Kernel buffers are shared, allowing read(2)-after-write(2) consistency
between different processes.  This is one (of many) ways for cooperating
processes to share data.

A few applications[1] may manage process-shared buffers in user space,
but (stock) Ruby does not have this.

Library buffers (like most memory) in the Ruby IO class /are/ shared
between different Threads and Fibers in Ruby.

Sharing of application buffers between Threads/Fibers is possible but
usually not a good idea.  It is of course, application-dependent.

Buffers may be implemented for both reading and writing.

The Ruby IO class can do the following:

* accept application buffers from the user (IO#write)
* return application buffers to the user (IO#read)
* manage library buffers internally
* copy (and remove) library buffers to kernel buffers
* (hopefully) force kernel and hardware buffers to storage/network

We will cover how to use/influence/avoid them individually as we
go further.  For now, just know they exist and what IO does.


    [1] - Multiple IO objects may wrap a single file descriptor, but
    that is often a bad idea (to be explained later :)

## License: GPLv3 (or later, at the discretion of Eric Wong) http://www.gnu.org/licenses/gpl-3.0.txt
