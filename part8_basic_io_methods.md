# Basic IO Methods

(Consider C library functions (3) analogues while the system call (2)
 functions are inevitably called by Ruby)

### Opening (and Closing) Files

* File.open - fopen(3) which wraps open(2) and provides a File object
    
* IO#close - fclose(3), close(2)
  Copies Ruby buffers to the kernel, invalidates the IO object
  and releases the file descriptor so it may be reused by the
  kernel.

These methods will be covered in more detail as we dig deeper.


### Some File Operations for Reading and Writing

* IO#read - fread(3) - read all specified bytes

* IO#write - fwrite(3) - write all specified bytes

* IO#sync, IO#sync= - setvbuf(3) - controls Ruby (write) buffering

* IO#flush - fflush(3) - copies existing Ruby (write) buffers to kernel

* IO#syswrite - write(2) - copies application buffers to kernel

* IO#sysread - read(2) - copies kernel buffers to application

(there are more methods which we'll cover in time, of course)


Setting IO#sync= to true will make IO#write copy application buffers
directly into kernel buffers, bypassing Ruby buffers.  IO#sync does not
influence read buffering.

If IO#sync is false, IO#write may copy small application buffers into
Ruby buffers to before internally doing IO#flush.

The non-sys* methods will retry until the operation is complete (or
raise on any errors).  The sys* variants do not retry if the kernel
returns a partial read(2) or write(2) and will also raise on errors.

Mixing sys* and non-sys* methods is not recommended and will result in
errors or exceptions.

If you need more detailed descriptions, check `ri' or other Ruby
doc for Ruby methods.  For C library functions and system calls, your
Unix-like OS should provide manpages and you can search online, too.
I have no intention of regurgitating API documentation.

## License: GPLv3 (or later, at the discretion of Eric Wong) http://www.gnu.org/licenses/gpl-3.0.txt
