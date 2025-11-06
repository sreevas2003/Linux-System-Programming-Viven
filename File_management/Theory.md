
## 1. What are the types of files present in Linux OS?

Linux OS supports seven major types of files, each serving a different purpose in the system.
| File Type               | Identifier | Description                                                                        |
| ----------------------- | ---------- | ---------------------------------------------------------------------------------- |
| Regular (ordinary) file | -          | Contains user data such as text, images, executables, or code.                     |
| Directory               | d          | Contains names and addresses of other files/directories.                           |
| Symbolic link (symlink) | l          | Points to another file (similar to shortcuts).                                     |
| Block device file       | b          | Represents devices that require block-oriented I/O (e.g. disks).                   |
| Character device file   | c          | Represents devices requiring character-oriented I/O (e.g. keyboard, serial ports). |
| FIFO (named pipe)       | p          | Supports one-way process communication.                                            |
| Socket file             | s          | Used for network and inter-process two-way communication.                          |

---

## 2. How do IPC Objects ,named pipes, be accessed? 

Named pipes (FIFOs) are accessed like normal files.
Processes use open(), read(), write(), and close() to communicate through them after creating them with mkfifo().

---

## 3. User space application sends request to Hardware by using which I/O calls? 

User-space applications send requests to hardware indirectly through the kernel using system calls such as:

open() – open device file (e.g., /dev/ttyS0)

read() / write() – send or receive data

ioctl() – send control/configuration commands to the hardware

mmap() – map device memory to user space (if supported)

✅ In short: Hardware is accessed from user space through device files using read(), write(), and ioctl() system calls

---
