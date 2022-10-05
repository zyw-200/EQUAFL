## EQUAFL

Our tool is mainly built on top of QEMU.
The source code we modified as follows.

###  Outline

EQUAFL contains both observation and replay mechanisms.
The observation mechanism is mainly implemented in the following source file.

  Full/cpu-exec.c

Notes: we implment the observation of dynamic files by instrumenting the both beginning and the end of the system call executions.
 You can refer to the function "syscall_start_ana" and "syscall_end_ana" in the source file.
 In particular, file-related system call includes open, read, write, dup, dup2, etc. 

  Full/linux_vmi_new.cpp
  
Notes: we identify application launch varaibles including arguments and environment variables by instrumenting the "do_execve" function. You can refer to the function "execve_analysis_vmi".
The function would be executed after detecting the kernel execution to specific program point, and then it performs dumping for the launch variables.

The replay mechnism is mainly implemented in the following source file.

  linux-user/main.c

Notes: we implment the replay mechnism of EQUAFL by instrumenting the system call execution. You can refer to the function "determine_local_or_not". The instrumented system calls are as follows.
Process Limit: getrlimit, setrlimit
Network: socket, bind, listen, accept, getpeername, getsockname, getsockopt,  poll, select, etc.

We have written the comments for the implmentation in these source files.

### Setup

In the top directory,

  ./configure --target-list=mipsel-linux-user,mips-linux-user,arm-linux-user --static --disable-werror
  make
  
Then, you can get the compiled emulator for enhance emulation, which can be obtained from  mips-linux-user, mipsel-linux-user, arm-linux-user directory.
