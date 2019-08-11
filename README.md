# LinuxKernelDevelopment

System Calls
- Kernel provides set of interfaces by which process running in user space can interact with the system

# Communicating with the kernel:

Purpose of system calls:

1. Abstract hardware interface: To write or read from the disk the applications are not concerned with type of disk, media or filesystem
2. System security & stability: Kernels can arbitrate access beased on permissions, users and other criteria
3. Virtualized environment - Enables to implement multitasking

# APIs, POSIX and C library

Example: call to printf() {APPLI} -> printf in C lib (CLIB) -> write() in the C lib (C LIB) -> write() system call {KERNEL}

- API is based on POSIX which in turn is composed of IEEE standard
- POSIX is a excellent relationship between APIs and system calls
- C library implements the main API on the UNIX systems including the standard C library and the system call interface

# Syscalls

- System calls accessed via function calls defined in the C library
- System calls provide a return value of type long that success or error. Mostly negative return value indicate error.
-
