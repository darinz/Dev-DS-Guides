# GDB (GNU Debugger) Complete Guide

[![GDB](https://img.shields.io/badge/GDB-12.1+-blue.svg)](https://sourceware.org/gdb/)
[![C](https://img.shields.io/badge/C-Debugging-green.svg)](https://sourceware.org/gdb/documentation/)
[![C++](https://img.shields.io/badge/C%2B%2B-Debugging-orange.svg)](https://sourceware.org/gdb/documentation/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](../LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen.svg)]()

## Table of Contents
- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Basic Commands](#basic-commands)
- [Breakpoints](#breakpoints)
- [Examining Program State](#examining-program-state)
- [Controlling Execution](#controlling-execution)
- [Advanced Features](#advanced-features)
- [Debugging Strategies](#debugging-strategies)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

## Introduction

**GDB (GNU Debugger)** is a powerful command-line debugger that allows you to examine and control the execution of programs. Unlike graphical debuggers, GDB operates entirely through text commands, making it ideal for remote debugging and environments without graphical interfaces.

### What GDB Can Do
- Set breakpoints to pause execution
- Step through code line by line
- Examine variable values and memory
- Analyze program crashes (segfaults)
- View call stacks and function calls
- Modify variables during execution
- Analyze assembly code

## Getting Started

### Prerequisites
Before using GDB, you must compile your program with debugging information:

```bash
# Compile with debug information
$ gcc -g -Og source.c -o program

# Or with additional flags
$ gcc -g -Og -Wall -std=gnu99 source.c -o program
```

### Starting GDB
```bash
# Start GDB with your program
$ gdb program

# Or start GDB first, then load program
$ gdb
(gdb) file program
```

### GDB Configuration
Set up GDB preferences for better debugging experience:

```bash
# Download Stanford's GDB configuration
$ wget https://web.stanford.edu/class/archive/cs/cs107/cs107.1194/resources/sample_gdbinit -O ~/.gdbinit

# Or create your own .gdbinit file
$ echo "set auto-load safe-path /" >> ~/.gdbinit
```

## Basic Commands

### Essential Commands

| Command | Abbreviation | Description |
|---------|--------------|-------------|
| `help [command]` | `h [command]` | Get help about commands |
| `run [args]` | `r [args]` | Start/restart program execution |
| `quit` | `q` | Exit GDB |
| `list` | `l` | Show source code |
| `print expression` | `p expression` | Print variable/expression value |
| `info [cmd]` | `i [cmd]` | Show program information |

### Getting Help
```bash
(gdb) help                    # List all commands
(gdb) help break             # Help for break command
(gdb) apropos breakpoint     # Search for breakpoint-related commands
```

### Running Programs
```bash
# Run program without arguments
(gdb) run

# Run program with arguments
(gdb) run arg1 arg2 arg3

# Run program with input redirection
(gdb) run < input.txt

# Run program with output redirection
(gdb) run > output.txt
```

## Breakpoints

### Setting Breakpoints
Breakpoints pause program execution at specific locations:

```bash
# Break at function
(gdb) break main
Breakpoint 1 at 0x4005f3: file program.c, line 6.

# Break at line number
(gdb) break program.c:20
Breakpoint 2 at 0x40064e: file program.c, line 20.

# Break at line in current file
(gdb) break 15

# Break at function with condition
(gdb) break function_name if condition
(gdb) break 20 if i > 10
```

### Managing Breakpoints
```bash
# List all breakpoints
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000000400a6e in main at program.c:6
2       breakpoint     keep y   0x0000000000400a8c in program.c:20

# Delete specific breakpoint
(gdb) delete 2

# Delete all breakpoints
(gdb) delete

# Disable/enable breakpoint
(gdb) disable 1
(gdb) enable 1

# Clear breakpoint at line
(gdb) clear 20
```

### Conditional Breakpoints
```bash
# Break only when condition is true
(gdb) break 15 if count == 0

# Break after hitting line N times
(gdb) break 15 if ++count == 5

# Break on specific function call
(gdb) break malloc if size > 1000
```

## Examining Program State

### Backtrace (Stack Trace)
The backtrace command shows the call stack:

```bash
(gdb) backtrace
#0  0x0000000000400ac1 in read_frag (fp=0x603010, nread=0) at program.c:51
#1  0x0000000000400bd7 in read_all_frags (fp=0x603010, arr=0x7fffffff4cb0, maxfrags=5000) at program.c:69
#2  0x00000000004010ed in main (argc=2, argv=0x7fffffffeab8) at program.c:211

# Short form
(gdb) bt

# Show more details
(gdb) bt full
```

### Examining Variables
```bash
# Print variable value
(gdb) print variable_name
$1 = 42

# Print expression
(gdb) print array[i]
$2 = 15

# Print with format specifiers
(gdb) print/x variable    # Hexadecimal
(gdb) print/t variable    # Binary
(gdb) print/c variable    # Character
(gdb) print/a variable    # Address

# Print array elements
(gdb) print array[0]@10   # First 10 elements
(gdb) print *array@10     # Same as above

# Print structure members
(gdb) print struct_var.member
(gdb) print struct_var->member
```

### Function Arguments and Local Variables
```bash
# Show function arguments
(gdb) info args
fp = 0x603010
nread = 0

# Show local variables
(gdb) info locals
start = 123 '{'
end = 125 '}'
nscanned = 3

# Show all variables in current scope
(gdb) info variables
```

### Stack Frame Navigation
```bash
# Move up call stack (to caller)
(gdb) up
#1  0x0000000000400bd7 in read_all_frags (fp=0x603010, arr=0x7fffffff4cb0, maxfrags=5000) at program.c:69

# Move down call stack (to callee)
(gdb) down
#0  0x0000000000400ac1 in read_frag (fp=0x603010, nread=0) at program.c:51

# Go to specific frame
(gdb) frame 2
#2  0x00000000004010ed in main (argc=2, argv=0x7fffffffeab8) at program.c:211
```

## Controlling Execution

### Execution Control Commands

| Command | Abbreviation | Description |
|---------|--------------|-------------|
| `continue` | `c` | Continue execution until next breakpoint |
| `next` | `n` | Step over (execute line, skip function calls) |
| `step` | `s` | Step into (execute line, enter function calls) |
| `finish` | | Run until current function returns |
| `until` | `u` | Continue until line with greater number |
| `return` | | Return from current function immediately |

### Stepping Through Code
```bash
# Step over (don't enter functions)
(gdb) next
7       printf("Hello, World!\n");

# Step into (enter functions)
(gdb) step
square (x=25) at program.c:27

# Continue execution
(gdb) continue
Continuing.

# Finish current function
(gdb) finish
Run till exit from #0  square (x=25) at program.c:29
main (argc=2, argv=0x7fffffffeab8) at program.c:20
Value returned is $3 = 625
```

### Advanced Execution Control
```bash
# Continue until specific line
(gdb) until 25

# Continue until function returns
(gdb) finish

# Return from current function
(gdb) return 0

# Jump to specific line (dangerous!)
(gdb) jump 20
```

## Advanced Features

### Memory Examination
```bash
# Examine memory at address
(gdb) x/10x 0x7fffffffeab8    # 10 words in hex
(gdb) x/20c 0x7fffffffeab8    # 20 characters
(gdb) x/5i 0x4005f3           # 5 instructions

# Examine memory at variable address
(gdb) x/10x &array

# Memory formats: x (hex), d (decimal), u (unsigned), o (octal), t (binary), a (address), c (char), s (string), i (instruction)
```

### Assembly Code
```bash
# Show assembly for current function
(gdb) disassemble

# Show assembly for specific function
(gdb) disassemble main

# Show assembly around current line
(gdb) disassemble /m

# Show source and assembly
(gdb) disassemble /s
```

### Watchpoints
```bash
# Watch variable for changes
(gdb) watch variable_name

# Watch expression
(gdb) watch array[i]

# Watch memory location
(gdb) watch *0x7fffffffeab8

# List watchpoints
(gdb) info watchpoints

# Delete watchpoint
(gdb) delete 3  # if watchpoint is number 3
```

### Catchpoints
```bash
# Catch exceptions
(gdb) catch throw

# Catch system calls
(gdb) catch syscall open

# Catch function calls
(gdb) catch call malloc
```

## Debugging Strategies

### Systematic Debugging Approach

1. **Observe the Bug**
   - Reproduce the problem consistently
   - Note the exact symptoms and error messages

2. **Create Reproducible Input**
   - Develop minimal test cases
   - Document the steps to reproduce

3. **Narrow the Search Space**
   - Use binary search with breakpoints
   - Focus on recently changed code
   - Use Valgrind for memory errors

4. **Analyze the Problem**
   - Examine variable values at breakpoints
   - Trace execution flow
   - Draw diagrams if helpful

5. **Devise and Run Experiments**
   - Formulate hypotheses
   - Test with different inputs
   - Validate assumptions

6. **Fix and Validate**
   - Implement the fix
   - Test with original failing case
   - Ensure no regressions

### Memory Error Debugging
```bash
# Run with Valgrind first
$ valgrind --tool=memcheck ./program

# Then use GDB for detailed analysis
$ gdb program
(gdb) run

# When segfault occurs, examine the stack
(gdb) backtrace
(gdb) info registers
(gdb) x/10x $rsp  # Examine stack
```

### Performance Debugging
```bash
# Use Callgrind for profiling
$ valgrind --tool=callgrind ./program

# Use GDB to examine hot spots
(gdb) break expensive_function
(gdb) run
(gdb) continue  # Let it run, then examine state
```

## Common Scenarios

### Scenario 1: Segmentation Fault
```bash
$ gdb program
(gdb) run
Program received signal SIGSEGV, Segmentation fault.
0x0000000000400ac1 in read_frag (fp=0x603010, nread=0) at program.c:51

(gdb) backtrace
#0  0x0000000000400ac1 in read_frag (fp=0x603010, nread=0) at program.c:51
#1  0x0000000000400bd7 in read_all_frags (fp=0x603010, arr=0x7fffffff4cb0, maxfrags=5000) at program.c:69

(gdb) print fp
$1 = (FILE *) 0x603010

(gdb) print nread
$2 = 0

(gdb) info locals
# Examine local variables to understand the problem
```

### Scenario 2: Infinite Loop
```bash
(gdb) break main
(gdb) run
(gdb) next
# Step through the loop to find the issue
(gdb) print loop_variable
(gdb) print loop_condition
```

### Scenario 3: Wrong Output
```bash
(gdb) break function_that_produces_output
(gdb) run
(gdb) print input_variables
(gdb) step
(gdb) print intermediate_results
# Continue stepping to find where calculation goes wrong
```

### Scenario 4: Function Not Called
```bash
(gdb) break function_name
(gdb) run
# If breakpoint is never hit, the function isn't being called
(gdb) break main
(gdb) run
(gdb) step
# Step through main to see the execution path
```

## Example Debugging Session

### Debugging a Simple Program
```c
// buggy.c
#include <stdio.h>
#include <stdlib.h>

int square(int x) {
    int sq = x * x;
    return sq;
}

int main(int argc, char *argv[]) {
    printf("This program will square an integer.\n");
    
    if (argc != 2) {
        printf("Usage: %s number\n", argv[0]);
        return 1;
    }
    
    int num = atoi(argv[1]);
    int result = square(num);
    
    printf("%d squared is %d\n", num, result);
    return 0;
}
```

### GDB Session
```bash
$ gcc -g -Og -Wall buggy.c -o buggy
$ gdb buggy

(gdb) break main
Breakpoint 1 at 0x4005f3: file buggy.c, line 8

(gdb) run 5
Starting program: /path/to/buggy 5

Breakpoint 1, main (argc=2, argv=0x7fffffffeab8) at buggy.c:8
8   int main(int argc, char *argv[]) {

(gdb) next
9       printf("This program will square an integer.\n");

(gdb) next
This program will square an integer.
11      if (argc != 2) {

(gdb) next
16      int num = atoi(argv[1]);

(gdb) print argv[1]
$1 = 0x7fffffffecc5 "5"

(gdb) next
17      int result = square(num);

(gdb) print num
$2 = 5

(gdb) step
square (x=5) at buggy.c:4
4   int square(int x) {

(gdb) next
5       int sq = x * x;

(gdb) print x
$3 = 5

(gdb) next
6       return sq;

(gdb) print sq
$4 = 25

(gdb) finish
Run till exit from #0  square (x=5) at buggy.c:6
main (argc=2, argv=0x7fffffffeab8) at buggy.c:18
Value returned is $5 = 25

(gdb) next
18      printf("%d squared is %d\n", num, result);

(gdb) print result
$6 = 25

(gdb) continue
Continuing.
5 squared is 25
[Inferior 1 (process 12345) exited normally]

(gdb) quit
```

## Advanced Debugging Examples

### Debugging a Linked List
```c
// linked_list.c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

Node* create_node(int data) {
    Node *node = malloc(sizeof(Node));
    if (!node) {
        fprintf(stderr, "Memory allocation failed\n");
        return NULL;
    }
    node->data = data;
    node->next = NULL;
    return node;
}

void insert_at_end(Node **head, int data) {
    Node *new_node = create_node(data);
    if (!new_node) return;
    
    if (*head == NULL) {
        *head = new_node;
        return;
    }
    
    Node *current = *head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = new_node;
}

void print_list(Node *head) {
    Node *current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

void free_list(Node *head) {
    Node *current = head;
    while (current != NULL) {
        Node *temp = current;
        current = current->next;
        free(temp);
    }
}

int main() {
    Node *head = NULL;
    
    insert_at_end(&head, 10);
    insert_at_end(&head, 20);
    insert_at_end(&head, 30);
    
    printf("Linked list: ");
    print_list(head);
    
    free_list(head);
    return 0;
}
```

### GDB Session for Linked List
```bash
$ gcc -g -O0 -Wall linked_list.c -o linked_list
$ gdb linked_list

(gdb) break create_node
Breakpoint 1 at 0x4005f3: file linked_list.c, line 10

(gdb) break insert_at_end
Breakpoint 2 at 0x40062a: file linked_list.c, line 20

(gdb) run
Starting program: /path/to/linked_list

Breakpoint 2, insert_at_end (head=0x7fffffffeab8, data=10) at linked_list.c:20
20  void insert_at_end(Node **head, int data) {

(gdb) print *head
$1 = (Node *) 0x0

(gdb) next
21      Node *new_node = create_node(data);

(gdb) step
create_node (data=10) at linked_list.c:10
10  Node* create_node(int data) {

(gdb) next
11      Node *node = malloc(sizeof(Node));

(gdb) next
12      if (!node) {

(gdb) print node
$2 = (Node *) 0x603010

(gdb) next
16      node->data = data;

(gdb) print node->data
$3 = 0

(gdb) next
17      node->next = NULL;

(gdb) print *node
$4 = {data = 10, next = 0x0}

(gdb) finish
Run till exit from #0  create_node (data=10) at linked_list.c:18
insert_at_end (head=0x7fffffffeab8, data=10) at linked_list.c:22
Value returned is $5 = (Node *) 0x603010

(gdb) continue
Continuing.
Linked list: 10 -> 20 -> 30 -> NULL
[Inferior 1 (process 12346) exited normally]
```

### Debugging a Recursive Function
```c
// factorial.c
#include <stdio.h>
#include <stdlib.h>

int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <number>\n", argv[0]);
        return 1;
    }
    
    int n = atoi(argv[1]);
    if (n < 0) {
        printf("Error: Factorial is not defined for negative numbers\n");
        return 1;
    }
    
    int result = factorial(n);
    printf("%d! = %d\n", n, result);
    
    return 0;
}
```

### GDB Session for Recursive Function
```bash
$ gcc -g -O0 -Wall factorial.c -o factorial
$ gdb factorial

(gdb) break factorial
Breakpoint 1 at 0x4005f3: file factorial.c, line 5

(gdb) run 5
Starting program: /path/to/factorial 5

Breakpoint 1, factorial (n=5) at factorial.c:5
5   int factorial(int n) {

(gdb) print n
$1 = 5

(gdb) next
6       if (n <= 1) {

(gdb) next
9       return n * factorial(n - 1);

(gdb) step
factorial (n=4) at factorial.c:5
5   int factorial(int n) {

(gdb) bt
#0  factorial (n=4) at factorial.c:5
#1  0x0000000000400612 in factorial (n=5) at factorial.c:9
#2  0x0000000000400634 in main (argc=2, argv=0x7fffffffeab8) at factorial.c:20

(gdb) continue
Continuing.
5! = 120
[Inferior 1 (process 12347) exited normally]
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "No debugging symbols found"
```bash
# Problem: Program compiled without debug info
$ gdb program
Reading symbols from program...(no debugging symbols found)...done.

# Solution: Recompile with -g flag
$ gcc -g -Og source.c -o program
```

#### 2. "Source file is more recent than executable"
```bash
# Problem: Source code changed but not recompiled
(gdb) list
warning: Source file is more recent than executable.

# Solution: Recompile from within GDB
(gdb) make
# Or quit and recompile
(gdb) quit
$ make
$ gdb program
```

#### 3. "No symbol in current context"
```bash
# Problem: Variable not accessible in current scope
(gdb) print variable
No symbol "variable" in current context.

# Solution: Check current function and scope
(gdb) info locals
(gdb) info args
(gdb) backtrace
```

#### 4. "Optimized out"
```bash
# Problem: Variable optimized away by compiler
(gdb) print variable
$1 = <optimized out>

# Solution: Use -Og instead of -O2/-O3 for debugging
$ gcc -g -Og source.c -o program
```

#### 5. Auto-loading declined
```bash
# Problem: GDB won't load local .gdbinit
warning: File ".gdbinit" auto-loading has been declined...

# Solution: Configure auto-load safe path
$ echo "set auto-load safe-path /" >> ~/.gdbinit
```

### Verbose Output and Debugging GDB
```bash
# Show detailed GDB operations
(gdb) set verbose on

# Show all commands being executed
(gdb) set trace-commands on

# Show assembly instructions
(gdb) set disassemble-next-line on
```

## Best Practices

### 1. Compile with Debug Information
```bash
# Always use these flags for debugging
$ gcc -g -Og -Wall -std=gnu99 source.c -o program
```

### 2. Use Meaningful Breakpoints
```bash
# Good: Break at function entry
(gdb) break main
(gdb) break process_data

# Good: Break at specific conditions
(gdb) break 25 if count > 100

# Avoid: Breaking at every line
(gdb) break 1
(gdb) break 2
(gdb) break 3
```

### 3. Examine State Systematically
```bash
# When stopped at breakpoint:
(gdb) backtrace          # Understand call stack
(gdb) info args          # Check function arguments
(gdb) info locals        # Check local variables
(gdb) print key_vars     # Examine important variables
```

### 4. Use Conditional Breakpoints
```bash
# Break only when condition is met
(gdb) break function if size > 1000
(gdb) break 15 if i == 42
```

### 5. Document Your Debugging Session
```bash
# Use GDB's logging feature
(gdb) set logging on
(gdb) set logging file debug.log
(gdb) set logging redirect on
```

### 6. Combine with Other Tools
```bash
# Use Valgrind for memory errors
$ valgrind --tool=memcheck ./program

# Use GDB for detailed analysis
$ gdb program

# Use strace for system call tracing
$ strace ./program
```

## Additional Resources

- [GDB Official Documentation](https://sourceware.org/gdb/documentation/)
- [GDB Reference Card](https://sourceware.org/gdb/current/onlinedocs/gdb/refcard.pdf)
- [GDB Manual](https://sourceware.org/gdb/current/onlinedocs/gdb/)
- [Stanford Unix Programming Tools](https://web.stanford.edu/class/archive/cs/cs107/cs107.1194/resources/unix_tools)
- [GDB's Greatest Hits](https://web.stanford.edu/class/archive/cs/cs107/cs107.1194/resources/gdb)

---

*This guide covers the essential aspects of using GDB for debugging C programs. For more advanced topics like remote debugging, core dump analysis, or debugging multi-threaded applications, refer to the official GDB documentation.* 