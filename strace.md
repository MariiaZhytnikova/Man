The strace -f -e trace=desc command is used to trace file descriptor-related system calls while running a program.
## Command Breakdown:

-strace:
        Strace is a debugging tool that traces system calls made by a process during execution. It helps in understanding how a program interacts with the operating system (e.g., file I/O, memory allocation, network communication).
-f:
        This option tells strace to trace child processes as well. If your program uses fork() or clone(), strace will follow these child processes and trace their system calls too.
-e trace=desc:
        The -e trace=desc option tells strace to only trace file descriptor-related system calls. This includes calls like open(), read(), write(), close(), dup(), dup2(), pipe(), and others that are related to file descriptors.

## What is Traced?

With -e trace=desc, strace will show all the system calls related to file descriptors, including:

  - Opening files: open(), openat()
  - Reading and writing files: read(), write()
  - Closing files: close()
  - Duplicating file descriptors: dup(), dup2()
  - Creating pipes: pipe(), pipe2()
  - Memory mapping: mmap() (if used for file mapping)

You will get output for operations that involve file descriptors and how they are handled, including which files are being opened or closed, which pipes are being created, and how input/output is redirected.
Example Output:

Running strace -f -e trace=desc ./pipex "grep a" "wc -l" out will generate output related to the file descriptor operations of your pipex program. Here's a sample of what you might see:

    pipe2([3, 4], 0)                        = 0
    openat(AT_FDCWD, "in", O_RDONLY)        = 3
    dup2(3, 0)                              = 0
    close(3)                                = 0
    openat(AT_FDCWD, "out", O_WRONLY|O_CREAT|O_TRUNC, 0777) = 4
    dup2(4, 1)                              = 1
    close(4)                                = 0

Explanation:

    pipe2([3, 4], 0):
  Creates a pipe with file descriptors 3 (read) and 4 (write). This is typically used to establish inter-process communication (IPC) between the parent and child processes in pipex.

    openat(AT_FDCWD, "in", O_RDONLY):
  The child process opens the in file for reading and gets file descriptor 3.

    dup2(3, 0):
  The child redirects its standard input (file descriptor 0) to the file descriptor 3 (the in file), so that it can read from in.

    close(3):
  The file descriptor 3 is closed after the redirection.

    openat(AT_FDCWD, "out", O_WRONLY|O_CREAT|O_TRUNC, 0777):
  The parent process opens the out file for writing with the O_WRONLY flag and the O_CREAT and O_TRUNC flags to create or truncate the file. It gets file descriptor 4.

    dup2(4, 1):
  The parent redirects its standard output (file descriptor 1) to the file descriptor 4 (the out file), so that the output will be written to out.

    close(4):
  The file descriptor 4 is closed after the redirection.

This is how strace shows you the interactions with file descriptors, pipes, and file I/O during the execution of a program.
Useful Tips:

   Why Use -e trace=desc?
        This filter can be very helpful when you're debugging issues related to file I/O, pipe communication, or file descriptor management.
        It allows you to focus on just the parts of the program that deal with files and their descriptors, without overwhelming you with other system calls.
   What Other Filters Can You Use with -e?
        You can also trace other types of system calls like network (trace=network), signal (trace=signal), and more. For example, -e trace=network traces only network-related system calls.
