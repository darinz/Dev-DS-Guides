# GCC (GNU Compiler Collection) Complete Guide

[![GCC](https://img.shields.io/badge/GCC-11.3+-green.svg)](https://gcc.gnu.org/)
[![C](https://img.shields.io/badge/C-99%2F11%2F17-blue.svg)](https://gcc.gnu.org/onlinedocs/)
[![C++](https://img.shields.io/badge/C%2B%2B-11%2F14%2F17%2F20-orange.svg)](https://gcc.gnu.org/onlinedocs/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](../LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen.svg)]()

## Table of Contents
- [Introduction](#introduction)
- [Basic Compilation](#basic-compilation)
- [Compiler Flags and Options](#compiler-flags-and-options)
- [Optimization Levels](#optimization-levels)
- [Debugging Support](#debugging-support)
- [Language Standards](#language-standards)
- [Advanced Features](#advanced-features)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

## Introduction

The **GNU Compiler Collection (GCC)** is one of the most widely used compilers in the world. It's free software and available on many different computing platforms. GCC performs the compilation step to build a program, then calls other programs to assemble and link the program's components into an executable.

### What GCC Does
1. **Preprocessing**: Expands macros and includes
2. **Compilation**: Converts source code to assembly
3. **Assembly**: Converts assembly to object files
4. **Linking**: Combines object files into executable

## Basic Compilation

### Simple Compilation
The simplest way to compile a C program:

```bash
$ gcc hello.c
```

This creates an executable named `a.out` in the current directory.

### Specifying Output Name
Use the `-o` flag to specify the output executable name:

```bash
$ gcc hello.c -o hello
$ ./hello
Hello, World!
```

### Compiling Multiple Source Files
```bash
$ gcc file1.c file2.c file3.c -o program
```

### Important Notes
- **Don't include header files (.h) in the gcc command** - they're automatically included via `#include` statements
- **Be careful with output names** - avoid using the same name as your source file:
  ```bash
  # ❌ DON'T DO THIS - could overwrite your source file
  $ gcc hello.c -o hello.c
  
  # ✅ DO THIS INSTEAD
  $ gcc hello.c -o hello
  ```

## Compiler Flags and Options

### Essential Flags

| Flag | Description | Example |
|------|-------------|---------|
| `-o filename` | Specify output filename | `gcc -o myprog source.c` |
| `-Wall` | Enable all common warnings | `gcc -Wall source.c` |
| `-Wextra` | Enable extra warnings | `gcc -Wextra source.c` |
| `-std=standard` | Specify C standard | `gcc -std=c99 source.c` |
| `-g` | Include debugging information | `gcc -g source.c` |
| `-Olevel` | Set optimization level | `gcc -O2 source.c` |

### Warning Flags
```bash
# Enable all warnings
$ gcc -Wall -Wextra -Werror source.c

# Treat warnings as errors
$ gcc -Werror source.c

# Specific warnings
$ gcc -Wunused-variable -Wunused-function source.c
```

## Optimization Levels

GCC provides several optimization levels to balance compilation speed, code size, and execution performance:

### Optimization Options

| Level | Description | Use Case |
|-------|-------------|----------|
| `-O0` | No optimization (default) | Debugging, fastest compilation |
| `-O1` | Basic optimization | General development |
| `-O2` | More aggressive optimization | Production builds |
| `-O3` | Maximum optimization | Performance-critical code |
| `-Os` | Optimize for size | Embedded systems |
| `-Ofast` | Disregard standards compliance | Maximum performance |
| `-Og` | Optimize for debugging | Debug builds |

### Detailed Examples

```bash
# Debug build - no optimization, with debug info
$ gcc -g -O0 -Wall source.c -o debug_program

# Development build - light optimization
$ gcc -g -O1 -Wall source.c -o dev_program

# Production build - aggressive optimization
$ gcc -O2 -Wall source.c -o release_program

# Size-optimized build
$ gcc -Os -Wall source.c -o small_program

# Debug-friendly optimization
$ gcc -g -Og -Wall source.c -o debug_optimized
```

## Debugging Support

### Including Debug Information
The `-g` flag includes debugging information that allows tools like GDB to provide detailed debugging capabilities:

```bash
# Basic debug info
$ gcc -g source.c -o program

# Enhanced debug info
$ gcc -g3 source.c -o program

# Debug info with optimization (use -Og for best debugging experience)
$ gcc -g -Og source.c -o program
```

### Debug Levels
- `-g`: Basic debug information
- `-g1`: Minimal debug information
- `-g2`: Standard debug information (same as `-g`)
- `-g3`: Maximum debug information (includes macro definitions)

## Language Standards

### C Standards
GCC supports multiple C language standards:

```bash
# C89/C90 (ANSI C)
$ gcc -std=c89 source.c

# C99
$ gcc -std=c99 source.c

# C11
$ gcc -std=c11 source.c

# C17/C18
$ gcc -std=c17 source.c

# GNU extensions to C99 (recommended for CS107)
$ gcc -std=gnu99 source.c
```

### Why Use GNU99?
The `-std=gnu99` flag enables GNU extensions to the C99 standard, including:
- Variable declarations in for loops: `for (int i = 0; i < n; i++)`
- Additional built-in functions
- Extended syntax features

## Advanced Features

### Preprocessor Options
```bash
# Define a macro
$ gcc -DDEBUG source.c

# Define a macro with value
$ gcc -DVERSION=1.0 source.c

# Include additional directories
$ gcc -I/path/to/headers source.c

# Preprocess only (don't compile)
$ gcc -E source.c
```

### Linking Options
```bash
# Link with specific libraries
$ gcc source.c -lm -lpthread

# Link with libraries in specific directories
$ gcc source.c -L/path/to/libs -lmylib

# Create shared library
$ gcc -shared -fPIC source.c -o libmylib.so

# Create static library
$ gcc -c source.c
$ ar rcs libmylib.a source.o
```

### Architecture and Platform Options
```bash
# Specify target architecture
$ gcc -march=native source.c

# Optimize for specific CPU
$ gcc -march=x86-64 -mtune=generic source.c

# Cross-compilation
$ gcc -target arm-linux-gnueabi source.c
```

## Common Use Cases

### 1. Simple Program Compilation
```bash
# Basic compilation
$ gcc hello.c -o hello
$ ./hello
```

### 2. Multi-file Project
```bash
# Compile multiple source files
$ gcc main.c utils.c math.c -o myprogram
$ ./myprogram
```

### 3. Library Development
```bash
# Create object file
$ gcc -c mylib.c -o mylib.o

# Create static library
$ ar rcs libmylib.a mylib.o

# Link against library
$ gcc main.c -L. -lmylib -o program
```

### 4. Debug Build
```bash
# Debug build with all warnings
$ gcc -g -Og -Wall -Wextra source.c -o debug_program
```

### 5. Release Build
```bash
# Optimized release build
$ gcc -O2 -Wall -DNDEBUG source.c -o release_program
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "No such file or directory" for headers
```bash
# Solution: Use -I flag to specify include path
$ gcc -I/path/to/headers source.c
```

#### 2. "Undefined reference" errors
```bash
# Solution: Link required libraries
$ gcc source.c -lm -lpthread
```

#### 3. Warnings about implicit function declarations
```bash
# Solution: Include proper headers or use -std=c99
$ gcc -std=c99 source.c
```

#### 4. Source file overwritten
```bash
# Problem: gcc hello.c -o hello.c
# Solution: Use different output name
$ gcc hello.c -o hello
```

### Verbose Output
```bash
# Show detailed compilation steps
$ gcc -v source.c

# Show preprocessor output
$ gcc -E source.c

# Show assembly output
$ gcc -S source.c
```

## Best Practices

### 1. Always Use Warning Flags
```bash
# Recommended compilation command
$ gcc -Wall -Wextra -std=gnu99 source.c -o program
```

### 2. Use Appropriate Optimization Levels
- **Development**: `-Og` for debugging-friendly optimization
- **Testing**: `-O1` or `-O2` for performance testing
- **Production**: `-O2` or `-O3` for maximum performance

### 3. Include Debug Information During Development
```bash
$ gcc -g -Og -Wall -std=gnu99 source.c -o program
```

### 4. Use Meaningful Output Names
```bash
# Good
$ gcc main.c -o calculator

# Avoid
$ gcc main.c  # Creates a.out
```

### 5. Organize Multi-file Projects
```bash
# Compile each file separately
$ gcc -c file1.c -o file1.o
$ gcc -c file2.c -o file2.o
$ gcc file1.o file2.o -o program
```

### 6. Use Makefiles for Complex Projects
For projects with multiple files, consider using Makefiles to automate the build process.

## Example Projects

### Simple Calculator
```c
// calc.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]) {
    if (argc != 4) {
        printf("Usage: %s <num1> <operator> <num2>\n", argv[0]);
        return 1;
    }
    
    double a = atof(argv[1]);
    double b = atof(argv[3]);
    char op = argv[2][0];
    
    switch (op) {
        case '+': printf("%.2f\n", a + b); break;
        case '-': printf("%.2f\n", a - b); break;
        case '*': printf("%.2f\n", a * b); break;
        case '/': 
            if (b == 0) {
                printf("Error: Division by zero\n");
                return 1;
            }
            printf("%.2f\n", a / b); 
            break;
        default: printf("Unknown operator\n"); return 1;
    }
    
    return 0;
}
```

Compilation:
```bash
$ gcc -Wall -std=gnu99 calc.c -o calc
$ ./calc 5 + 3
8.00
$ ./calc 10 / 2
5.00
$ ./calc 5 / 0
Error: Division by zero
```

### Multi-file Project Structure
```
project/
├── src/
│   ├── main.c
│   ├── utils.c
│   └── math.c
├── include/
│   ├── utils.h
│   └── math.h
└── Makefile
```

Example source files:

```c
// include/utils.h
#ifndef UTILS_H
#define UTILS_H

void print_help(void);
int validate_input(const char *input);

#endif
```

```c
// src/utils.c
#include <stdio.h>
#include <string.h>
#include "../include/utils.h"

void print_help(void) {
    printf("Usage: program <input_file> <output_file>\n");
    printf("Options:\n");
    printf("  -h, --help    Show this help message\n");
    printf("  -v, --verbose Enable verbose output\n");
}

int validate_input(const char *input) {
    if (!input || strlen(input) == 0) {
        return 0;
    }
    return 1;
}
```

```c
// src/main.c
#include <stdio.h>
#include <stdlib.h>
#include "../include/utils.h"
#include "../include/math.h"

int main(int argc, char *argv[]) {
    if (argc < 2) {
        print_help();
        return 1;
    }
    
    if (!validate_input(argv[1])) {
        printf("Error: Invalid input\n");
        return 1;
    }
    
    // Process input
    printf("Processing: %s\n", argv[1]);
    
    return 0;
}
```

Compilation:
```bash
$ gcc -Iinclude -Wall -std=gnu99 src/*.c -o program
$ ./program input.txt
Processing: input.txt
```

### Library Development Example
```c
// libmath.h
#ifndef LIBMATH_H
#define LIBMATH_H

// Basic math operations
double add(double a, double b);
double subtract(double a, double b);
double multiply(double a, double b);
double divide(double a, double b);

// Advanced operations
double power(double base, double exponent);
double sqrt(double x);

#endif
```

```c
// libmath.c
#include "libmath.h"
#include <math.h>

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    if (b == 0) {
        return 0.0; // Handle division by zero
    }
    return a / b;
}

double power(double base, double exponent) {
    return pow(base, exponent);
}

double sqrt(double x) {
    if (x < 0) {
        return 0.0; // Handle negative numbers
    }
    return sqrt(x);
}
```

```c
// test_math.c
#include <stdio.h>
#include "libmath.h"

int main() {
    printf("Testing math library:\n");
    printf("5 + 3 = %.2f\n", add(5, 3));
    printf("10 - 4 = %.2f\n", subtract(10, 4));
    printf("6 * 7 = %.2f\n", multiply(6, 7));
    printf("15 / 3 = %.2f\n", divide(15, 3));
    printf("2^8 = %.2f\n", power(2, 8));
    printf("sqrt(16) = %.2f\n", sqrt(16));
    
    return 0;
}
```

Compilation and testing:
```bash
# Create object file
$ gcc -c -Wall libmath.c -o libmath.o

# Create static library
$ ar rcs libmath.a libmath.o

# Compile test program
$ gcc -Wall test_math.c -L. -lmath -lm -o test_math

# Run test
$ ./test_math
Testing math library:
5 + 3 = 8.00
10 - 4 = 6.00
6 * 7 = 42.00
15 / 3 = 5.00
2^8 = 256.00
sqrt(16) = 4.00
```

## Additional Resources

- [GCC Official Documentation](https://gcc.gnu.org/onlinedocs/)
- [GCC Manual](https://gcc.gnu.org/onlinedocs/gcc/)
- [GCC Command Options](https://gcc.gnu.org/onlinedocs/gcc/Option-Index.html)
- [Stanford Unix Programming Tools](https://web.stanford.edu/class/archive/cs/cs107/cs107.1194/resources/unix_tools)

---

*This guide covers the essential aspects of using GCC for C programming. For more advanced topics like cross-compilation, plugin development, or specific optimizations, refer to the official GCC documentation.* 