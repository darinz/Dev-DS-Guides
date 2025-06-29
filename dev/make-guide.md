# Make Complete Guide

[![Make](https://img.shields.io/badge/Make-4.3+-orange.svg)](https://www.gnu.org/software/make/)
[![Build](https://img.shields.io/badge/Build-Automation-blue.svg)](https://www.gnu.org/software/make/manual/)
[![C](https://img.shields.io/badge/C-Build-green.svg)](https://www.gnu.org/software/make/)
[![C++](https://img.shields.io/badge/C%2B%2B-Build-orange.svg)](https://www.gnu.org/software/make/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](../LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen.svg)]()

## Table of Contents
- [Introduction](#introduction)
- [Basic Concepts](#basic-concepts)
- [Simple Makefiles](#simple-makefiles)
- [Advanced Makefiles](#advanced-makefiles)
- [Variables and Macros](#variables-and-macros)
- [Dependencies](#dependencies)
- [Phony Targets](#phony-targets)
- [Pattern Rules](#pattern-rules)
- [Functions](#functions)
- [Conditionals](#conditionals)
- [Common Use Cases](#common-use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Introduction

**Make** is a build automation tool that automatically builds executable programs and libraries from source code by reading files called **Makefiles** which specify how to derive the target program. Make was originally created in 1976 and is still widely used today.

### Why Use Make?
- **Automation**: Automates the build process
- **Dependency Management**: Only rebuilds what's necessary
- **Consistency**: Ensures consistent builds across environments
- **Efficiency**: Saves time by avoiding unnecessary recompilation
- **Portability**: Works across different platforms

### What Make Does
1. **Reads Makefile**: Parses the build specification
2. **Checks Dependencies**: Determines what needs to be rebuilt
3. **Executes Commands**: Runs the necessary compilation commands
4. **Updates Timestamps**: Tracks when files were last modified

## Basic Concepts

### Makefile Structure
A Makefile consists of **rules** with the following format:

```makefile
target: dependencies
    command
```

- **target**: The file to be created
- **dependencies**: Files that the target depends on
- **command**: The command to create the target (must be indented with a tab)

### Basic Rule Example
```makefile
hello: hello.c
    gcc -o hello hello.c
```

This rule says: "To create `hello`, you need `hello.c`, and the command to do this is `gcc -o hello hello.c`"

## Simple Makefiles

### Basic Single-File Project
```makefile
# Simple Makefile for hello.c
CC = gcc
CFLAGS = -g -Wall -std=gnu99

hello: hello.c
    $(CC) $(CFLAGS) -o hello hello.c

clean:
    rm -f hello
```

### Multi-File Project
```makefile
# Makefile for a project with multiple source files
CC = gcc
CFLAGS = -g -Wall -std=gnu99

# Target executable
program: main.o utils.o math.o
    $(CC) $(CFLAGS) -o program main.o utils.o math.o

# Object files
main.o: main.c
    $(CC) $(CFLAGS) -c main.c

utils.o: utils.c utils.h
    $(CC) $(CFLAGS) -c utils.c

math.o: math.c math.h
    $(CC) $(CFLAGS) -c math.c

clean:
    rm -f program *.o
```

### Using Make
```bash
# Build the default target (first target in Makefile)
$ make

# Build a specific target
$ make program

# Clean build artifacts
$ make clean

# Show what would be done without executing
$ make -n

# Force rebuild everything
$ make clean && make
```

## Advanced Makefiles

### Complete Project Makefile
```makefile
# Comprehensive Makefile for a C project

# Compiler and flags
CC = gcc
CFLAGS = -g -Wall -O2 -std=gnu99
LDFLAGS = -lm

# Directories
SRCDIR = src
INCDIR = include
OBJDIR = obj
BINDIR = bin

# Source files
SOURCES = $(wildcard $(SRCDIR)/*.c)
OBJECTS = $(SOURCES:$(SRCDIR)/%.c=$(OBJDIR)/%.o)
TARGET = $(BINDIR)/program

# Default target
all: $(TARGET)

# Create directories
$(OBJDIR):
    mkdir -p $(OBJDIR)

$(BINDIR):
    mkdir -p $(BINDIR)

# Link the program
$(TARGET): $(OBJECTS) | $(BINDIR)
    $(CC) $(OBJECTS) -o $@ $(LDFLAGS)

# Compile object files
$(OBJDIR)/%.o: $(SRCDIR)/%.c | $(OBJDIR)
    $(CC) $(CFLAGS) -I$(INCDIR) -c $< -o $@

# Phony targets
.PHONY: all clean install uninstall

clean:
    rm -rf $(OBJDIR) $(BINDIR)

install: $(TARGET)
    cp $(TARGET) /usr/local/bin/

uninstall:
    rm -f /usr/local/bin/program
```

## Variables and Macros

### Variable Assignment
```makefile
# Simple assignment
CC = gcc

# Recursive assignment (evaluated each time)
CFLAGS := -g -Wall

# Conditional assignment (only if not already set)
LDFLAGS ?= -lm

# Append to variable
CFLAGS += -std=gnu99
```

### Automatic Variables
```makefile
program: main.o utils.o
    $(CC) $^ -o $@

# $@ = target name
# $< = first dependency
# $^ = all dependencies
# $* = stem (filename without extension)
# $? = dependencies newer than target
```

### Variable Examples
```makefile
# Define source files
SOURCES = main.c utils.c math.c

# Derive object files from sources
OBJECTS = $(SOURCES:.c=.o)

# Define target
TARGET = program

# Build target from objects
$(TARGET): $(OBJECTS)
    $(CC) $^ -o $@

# Compile each source file
%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@
```

## Dependencies

### Header File Dependencies
```makefile
# Explicit dependencies
main.o: main.c main.h utils.h
    $(CC) $(CFLAGS) -c main.c

utils.o: utils.c utils.h
    $(CC) $(CFLAGS) -c utils.c

# Or use automatic dependency generation
%.o: %.c
    $(CC) $(CFLAGS) -MMD -c $< -o $@
    @cp $*.d $*.P; \
    sed -e 's/#.*//' -e 's/^[^:]*: *//' -e 's/ *\\$$//' \
        -e '/^$$/ d' -e 's/$$/ :/' < $*.P >> $*.d; \
    rm -f $*.P

-include *.d
```

### Implicit Rules
Make has built-in rules for common file types:

```makefile
# These rules are built into Make
%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

%.o: %.cpp
    $(CXX) $(CXXFLAGS) -c $< -o $@
```

### Dependency Examples
```makefile
# Simple dependency chain
program: main.o utils.o
    $(CC) $^ -o $@

main.o: main.c main.h
    $(CC) $(CFLAGS) -c main.c

utils.o: utils.c utils.h
    $(CC) $(CFLAGS) -c utils.c

# Header file changes trigger recompilation
main.h: config.h
    # Update main.h if config.h changes
```

## Phony Targets

### Common Phony Targets
```makefile
.PHONY: all clean install uninstall test help

# Default target
all: program

# Clean build artifacts
clean:
    rm -f *.o program

# Install the program
install: program
    cp program /usr/local/bin/

# Uninstall the program
uninstall:
    rm -f /usr/local/bin/program

# Run tests
test: program
    ./program --test

# Show help
help:
    @echo "Available targets:"
    @echo "  all       - Build the program"
    @echo "  clean     - Remove build artifacts"
    @echo "  install   - Install the program"
    @echo "  uninstall - Remove the program"
    @echo "  test      - Run tests"
    @echo "  help      - Show this help"
```

### Why Use .PHONY?
- Prevents conflicts with files named `clean`, `install`, etc.
- Ensures the target always runs, even if a file with the same name exists
- Makes the Makefile more robust

## Pattern Rules

### Basic Pattern Rules
```makefile
# Compile all .c files to .o files
%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

# Compile all .cpp files to .o files
%.o: %.cpp
    $(CXX) $(CXXFLAGS) -c $< -o $@

# Create executables from .o files
%: %.o
    $(CC) $< -o $@
```

### Advanced Pattern Rules
```makefile
# Compile with different flags for different directories
$(OBJDIR)/%.o: $(SRCDIR)/%.c
    $(CC) $(CFLAGS) -I$(INCDIR) -c $< -o $@

$(TESTDIR)/%.o: $(TESTDIR)/%.c
    $(CC) $(CFLAGS) -DTEST -c $< -o $@

# Handle different source directories
$(OBJDIR)/%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

$(OBJDIR)/%.o: $(SRCDIR)/%.c
    $(CC) $(CFLAGS) -c $< -o $@
```

## Functions

### Built-in Functions
```makefile
# String functions
SOURCES = main.c utils.c math.c
OBJECTS = $(SOURCES:.c=.o)                    # Substitution
DIRS = $(dir $(SOURCES))                      # Directory names
BASENAMES = $(notdir $(SOURCES))              # Base names
UPPER = $(shell echo $(SOURCES) | tr a-z A-Z) # Shell command

# File functions
EXISTING = $(wildcard *.c)                    # Existing files
ALL_C = $(wildcard *.c)                       # All .c files
ALL_H = $(wildcard *.h)                       # All .h files

# Conditional functions
DEBUG_OBJS = $(if $(DEBUG),$(OBJECTS),)       # Conditional objects
```

### Custom Functions
```makefile
# Define a function to add prefix
add_prefix = $(addprefix $(1),$(2))

# Use the function
INCLUDES = -Iinclude -Isrc
CFLAGS = $(call add_prefix,-I,$(INCLUDES))

# Function to find source files
find_sources = $(wildcard $(1)/*.c)
SOURCES = $(call find_sources,src)
```

## Conditionals

### Basic Conditionals
```makefile
# Check if DEBUG is set
ifdef DEBUG
    CFLAGS += -DDEBUG -g
else
    CFLAGS += -O2
endif

# Check if variable is empty
ifneq ($(TARGET),)
    INSTALL_DIR = /usr/local/bin
else
    INSTALL_DIR = ./bin
endif

# String comparison
ifeq ($(CC),gcc)
    CFLAGS += -std=gnu99
else
    CFLAGS += -std=c99
endif
```

### Advanced Conditionals
```makefile
# Check operating system
ifeq ($(OS),Windows_NT)
    RM = del /Q
    EXE = .exe
else
    RM = rm -f
    EXE =
endif

# Check architecture
ifeq ($(shell uname -m),x86_64)
    CFLAGS += -m64
else
    CFLAGS += -m32
endif

# Conditional compilation
ifdef RELEASE
    CFLAGS += -O3 -DNDEBUG
    LDFLAGS += -s
else
    CFLAGS += -g -DDEBUG
endif
```

## Common Use Cases

### 1. Simple C Project
```makefile
# Simple C project Makefile
CC = gcc
CFLAGS = -g -Wall -std=gnu99
TARGET = myprogram
SOURCES = main.c utils.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
    $(CC) $(OBJECTS) -o $(TARGET)

%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

clean:
    rm -f $(OBJECTS) $(TARGET)

.PHONY: clean
```

### 2. Library Project
```makefile
# Library project Makefile
CC = gcc
CFLAGS = -g -Wall -std=gnu99 -fPIC
AR = ar
RANLIB = ranlib

LIBNAME = libmylib.a
SOURCES = lib1.c lib2.c lib3.c
OBJECTS = $(SOURCES:.c=.o)

$(LIBNAME): $(OBJECTS)
    $(AR) rcs $(LIBNAME) $(OBJECTS)
    $(RANLIB) $(LIBNAME)

%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

clean:
    rm -f $(OBJECTS) $(LIBNAME)

.PHONY: clean
```

### 3. Multi-Platform Project
```makefile
# Multi-platform Makefile
CC = gcc
CFLAGS = -g -Wall -std=gnu99

# Detect operating system
ifeq ($(OS),Windows_NT)
    TARGET = program.exe
    RM = del /Q
else
    TARGET = program
    RM = rm -f
endif

# Detect architecture
ifeq ($(shell uname -m),x86_64)
    CFLAGS += -m64
else
    CFLAGS += -m32
endif

SOURCES = main.c utils.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
    $(CC) $(OBJECTS) -o $(TARGET)

%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

clean:
    $(RM) $(OBJECTS) $(TARGET)

.PHONY: clean
```

### 4. Project with Tests
```makefile
# Project with tests
CC = gcc
CFLAGS = -g -Wall -std=gnu99
TEST_CFLAGS = $(CFLAGS) -DTEST

PROGRAM = myprogram
TEST_PROGRAM = test_program

SOURCES = main.c utils.c
TEST_SOURCES = test_main.c utils.c
OBJECTS = $(SOURCES:.c=.o)
TEST_OBJECTS = $(TEST_SOURCES:.c=.o)

all: $(PROGRAM) $(TEST_PROGRAM)

$(PROGRAM): $(OBJECTS)
    $(CC) $(OBJECTS) -o $(PROGRAM)

$(TEST_PROGRAM): $(TEST_OBJECTS)
    $(CC) $(TEST_OBJECTS) -o $(TEST_PROGRAM)

%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

test_%.o: test_%.c
    $(CC) $(TEST_CFLAGS) -c $< -o $@

test: $(TEST_PROGRAM)
    ./$(TEST_PROGRAM)

clean:
    rm -f *.o $(PROGRAM) $(TEST_PROGRAM)

.PHONY: all test clean
```

## Advanced Makefile Examples

### Complete Project with Multiple Targets
```makefile
# Advanced project Makefile
CC = gcc
CXX = g++
CFLAGS = -g -Wall -std=gnu99
CXXFLAGS = -g -Wall -std=c++11
LDFLAGS = -lm

# Directories
SRCDIR = src
INCDIR = include
OBJDIR = obj
BINDIR = bin
TESTDIR = tests
DOCDIR = docs

# Files
SOURCES = $(wildcard $(SRCDIR)/*.c)
CXXSOURCES = $(wildcard $(SRCDIR)/*.cpp)
OBJECTS = $(SOURCES:$(SRCDIR)/%.c=$(OBJDIR)/%.o)
CXXOBJECTS = $(CXXSOURCES:$(SRCDIR)/%.cpp=$(OBJDIR)/%.o)
TARGET = $(BINDIR)/program

# Test files
TEST_SOURCES = $(wildcard $(TESTDIR)/*.c)
TEST_OBJECTS = $(TEST_SOURCES:$(TESTDIR)/%.c=$(OBJDIR)/test_%.o)
TEST_TARGET = $(BINDIR)/test_program

# Documentation
DOCS = $(wildcard $(DOCDIR)/*.md)

# Default target
all: $(TARGET) $(TEST_TARGET)

# Create directories
$(OBJDIR) $(BINDIR):
    mkdir -p $@

# Main program
$(TARGET): $(OBJECTS) $(CXXOBJECTS) | $(BINDIR)
    $(CC) $^ -o $@ $(LDFLAGS)

# C object files
$(OBJDIR)/%.o: $(SRCDIR)/%.c | $(OBJDIR)
    $(CC) $(CFLAGS) -I$(INCDIR) -c $< -o $@

# C++ object files
$(OBJDIR)/%.o: $(SRCDIR)/%.cpp | $(OBJDIR)
    $(CXX) $(CXXFLAGS) -I$(INCDIR) -c $< -o $@

# Test program
$(TEST_TARGET): $(TEST_OBJECTS) $(filter-out $(OBJDIR)/main.o,$(OBJECTS)) | $(BINDIR)
    $(CC) $^ -o $@ $(LDFLAGS)

# Test object files
$(OBJDIR)/test_%.o: $(TESTDIR)/%.c | $(OBJDIR)
    $(CC) $(CFLAGS) -I$(INCDIR) -DTEST -c $< -o $@

# Documentation
docs: $(DOCS)
    @echo "Generating documentation..."
    # Add documentation generation commands here

# Phony targets
.PHONY: all clean test install uninstall help docs

test: $(TEST_TARGET)
    ./$(TEST_TARGET)

clean:
    rm -rf $(OBJDIR) $(BINDIR)

install: $(TARGET)
    cp $(TARGET) /usr/local/bin/

uninstall:
    rm -f /usr/local/bin/program

help:
    @echo "Available targets:"
    @echo "  all       - Build the program"
    @echo "  test      - Build and run tests"
    @echo "  clean     - Remove build artifacts"
    @echo "  install   - Install the program"
    @echo "  uninstall - Remove the program"
    @echo "  docs      - Generate documentation"
    @echo "  help      - Show this help"
```

### Example Source Files for the Advanced Project

```c
// include/utils.h
#ifndef UTILS_H
#define UTILS_H

#include <stdio.h>
#include <stdlib.h>

// Utility functions
void print_help(void);
int validate_input(const char *input);
void log_message(const char *message);
int file_exists(const char *filename);

// Error handling
void handle_error(const char *message);
void cleanup_resources(void);

#endif
```

```c
// src/utils.c
#include "utils.h"
#include <string.h>
#include <unistd.h>

void print_help(void) {
    printf("Usage: program [options] <input_file> <output_file>\n");
    printf("Options:\n");
    printf("  -h, --help    Show this help message\n");
    printf("  -v, --verbose Enable verbose output\n");
    printf("  -d, --debug   Enable debug mode\n");
}

int validate_input(const char *input) {
    if (!input || strlen(input) == 0) {
        return 0;
    }
    return 1;
}

void log_message(const char *message) {
    printf("[LOG] %s\n", message);
}

int file_exists(const char *filename) {
    return access(filename, F_OK) == 0;
}

void handle_error(const char *message) {
    fprintf(stderr, "ERROR: %s\n", message);
    cleanup_resources();
    exit(1);
}

void cleanup_resources(void) {
    // Cleanup code here
    printf("Cleaning up resources...\n");
}
```

```c
// src/main.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "utils.h"

int main(int argc, char *argv[]) {
    if (argc < 2) {
        print_help();
        return 1;
    }
    
    // Parse command line arguments
    int verbose = 0;
    int debug = 0;
    
    for (int i = 1; i < argc; i++) {
        if (strcmp(argv[i], "-h") == 0 || strcmp(argv[i], "--help") == 0) {
            print_help();
            return 0;
        } else if (strcmp(argv[i], "-v") == 0 || strcmp(argv[i], "--verbose") == 0) {
            verbose = 1;
        } else if (strcmp(argv[i], "-d") == 0 || strcmp(argv[i], "--debug") == 0) {
            debug = 1;
        }
    }
    
    // Get input file
    char *input_file = NULL;
    for (int i = 1; i < argc; i++) {
        if (argv[i][0] != '-') {
            input_file = argv[i];
            break;
        }
    }
    
    if (!input_file) {
        handle_error("No input file specified");
    }
    
    if (!validate_input(input_file)) {
        handle_error("Invalid input file");
    }
    
    if (!file_exists(input_file)) {
        handle_error("Input file does not exist");
    }
    
    if (verbose) {
        log_message("Processing input file");
    }
    
    if (debug) {
        log_message("Debug mode enabled");
    }
    
    // Process input
    printf("Processing: %s\n", input_file);
    
    if (verbose) {
        log_message("Processing completed successfully");
    }
    
    return 0;
}
```

```c
// tests/test_main.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "../include/utils.h"

void test_validate_input(void) {
    printf("Testing validate_input...\n");
    
    // Test valid input
    if (!validate_input("test.txt")) {
        printf("FAIL: validate_input should accept valid input\n");
        exit(1);
    }
    
    // Test invalid input
    if (validate_input("")) {
        printf("FAIL: validate_input should reject empty input\n");
        exit(1);
    }
    
    if (validate_input(NULL)) {
        printf("FAIL: validate_input should reject NULL input\n");
        exit(1);
    }
    
    printf("PASS: validate_input tests\n");
}

void test_file_exists(void) {
    printf("Testing file_exists...\n");
    
    // Test with non-existent file
    if (file_exists("nonexistent_file.txt")) {
        printf("FAIL: file_exists should return false for non-existent file\n");
        exit(1);
    }
    
    printf("PASS: file_exists tests\n");
}

int main() {
    printf("Running tests...\n");
    
    test_validate_input();
    test_file_exists();
    
    printf("All tests passed!\n");
    return 0;
}
```

## Best Practices

### 1. Use Variables for Configuration
```makefile
# Good: Centralized configuration
CC = gcc
CFLAGS = -g -Wall -std=gnu99
LDFLAGS = -lm
TARGET = myprogram
SOURCES = main.c utils.c math.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
    $(CC) $(OBJECTS) -o $(TARGET) $(LDFLAGS)

# Bad: Hard-coded values scattered throughout
myprogram: main.o utils.o math.o
    gcc main.o utils.o math.o -o myprogram -lm
```

### 2. Use Automatic Variables
```makefile
# Good: Use automatic variables
%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

# Bad: Hard-coded filenames
main.o: main.c
    $(CC) $(CFLAGS) -c main.c -o main.o
```

### 3. Include Clean Targets
```makefile
# Always include a clean target
clean:
    rm -f *.o $(TARGET)

distclean: clean
    rm -f *~ .*~

.PHONY: clean distclean
```

### 4. Use .PHONY for Non-File Targets
```makefile
.PHONY: all clean install test help

all: $(TARGET)
clean: # ...
install: # ...
test: # ...
help: # ...
```

### 5. Handle Dependencies Properly
```makefile
# Include header dependencies
main.o: main.c main.h utils.h
    $(CC) $(CFLAGS) -c main.c

# Or use automatic dependency generation
%.o: %.c
    $(CC) $(CFLAGS) -MMD -c $< -o $@

-include *.d
```

### 6. Use Meaningful Target Names
```makefile
# Good: Clear target names
build: $(TARGET)
install: $(TARGET)
    cp $(TARGET) /usr/local/bin/
uninstall:
    rm -f /usr/local/bin/$(TARGET)

# Bad: Unclear names
a: $(TARGET)
b: $(TARGET)
    cp $(TARGET) /usr/local/bin/
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "Missing separator" Error
```makefile
# Problem: Using spaces instead of tabs
target: dependency
    command  # Must use TAB, not spaces

# Solution: Ensure all commands are indented with tabs
```

#### 2. "No rule to make target" Error
```bash
# Problem: Missing dependency file
$ make
make: *** No rule to make target 'missing_file.h', needed by 'main.o'.  Stop.

# Solution: Create the missing file or fix the dependency
```

#### 3. "Nothing to be done" Message
```bash
# Problem: All targets are up to date
$ make
make: Nothing to be done for 'all'.

# Solution: Use 'make clean' to force rebuild
$ make clean && make
```

#### 4. Circular Dependencies
```makefile
# Problem: Circular dependency
a: b
b: a

# Solution: Restructure dependencies
a: b
b: c
c: d
```

### Debugging Makefiles
```bash
# Show what Make would do without executing
$ make -n

# Show detailed execution
$ make -d

# Show database of rules and variables
$ make -p

# Show why a target is being rebuilt
$ make --debug=b
```

### Verbose Output
```makefile
# Enable verbose output
V ?= 0
ifeq ($(V),1)
    Q =
else
    Q = @
endif

%.o: %.c
    $(Q)$(CC) $(CFLAGS) -c $< -o $@
    @echo "Compiled $<"
```

## Example Projects

### Complete Project Structure
```
project/
├── Makefile
├── src/
│   ├── main.c
│   ├── utils.c
│   └── math.c
├── include/
│   ├── utils.h
│   └── math.h
├── tests/
│   └── test_main.c
└── docs/
    └── README.md
```

### Comprehensive Makefile
```makefile
# Comprehensive project Makefile
CC = gcc
CXX = g++
CFLAGS = -g -Wall -std=gnu99
CXXFLAGS = -g -Wall -std=c++11
LDFLAGS = -lm

# Directories
SRCDIR = src
INCDIR = include
OBJDIR = obj
BINDIR = bin
TESTDIR = tests

# Files
SOURCES = $(wildcard $(SRCDIR)/*.c)
CXXSOURCES = $(wildcard $(SRCDIR)/*.cpp)
OBJECTS = $(SOURCES:$(SRCDIR)/%.c=$(OBJDIR)/%.o)
CXXOBJECTS = $(CXXSOURCES:$(SRCDIR)/%.cpp=$(OBJDIR)/%.o)
TARGET = $(BINDIR)/program

# Test files
TEST_SOURCES = $(wildcard $(TESTDIR)/*.c)
TEST_OBJECTS = $(TEST_SOURCES:$(TESTDIR)/%.c=$(OBJDIR)/test_%.o)
TEST_TARGET = $(BINDIR)/test_program

# Default target
all: $(TARGET)

# Create directories
$(OBJDIR) $(BINDIR):
    mkdir -p $@

# Main program
$(TARGET): $(OBJECTS) $(CXXOBJECTS) | $(BINDIR)
    $(CC) $^ -o $@ $(LDFLAGS)

# C object files
$(OBJDIR)/%.o: $(SRCDIR)/%.c | $(OBJDIR)
    $(CC) $(CFLAGS) -I$(INCDIR) -c $< -o $@

# C++ object files
$(OBJDIR)/%.o: $(SRCDIR)/%.cpp | $(OBJDIR)
    $(CXX) $(CXXFLAGS) -I$(INCDIR) -c $< -o $@

# Test program
$(TEST_TARGET): $(TEST_OBJECTS) $(filter-out $(OBJDIR)/main.o,$(OBJECTS)) | $(BINDIR)
    $(CC) $^ -o $@ $(LDFLAGS)

# Test object files
$(OBJDIR)/test_%.o: $(TESTDIR)/%.c | $(OBJDIR)
    $(CC) $(CFLAGS) -I$(INCDIR) -DTEST -c $< -o $@

# Phony targets
.PHONY: all clean test install uninstall help

test: $(TEST_TARGET)
    ./$(TEST_TARGET)

clean:
    rm -rf $(OBJDIR) $(BINDIR)

install: $(TARGET)
    cp $(TARGET) /usr/local/bin/

uninstall:
    rm -f /usr/local/bin/program

help:
    @echo "Available targets:"
    @echo "  all       - Build the program"
    @echo "  test      - Build and run tests"
    @echo "  clean     - Remove build artifacts"
    @echo "  install   - Install the program"
    @echo "  uninstall - Remove the program"
    @echo "  help      - Show this help"
```

## Additional Resources

- [GNU Make Manual](https://www.gnu.org/software/make/manual/)
- [Make Tutorial](https://makefiletutorial.com/)
- [Make Documentation](https://www.gnu.org/software/make/documentation.html)
- [Stanford Unix Programming Tools](https://web.stanford.edu/class/archive/cs/cs107/cs107.1194/resources/unix_tools)

---

*This guide covers the essential aspects of using Make for building C and C++ projects. For more advanced topics like cross-compilation, parallel builds, or integration with other build systems, refer to the official GNU Make documentation.* 