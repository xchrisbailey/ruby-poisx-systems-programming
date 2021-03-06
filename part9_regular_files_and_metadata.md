# Regular Files and Metadata
Regular files on a persistent filesystem are pointers/links to internal
metadata known as inodes.  The implementation details of inodes vary
between various filesystems and is usually not the concern of user
space.  User space developers should only need to know a few common
traits about inodes

### Device Number (File::Stat#dev)
The device number is a unique integer identifier for the device an inode
belongs to.  Each file system partition is considered its own "device",
so a device number does not necessarily correspond to a physical device
(e.g. hard disk drive)

This device number is unique within a running Unix system, no two
"devices" may have the same device number at the same point in time.

### Inode Number (File::Stat#ino)
The inode number is unique integer identifier for an inode within a
particular device.  Like PIDs, FDs, and device numbers, they are opaque
pointers to complex objects in kernel space.

The combination of device and inode number is required to uniquely
identify any inode (and thus file) in the system.


### File Names
In many cases, there is a 1:1 relationship between file names and inodes.
That is, each file name points to its own inode, much like how one
variable points to one Ruby object:

    ".bashrc" -> inode_number=123, device_number=456

It is important to know the file name -> inode relationship is one way.
Inodes do not know which file name(s) it has pointing to it.  Similarly,
Ruby objects do not know which variable name(s) are assigned to it.

When working with File objects, one should know the File#path method
return value in Ruby is determined in user space when File.open is
called, _not_ when File#path is called:

    file = File.open(".bashrc")
    file.path       # returns value of ".bashrc" determined above


### Creating Inodes and Files
Making the open(2) system call with the O_CREAT flag will create a file.

File.open can accept the equivalent IO::CREAT constant or with
fopen(3)-style mode strings ("w", "a") to create an inode if the
specified file name does not exist.

The Unix touch(1) command and FileUtils.touch method both create files
with open(2) in this way.  Historically there is also a creat(2) system
call for creating and opening files, but open(2) is more flexible and
preferred.  Ruby does not use nor expose creat(2) to the user.

### Hard links
Since you should understand how two (or more) variables can point to the
same object in Ruby, you should be able to understand how two (or more)
file names can point to the same inode (and thus the same file) on one
device.

Like two variables pointing to the same Ruby object, two (or more)
links pointing to the same inode are indistinguishable from each other:


    "filename_1" ------+
                       |
    "filename_2" ------+----> inode_number=123, device_number=456
                       |
    "filename_3" ------+
                       |
                     ...
                       |
    "filename_X" ------+

Hard links to existing files are created with the File.link method which
makes the link(2) system call.  There is usually a system-dependent
limit on the number of links to a particular inode, File.link will
raise Errno::EMLINK if this limit is hit.


### Number of Hard Links (File::Stat#nlink)
While inodes do not know which file name(s) point to it, inodes are
aware of how many file names point to it.  Each time a hard link
is created, the number of hard links to the referenced given inode
is incremented.

File names may be removed with the unlink(2) system call (via
File.unlink in Ruby).  Unlinking a file will decrement the number of
links an inode has.  It is possible for an inode to have zero links to
it and remain valid (which we will surely discuss later).


### File::Stat
File::Stat is the Ruby class to represent inode information to the user.
All of the fields mentioned above (dev, ino, nlink) are available from
a File::Stat object.

File.stat is a wrapper for the stat(2) system call and returns a
File::Stat object for a given path.

If you have a Ruby IO object (and thus file) open, you can use IO#stat
to get the File::Stat object for the file descriptor belonging to the
IO/File object.  IO#stat makes the fstat(2) system call instead of
stat(2).

File::Stat is the Ruby equivalent/wrapper of the "struct stat"
structure seen by C programmers.

We will discuss File.lstat when we discuss symbolic links.

## License: GPLv3 (or later, at the discretion of Eric Wong) http://www.gnu.org/licenses/gpl-3.0.txt
