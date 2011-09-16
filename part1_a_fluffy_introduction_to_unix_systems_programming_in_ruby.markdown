A Fluffy Introduction to Unix Systems Programming in Ruby
=========================================================

Systems programming for Unix and Unix-like systems using Ruby
-------------------------------------------------------------
>>(This is a introductory/overview material, more in-depth coverage can be
>>readily be found on the Internet outside of this list)

What is Unix?
-------------
Unix is an operating system originally developed at Bell Labs in 1969. Numerous versions and clones ("Unix-like" systems) have been released over the decades as the Unix family continues to evolve.  Nowadays, POSIX is the set of standards that attempt to define the interfaces of Unix and Unix-like systems[1].

Modern Unix systems have two distinct software layers: User space and kernel space.

>>>User space (user applications, where Ruby runs)
>>>--------------------------------------------------
>>>Kernel space (schedulers, sockets, pipes, VFS ...)
>>>--------------------------------------------------
>>>Hardware (disks, network cards, memory)

As you can see, the kernel acts as a mediator for user space and the underlying hardware.

>>[1] however, "POSIX-compliant" does not guarantee a system is Unix or even "Unix-like". Old Unix systems are not "POSIX-compliant", either.

What is Unix Systems Programming?
---------------------------------
Unix and Unix-like operating systems share a set of common programming interfaces for user space to interact with the kernel (and in turn, hardware).  These interfaces are referred to as "system calls"[2] or "syscalls".

We will (hopefully) learn to interact with Unix and Unix-like operating systems using the Ruby programming language.

Unix systems programming is often thought of as "plumbing" as it deals with basic infrastructure and data flow in and out of the system.  Given Unix is responsible for much of the Internet, the Internet _can_ be thought of as a series of tubes :)

>>[2] - This is absolutely NOT the Kernel#system method in Ruby nor the corresponding C library system(3) function, though we'll cover those later.

Why Unix?
---------
Unix and Unix-like systems are everywhere.  From traditional deployments on workstations and servers to smartphones and tablets, Unix and its clones have become pervasive in the world of computing over the past four decades and shows no sign of decline.

Many implementations of Unix and its clones are free (as in beer and in speech), so it is readily available for anybody with the time and will to study.

The design of Unix is minimalist and simple.  While there have been undeniable missteps and embarassing mistakes over the years, its systems programming interface remains reasonably consistent and straightforward.

Why Systems Programming?
------------------------
Knowledge of systems programming is often useful for understanding, troubleshooting and debugging problems in high-level applications. /Lack/ of systems programming knowledge/understanding can cause problems in both the design and implementation of applications.

Unix systems programming knowledge transcends programming languages.

As long as an application runs on a Unix-like system, Unix systems programming knowledge will be useful for implementing, improving, and troubleshooting applications.

Why Ruby?
---------
Traditionally Unix systems programming is taught in C, but high-level languages such as Perl, Python, and (of course) Ruby can access most of the same Unix APIs available to C programmers.

Unlike C, Ruby is a very forgiving language.  There is no pointer manipulation and no manual memory management.  The Rubyist does need not worry about many common bugs found in C code.

Error-handling is also enforced in Ruby.  While C programmers should check for errors on every syscall, Rubyists have SystemCallExceptions (Errno::\*) thrown in their face when errors are encountered.

As far as the author knows, there is little documentation on Unix systems programming in Ruby (or languages other than C for that matter).  Given the prevalance of programmers working in high-level languages but do not know C, Unix systems programming knowledge may have been off limits to many.

>>License: GPLv3 (or later, at the discretion of Eric Wong)
>>http://www.gnu.org/licenses/gpl-3.0.txt
