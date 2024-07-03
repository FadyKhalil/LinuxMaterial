# Makefile 

## Table of Contents

1. [Introduction to Makefile](#introduction-to-makefile)
2. [Basic Syntax and Structure](#basic-syntax-and-structure)
3. [PHONY Targets](#phony-targets)
4. [Variables](#variables-in-makefile)
5. [Automatic Variables](#automatic-variables)
6. [Pattern Rule](#pattern-rule)
7. [Lab 1](#lab-1)
8. [Wildcard Characters](#wildcard-characters)
9. [Common Built-in Functions in Makefiles](#Common-Built-in-Functions-in-Makefiles)
10. [Lab 2](#lab-2)
11. [Understanding Implicit Rules in Makefiles](#Understanding-Implicit-Rules-in-Makefiles)

---



## Introduction to Makefile

#### - What is Make?

- its build automation tool we can install it using " sudo apt-get install make " 

#### Why are we learning Make?

- **[ Emedded Linux ]** : To build any open source tool and use this tool

- **[ YOCTO ]** : To Invoke it in recipes to compile custom apps

- **[ Device Drivers ]** : To compile drivers

  _Dont worry we will learn about YOCTO and Device Drivers later_

#### How Make works?

- By parsing input file named ' Makefile ' which contains instructions (Rules) for ' Make ' on how to compile and link the program



---



## Basic Syntax and Structure

A Makefile consists of rules. Each rule has three parts:

1. Target: the file to be generated.
2. Prerequisites: the files that are used as input to create the target.
3. Recipe: the shell commands used to create the target.

```
target: prerequisites
[<Tab>] recipe
```

### Example

```makefile
add.o: add.c
	gcc -c add.c

main.o: main.c
	gcc -c main.c

a.out: main.o add.o
	gcc main.o add.o 
	
```



---



## Phony Targets

A phony target is one that is not really the name of a file; rather, it is just a name for a recipe to be executed when you make an explicit request.

### Example

```makefile
main.o: main.c 
	gcc -c main.c

add.o: add.c
	gcc -c add.c
	
main.elf: main.o add.o
	gcc main.o add.o -o main.elf 
	
.PHONY: clean
clean:
	@rm main.o main.elf add.o
	@echo "Cleaned Successfully!"
```



---



## Variables in Makefile

Variables are used to store values that can be reused throughout the Makefile.

- **Case Sensitivity**: Variable names are case-sensitive (e.g., `var` and `VAR` are different).
- **No Whitespace**: Avoid spaces within variable names.
- **Naming Conventions**:
  - Use uppercase for constants or config parameters (e.g., `SRC_DIR`).
  - Use lowercase or mixed case for dynamic or less critical variables (e.g., `srcs`).
- **Avoid Reserved Words**: Don’t use names that might clash with built-in functions or rules.



### Variable References

Variable references are used to access the value of a variable. They are denoted by \`$(VAR)\` 

## Example

```makefile
program=iti.elf
main.o: main.c
	gcc -c main.c

add.o: add.c 
	gcc -c add.c 

$(program): main.o add.o
	gcc add.o main.o -o $(program)

.PHONY: clean
clean:
	@rm main.o main.elf add.o
	@echo "Cleaned Successfully!"
```

### Substitution References

Substitution references in Makefiles allow you to modify variable values based on patterns or suffixes.

### Syntax:

- `$(var:pattern=replacement)`: Replace matches of `pattern` with `replacement` in `var`.
- `$(var:suffix=replacement)`: Replace suffix `suffix` with `replacement` in `var`.

### Example

```makefile
OBJ = main.o file1.o file2.o
SRC = $(OBJ:.o=.c)

all:
	@echo "OBJ: $(OBJ)"
	@echo "SRC: $(SRC)"
```



---



## Automatic Variables

Automatic variables are special variables that are set by Make for each rule.

- \`$@\`: The file name of the target.
- \`$<\`: The name of the first prerequisite.
- \`$^\`: The names of all the prerequisites.

### Example

```makefile
program: file1.o file2.o
    gcc $^ -o $@
```



---



## Pattern Rule Explanation

A pattern rule in Makefiles is a rule that can be used to build files based on their names or types without explicitly specifying each dependency. It allows you to define a generic recipe for building a target based on a pattern of filenames.

### Before Using Pattern Rule:

Before using a pattern rule, you might have explicit rules for each target, like this:

```makefile
makefileCopy codemain.o: main.c
    $(CC) -c main.c -o main.o

add.o: add.c
    $(CC) -c add.c -o add.o

program: main.o add.o
    $(CC) main.o add.o -o program
```

In this example:

- `main.o` and `add.o` are compiled from `main.c` and `add.c` respectively using explicit rules.
- `program` is linked from `main.o` and `add.o` to create the final executable.

### After Using Pattern Rule:

After applying a pattern rule, you can generalize the compilation process using a single rule that matches multiple targets and their corresponding source files:

```makefile
makefileCopy code%.o: %.c
    $(CC) -c $< -o $@

program: main.o add.o
    $(CC) main.o add.o -o program
```

In this revised example:

- `%.o: %.c` is a pattern rule that matches any `.o` file to be built from its corresponding `.c` file.
- `$(CC) -c $< -o $@` is the recipe for compiling a `.c` file (`$<`) into a `.o` file (`$@`).

### Explanation:

- **Pattern Rule (`%.o: %.c`)**: This rule states that any `.o` file can be built from a `.c` file using the specified recipe.
- **Recipe (`$(CC) -c $< -o $@`)**:
  - `$(CC)`: The variable `CC` holds the C compiler command.
  - `-c $<`: `-c` option tells the compiler to produce an object file from the source file (`$<` represents the prerequisite, i.e., the source file).
  - `-o $@`: `-o` option specifies the output file (`$@` represents the target, i.e., the object file).
- **Usage in `program` Target**:
  - `program` depends on `main.o` and `add.o`.
  - The recipe for `program` links `main.o` and `add.o` into the executable `program`.

### Benefits:

- **Simplicity**: Instead of writing separate rules for each object file, a single pattern rule handles all `.o` files.
- **Flexibility**: If you add more `.c` files, the pattern rule automatically applies to them without needing additional rules.

Using pattern rules in Makefiles reduces redundancy and makes your build process more maintainable and scalable as your project grows with additional source files.

---



## Lab 1

### Requirement

We need to write a Makefile that can compile my application to work either on my host machine or on Raspberry Pi.

### Givens

The compiler for Raspberry Pi is `arm-linux-gnueabihf-gcc` and can be installed using `sudo apt install gcc-arm-linux-gnueabihf`.

### Hint

Use variable references.

### Solution for Cross Compilation (Raspberry Pi)

```makefile
cc = arm-linux-gnueabihf-gcc
obj = main.o add.o
programType = arm.elf

all: $(programType)

%.o: %.c
	$(cc) -c $<

$(programType): $(obj)
	$(cc) $^ -o $@

.PHONY: clean
clean:
	@rm main.o add.o
	@rm $(programType)
```

### Solution for Host Machine

```makefile
cc = gcc
obj = main.o add.o
programType = host.elf

all: $(programType)

%.o: %.c
	$(cc) -c $<

$(programType): $(obj)
	$(cc) $^ -o $@

.PHONY: clean
clean:
	@rm main.o add.o
	@rm $(programType)
```

### Explanation

- **Variables (`cc`, `obj`, `programType`)**: Defined to manage different compiler and target specifics.

- **Pattern Rule (`%.o: %.c`)**: Builds `.o` files from corresponding `.c` files using the specified compiler (`$(cc)`).

- **Target (`$(programType)`)**: Builds the final executable (`$(programType)`) using object files (`$(obj)`).

- **Phony Target (`clean`)**: Removes object files (`main.o`, `add.o`) and the executable (`$(programType)`).

  

These Makefiles provide a flexible solution for compiling applications for different platforms using variable references to manage compiler and target specifics easily.



---





## Wildcard Characters 

In Makefiles, wildcard characters (`*`, `?`, `[...]`) are used to perform pattern matching or globbing, allowing dynamic selection of files based on patterns rather than explicitly listing each file. Here’s a breakdown of each wildcard character:

- **`*` (Wildcard)**: Matches zero or more characters in filenames or parts of filenames.
- **`?` (Single-character Wildcard)**: Matches any single character in filenames.
- **`[...]` (Bracket Expression)**: Matches any single character specified within the brackets.

### Usage in Makefiles

#### Targets and Prerequisites

Wildcards are typically used in targets and prerequisites to specify patterns of files:

```makefile
makefileCopy code# Target with wildcard in prerequisites
program: *.o
    $(CC) $^ -o $@

# Pattern rule with wildcard in target
%.o: %.c
    $(CC) -c $< -o $@
```

- Explanation:
  - `program` depends on all `.o` files found in the current directory.
  - The `%o: %.c` rule specifies that any `.o` file should be built from its corresponding `.c` file.

#### Recipes

Wildcards are used in recipes to perform actions that apply to multiple files at once:

```makefile
makefileCopy code# Recipe using wildcard to clean all object files
clean:
    rm *.o
```

- Explanation:
  - `rm *.o` removes all `.o` files present in the current directory.

### Limitations in Variables

Wildcards are treated as literal strings when used within variable assignments:

```makefile
makefileCopy code# Incorrect usage: Wildcard in a variable assignment
objects = *.o

# Correct usage: Explicitly specifying file names
objects = main.o add.o
```

- Explanation:
  - `objects = *.o` assigns the string `*.o` to the variable `objects`, rather than capturing a list of `.o` files. To dynamically capture file lists, wildcards should be used within target or recipe contexts where Make evaluates them based on the current file system state.

---



## Common Built-in Functions in Makefiles

Among the most commonly used built-in functions in Makefiles are `wildcard`, `subst`, and `call`. These functions provide powerful capabilities for managing variables and performing operations within Makefile recipes.

#### `wildcard`

The `wildcard` function allows Makefiles to find filenames matching a specified pattern (wildcard), facilitating dynamic file selection.

**Usage:**

```makefile
makefileCopy code# Example: Find all .c files in current directory
sources = $(wildcard *.c)
```

In this example, `$(wildcard *.c)` collects all `.c` files in the current directory into the variable `sources`, demonstrating how `wildcard` enables the use of wild characters (`*`, `?`, `[...]`) in variables to match filenames based on patterns.

#### `subst`

The `subst` function substitutes occurrences of a substring within a string with another substring.

**Usage:**

```makefile
makefileCopy code# Example: Replace all occurrences of 'old' with 'new' in a string
new_var = $(subst old,new,$(var))
```

Here, `$(subst old,new,$(var))` replaces all instances of `old` with `new` in the variable `$(var)` and assigns the result to `$(new_var)`.

#### `call`

The `call` function invokes a user-defined function, passing it arguments that can be accessed within the function body using `$1`, `$2`, etc.

**Usage:**

```makefile
makefileCopy code# Define a function to echo its arguments
define say_hello
    @echo Hello, $1!
endef

# Call the function with arguments
$(call say_hello,World)
```

In this example:

- `define say_hello ... endef` defines a function named `say_hello` that echoes "Hello, $1!" where `$1` is replaced by the first argument passed to `call`.
- `$(call say_hello,World)` calls the `say_hello` function with argument `World`, resulting in `Hello, World!` being echoed.



---

## Lab 2

**Objective:** Create a Makefile to compile a project consisting of the following files and directories:

- Files: `addition.c`, `division.c`, `main.c`, `modulus.c`, `multiplication.c`, `subtraction.c`
- Directory: `Includes` containing header files (`*.h`) for each `.c` file except `main.c`.

**Makefile Requirements:**

1. Use variables for compiler (`cc`), include directories (`INCS`), source files (`src`), and project name (`projectName`).
2. Implement targets:
   - `all`: Compile all source files into an executable.
   - `%.o`: Rule to compile each `.c` file into a corresponding `.o` file.
   - `$(projectName)`: Link all object files into the executable.
   - `clean`: Remove all object files and the executable.

### Solution

```makefile
makefileCopy codecc=gcc
INCS= -I ./Includes
src= $(wildcard *.c)
projectName= iti.elf

all: $(projectName)

%.o : %.c
	$(cc) -c $(INCS) $< -o $@

$(projectName) : $(src:.c=.o)
	$(cc) $(INCS) $^ -o $@

.PHONY: clean
clean:
	rm *.o
	rm $(projectName)
```

### Explanation

- **Variables:**
  - `cc`: Compiler command (`gcc`).
  - `INCS`: Include directories (`-I ./Includes`).
  - `src`: List of source files (`$(wildcard *.c)`).
  - `projectName`: Name of the executable (`iti.elf`).
- **Targets:**
  - `all`: Default target that builds the executable `iti.elf`.
  - `%.o`: Pattern rule to compile each `.c` file into a corresponding `.o` file.
  - `$(projectName)`: Target to link all object files into the executable.
  - `clean`: Target to remove all object files (`*.o`) and the executable (`iti.elf`).

This Makefile efficiently compiles the project, utilizing variables for flexibility and providing clean management of compiled artifacts. Adjust paths and compiler options (`cc` and `INCS`) as needed for specific project requirements.



---



## Understanding Implicit Rules in Makefiles

Implicit rules in Makefiles provide predefined instructions for building certain types of targets based on file extensions and dependencies. They streamline the build process by automating common compilation tasks without requiring explicit rules for each individual file.

#### Key Concepts

1. **Automatic Dependency Resolution**:
   - Implicit rules define patterns for building targets like object files (`%.o`) from corresponding source files (`%.c`). When Make encounters a target without an explicit rule but matches an implicit rule pattern, it applies the implicit rule to build the target.
2. **File Extension Matching**:
   - Implicit rules rely on file extensions to match targets and prerequisites. For example, a `.o` file target (`%.o`) matched with a `.c` file prerequisite (`%.c`) triggers the implicit rule for compiling C source files into object files.
3. **Variable-Based Configuration**:
   - Implicit rules use variables like `CC` (compiler command) and `CFLAGS` (compiler flags) to customize compilation settings. By modifying these variables, you can change compilers or adjust compilation options globally across the Makefile.
4. **Flexibility and Efficiency**:
   - By automating repetitive build tasks, implicit rules reduce the need for redundant rule definitions. They enhance build efficiency and maintainability by ensuring consistent build processes across projects.



### Example

```makefile
# Default compiler
CC = gcc

# Explicit rule for linking executable
program: main.o add.o
    $(CC) $^ -o $@

.PHONY: clean
clean:
    rm *.o program
```

### Explanation

1. **Default Compiler (`CC`)**:
   - `CC = gcc` sets `gcc` as the default compiler.
2. **Explicit Rule (`program: main.o add.o`)**:
   - Links the `program` executable from `main.o` and `add.o`, utilizing `$(CC)` and `$(CFLAGS)`.
3. **Clean Target**:
   - `clean` is a phony target that removes all object files (`*.o`) and the `program` executable.

### Usage

- **Implicit Rule Usage**: Despite removing the explicit definition of the implicit rule (`%.o: %.c`), Makefile still applies its built-in implicit rule for building `.o` files from `.c` files. This behavior is inherent to Makefiles, where Make automatically infers how to build `.o` files from `.c` files based on file extensions.
- **Functionality**: The Makefile remains functional as `make` understands how to compile each `.c` file into a corresponding `.o` file using the default implicit rule (`$(CC) -c $< -o $@`).

### Summary

Makefiles inherently support implicit rules that define default actions for building targets like `.o` files from `.c` files. Removing the explicit definition of an implicit rule demonstrates Make's ability to rely on its built-in rules for compiling source files into object files without compromising the build process. This showcases the flexibility and robustness of Makefiles in managing and automating software builds effectively.