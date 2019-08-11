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
- Mostly return value of zero means success
- The C library, when a system call returns an error, writes a special error code into the global error number variable
- perror() translates human readable error via library function

# System Call Numbers

- In Linux, each system call is assigned a syscall number
- Process does not refer syscall by name instead uses syscall number
- sys_ni_syscall() (Not implemented system call) does nothing except return -ENOSYS error corresponding to invalid system call. This function is used to "plug the hole" in the rare event that a syscall is removed or otherwise made unavailable
- kernel keeps a list of all registered system calls in table "sys_call_table"

# System Call Performance

System calls in Linux faster due to context switch times, entering and existing the kernel is stream lined and simple affair

# System Call Handler

- User space application can't execute kernel code directly because the kernel exists in a protected memory
- User space application signals to the kernel that they want to execute a system call and switch to kernel mode to execute the instruction in kernel space
- Mechanism to signal the kernel is a sotware interrupt: Incur an exception and system will switch to kernel mode and execute the exception handler which is actually a system call handler
- The defined software interrupt on x86 is interrupt number/system call handler 128 which is incurred via int 0x80 instruction
- System call handler is the aptly names function as system_call()
- Recently x86 processors added a feature known as sysenter. This feature provides a faster, more specialized way of trapping into a kernel to execute a system call than using the int interrupt instruction 

# Denoting the Correct System Call

- On x86, the syscall number is fed to the kernel via the eax register 
- Before causing the trap into the kernel, user space sticks in eax the number corresponding to the desired system call
- The system call handler then reads the value from eax
- The system_call() function checks the validity of the given system call number by comparing it to NR_syscalls. If it is larger than or equal to NR_syscalls, the function returns -ENOSYS else specified call is invoked:
call \*sys_call_table(,%rax,8)
- Each element in the system call table is 64 bit (8 bytes), kernel multiplies system call number by 4 to arrive at its location in the system call table (NEED TO CHECK if its multiply by 4 or 8 and also know for 32 bit x86?)
- Invoking the system call handler and executing a sytem call

Example:
USER SPACE/APPLICATION call_read() -> read() wrapper -> KERNEL SPACE system_call() -> sys_read()
















