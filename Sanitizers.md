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
What AddressSanitizer does:

Memory error detection: ASan inserts checks in your code to detect various memory errors as described earlier (e.g., out-of-bounds, use-after-free).
    Detailed diagnostics: Provides stack traces and memory locations to help you track down issues.
    Runtime overhead: While useful, AddressSanitizer comes with a performance overhead (around 2-3x slower execution in some cases), but it's often worth it for catching memory errors.

Important Notes:

    Memory leak detection: If you want to detect memory leaks specifically, you may need to use the ASAN_OPTIONS=detect_leaks=1 environment variable:

    export ASAN_OPTIONS=detect_leaks=1
    ./your_program

Linking with ASan: 
If your program uses libraries that are not compiled with -fsanitize=address, you may need to ensure they are also compiled with this flag, or you'll get mismatched runtime behavior.

Using AddressSanitizer can greatly help in identifying and fixing tricky memory bugs that might otherwise be difficult to catch with manual inspection or static analysis.

