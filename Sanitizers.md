## Compiler Sanitizers
To make use of sanitizers, you can add the following flags to your gcc or clang compiler:

For Undefined Behavior Detection: Use -fsanitize=undefined to detect undefined behavior, including accessing uninitialized variables.

Example compile command:

    gcc -fsanitize=undefined -g -o my_program my_program.c

This enables undefined behavior sanitizer (UBSAN), which will report issues like uninitialized variables, illegal memory accesses, and more.

For Memory Issues (e.g., uninitialized memory): Use -fsanitize=address to catch issues like memory leaks, use of uninitialized memory, and heap/stack buffer overflows.

Example compile command:

    gcc -fsanitize=address -g -o my_program my_program.c

This enables address sanitizer (ASAN), which helps detect issues related to memory management, such as using uninitialized memory.

Combining Both: You can combine both -fsanitize=undefined and -fsanitize=address for a thorough check:

Example:

    gcc -fsanitize=address,undefined -g -o my_program my_program.c

### Key Sanitizers:

    -fsanitize=undefined (UBSan):
  Detects undefined behavior (e.g., accessing uninitialized variables, signed integer overflow, invalid memory accesses).
  Reports uninitialized values, misuse of types, etc.

    -fsanitize=address (ASan):
  Detects memory errors such as:
            - Use of uninitialized memory.
            - Memory leaks.
            - Buffer overflows.
            - Stack/heap corruption.
  Ideal for catching uninitialized variable errors in your code.

    -fsanitize=thread (TSan):
  Detects data races in multi-threaded programs.

    -fsanitize=leak (LSan):
  Detects memory leaks by tracking memory allocations and deallocations.

    -fsanitize=memory (MSan):
  Detects use of uninitialized memory (different from ASan).

### Example to Compile and Run with Sanitizers:

Compilation: Compile your code with the sanitizer enabled.

    gcc -fsanitize=address,undefined -g -o my_program my_program.c

The -g flag is for generating debugging information, which makes it easier to trace the source of errors.

Running the Program: After compiling with sanitizers enabled, run your program as usual.

    ./my_program

The sanitizer will detect and print any errors (like uninitialized values) during execution.

### Interpreting the Output:

When you run your program with sanitizers, you'll see output that indicates what kind of issue was detected. For example, an uninitialized variable issue may look like this:

    ==12345==WARNING: AddressSanitizer: use of uninitialized value
    ==12345==    at 0x401234: some_function (my_program.c:42)
    ==12345==    by 0x401256: main (my_program.c:52)

This shows where the uninitialized value was accessed (line numbers, function names, etc.), which helps you quickly pinpoint the issue.
Common Issues Detected:

    Uninitialized values: If you access a variable that hasn't been initialized properly, the sanitizer will detect this and give an error message.
    Buffer overflows: If you write outside the bounds of an array or a buffer, sanitizers will detect this.
    Memory leaks: If you allocate memory dynamically (e.g., using malloc) and forget to free it, sanitizers will flag this as a memory leak.

Using Sanitizers to Debug Your Code:

If the given the error: Conditional jump or move depends on uninitialized value(s), try:

    -fsanitize=undefined
to detect uninitialized values specifically. This will give more context about what might be happening inside function.

For example:

    gcc -fsanitize=undefined -g -o my_program my_program.c

Once you have compiled with the sanitizer, running your program will provide detailed information about which variable is uninitialized.
