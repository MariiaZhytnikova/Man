# GDB Manual: Basic Commands and Tips
These are just some of the basic commands and useful tips for working with GDB. It’s an incredibly powerful tool for debugging, especially when working with complex code. As you become more familiar with it, you’ll find even more advanced features and ways to streamline your debugging process.

## Table of Contents
0. [List of Commands](#list-of-commands)  
1. [Starting GDB](#starting-gdb)  
2. [Basic Commands](#basic-commands)  
3. [Breakpoints](#breakpoints)  
4. [List and View Code](#list-and-view-code)  
5. [Stepping Through Code](#stepping-through-code)  
6. [Inspecting Variables](#inspecting-variables)  
7. [Backtrace](#backtrace)  
8. [Quit GDB](#quit-gdb)  
9. [Inspecting Memory](#inspecting-memory)  
10. [Using GDB with Multithreaded Programs](#using-gdb-with-multithreaded-programs)  
11. [GDB Layout Commands](#gdb-layout-commands)

---

## 0. List of commands

| Command	| Description |
|---------|-------------|
|run (or r)	| Start the program and optionally pass command-line arguments. |
|continue (or c)	| Continue program execution after a breakpoint or interrupt. |
| next (or n)	| Execute the next line of code, stepping over function calls. |
| step (or s)	| Step into the next line of code, including function calls. |
| finish	| Finish the current function and return to the caller. |
| until	| Continue execution until a specific line or instruction is reached. |
| interrupt (Ctrl+C)	| Interrupt the program’s execution (break out of a running program). |
| break (or b)	| Set a breakpoint at a specified function, line, or address. |
| delete	| Delete one or more breakpoints. |
| disable	| Disable one or more breakpoints temporarily. |
| enable	| Re-enable a previously disabled breakpoint. |
| watch	| Set a watchpoint to break when a variable's value changes. |
| catch	| Catch specific signals or exceptions (e.g., catch signal SIGSEGV). |
| print (or p)	| Print the value of a variable or an expression. |
| set	| Set the value of a variable. |
| info locals	| Display all local variables in the current function. |
| info args	| Display the arguments passed to the current function. |
| info registers	| Display the contents of CPU registers. |
| backtrace (or bt)	| Show the stack trace (call stack). |
| frame	| Switch to a specific stack frame. |
| x	| Examine memory at a specific address, in various formats (e.g., hex, string). |
| layout src	| Display the source code in TUI (Text User Interface) mode. |
| layout asm	| Display assembly code in TUI mode. |
| layout split	| Show both source code and assembly code side-by-side in TUI mode. |
| layout regs	| Show the CPU registers in a separate window in TUI mode. |
| tui enable	| Enable TUI (Text User Interface) mode with multiple panes. |
| tui disable	| Disable TUI mode and return to regular GDB interface. |
| info breakpoints	| Display information about all active breakpoints. |
| info threads	| List all threads in the program. |
| thread	| Switch to a specific thread in multi-threaded programs. |
| file	| Load a program or a core dump into GDB. |
| target	| Set the debugging target (e.g., remote debugging). |
| quit	| Exit GDB. |
| help	| Display help information for a specific command or topic. |
| set pagination off	| Disable pagination in output (useful for large outputs). |
| set confirm off	| Disable confirmation prompts (useful for automating debugging). |
| set disassembly-flavor	| Choose the disassembly syntax (e.g., Intel or AT&T). |
| set variable	| Set the value of a variable. |
| info stack	| Show information about the program's call stack. |
| target remote	| Connect to a remote system for debugging. |
| core	| Load a core dump file into GDB for post-mortem debugging. |

## 1.Starting GDB

To start GDB, use the following command in the terminal:

    gdb <your-program>

This will launch GDB and load your program for debugging. If you want to debug a specific core dump file, you can do it like this:

    gdb <your-program> core

## 2. Basic Commands
Run the Program

Once inside GDB, use the run (or just r) command to start the program:

    (gdb) run

You can also pass command-line arguments to your program when you run it:

    (gdb) run arg1 arg2

Finish executing the current function and return to the caller.

    (gdb) finish


## 3. Breakpoints

Breakpoints are used to pause the program's execution at specific points.

To set a breakpoint at a function:

    (gdb) break <function-name>

To set a breakpoint at a specific line in a file:

    (gdb) break <file-name>:<line-number>

To set a conditional breakpoint (only break if a certain condition is met):

    (gdb) break <function-name> if <condition>

### Set a Breakpoint on a Specific Condition

You can set breakpoints that only trigger when a specific condition is true. For example, break when the value of a variable exceeds a threshold:

    (gdb) break <function-name> if <variable-name> > 10

### Break at an Address

Instead of breaking at a function or line, you can set a breakpoint at a specific memory address (useful for lower-level debugging):

    (gdb) break *<memory-address>

Delete a breakpoint. You can specify a specific breakpoint number or delete all breakpoints.

    (gdb) delete 1       # Delete breakpoint number 1
    (gdb) delete         # Delete all breakpoints

Temporarily disable a breakpoint without deleting it. The breakpoint can be re-enabled later.

    (gdb) disable 1      # Disable breakpoint number 1

Enable a previously disabled breakpoint.

    (gdb) enable 1       # Enable breakpoint number 1

List all active breakpoints and their status.

    (gdb) info breakpoints

## 4. List and View Code

You can view the code around the current line or the function you're interested in:

To list lines of code around the current line:

    (gdb) list

To list specific lines or a function:

    (gdb) list <function-name>

## 5. Stepping Through Code

Once your program hits a breakpoint, you can step through the code:

next (or n): Move to the next line of code, stepping over function calls.

    (gdb) next

step (or s): Step into a function call.

    (gdb) step

continue (or c): Continue execution until the next breakpoint or program termination.

    (gdb) continue

Continue execution until a specified line number or instruction is reached. This is useful when you want to run until a certain point, but don’t know the exact path.

    (gdb) until <line-number>

## 6. Inspecting Variables

GDB allows you to inspect the values of variables in the program.

To display the value of a variable:

    (gdb) print <variable-name>

To print the value of a variable in a different format (e.g., hexadecimal):

    (gdb) print /x <variable-name>

To watch a variable (automatically print its value when it changes):

    (gdb) watch <variable-name>

You can modify the values of variables during debugging. This is useful for testing how your program behaves when variables are changed:

    (gdb) set var <variable-name> = <new-value>

For example:

    (gdb) set var counter = 5

Display all local variables in the current stack frame.

    (gdb) info locals
    
Display all the arguments passed to the current function.

    (gdb) info args

Display the contents of all CPU registers.

    (gdb) info registers

Examine memory at a given address in various formats (e.g., hexadecimal, decimal, characters).

    (gdb) x/4x 0x601040    # Examine 4 words at address 0x601040 in hexadecimal
    (gdb) x/4d 0x601040    # Examine 4 words in decimal
    (gdb) x/s 0x601040     # Examine a string at address 0x601040

## 7. Backtrace

Display the stack trace, showing the function calls that led to the current point in the program. If your program crashes, you can use the backtrace (or bt) command to see the stack trace:

    (gdb) backtrace

    (gdb) backtrace full    # Full backtrace with local variables

This will show the call stack, helping you understand where the crash happened.
Select and inspect a specific stack frame.

    (gdb) frame 1           # Switch to frame 1 (i.e., the second call in the stack)

Show detailed information about the stack, including function names and arguments.

    (gdb) info stack

Catch specific events, like exceptions or signals. This can be useful for debugging events like segmentation faults or system signals.

    (gdb) catch signal <signal-name>   # Catch a specific signal (e.g., SIGSEGV)
    (gdb) catch exception <exception-type>   # Catch exceptions in C++
    
## 8. Quit GDB

When you’re done, you can exit GDB using:

    (gdb) quit

## 9. Inspecting Memory

If you want to inspect raw memory, use the x command:

    (gdb) x/<format> <memory-address>

Where <format> can be things like x for hexadecimal, d for decimal, and so on. For example:

    (gdb) x/4x 0x601040

This will examine 4 words of memory at the address 0x601040 in hexadecimal.
Modifying Variables


## 10. Using GDB with Multithreaded Programs

When debugging multithreaded programs, use the following commands to switch between threads:

To list all threads:

    (gdb) info threads

To switch to a specific thread:

    (gdb) thread <thread-id>

To stop a specific thread:

    (gdb) thread <thread-id> stop

## 11. GDB Layout Commands

The layout command controls how the GDB interface is displayed. You can switch between different views, such as source code, assembly, or mixed views, and arrange them side-by-side.

Here are some of the layout options:
Display the source code window, which shows the current source code and any breakpoints.

    (gdb) layout src

Show the assembly code window. This is useful if you're debugging at the assembly level or want to view the machine-level code alongside your source.

    (gdb) layout asm

Display both the source code and assembly code windows side by side. This is useful for mixed debugging (source + assembly).

    (gdb) layout split

Display the registers window. This shows the values of the CPU registers, which is particularly useful when you're examining the program’s execution state.

    (gdb) layout regs

Display the current layout configuration.

    (gdb) layout
    
Switch to a single-window view and remove any active layout.

    (gdb) layout none

Redraw the TUI (Text User Interface): You can redraw the entire TUI window, including the source code, registers, and other views, with the layout command or the tui refresh command:

    (gdb) tui refresh

    (gdb) refresh

This command redraws the TUI layout, which can be useful if the screen gets corrupted or you need to refresh the view manually.

