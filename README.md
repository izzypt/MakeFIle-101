# MakeFile-101
Makefile fundamentals, concepts and ideas.

Another tutorials and references :
 1. https://makefiletutorial.com/
 2. http://www.cs.ecu.edu/karl/3300/spr16/Notes/System/make.html
 3. https://www.youtube.com/watch?v=_r7i5X0rXJk
 4. https://alextan.medium.com/makefile-101-56ba4590025b
 5. https://opensource.com/article/18/8/what-how-makefile

# Makefiles why ?

Imagine that a piece of software has 100 modules. In order to compile and link it, you do not want to have to type 101 commands! You do not even want to type two commands. One should be enough. 

- Makefiles are used to help decide which parts of a large program need to be recompiled. 
- Make can also be used beyond compilation too, when you need a series of instructions to run depending on what files have changed. 
- This tutorial will focus on the C/C++ compilation use case.

# MakeFile Structure

A makefile has 2 essential components :

 - <ins>Rules</ins>
 
   - A <ins>rule</ins> tells make how to build a particular <ins>target</ins>. Typically, a target is a file name. The form of a rule is

   ``` 
    target: dependencies
	    command
	    ...
	    command
    ```
    
    -  The <ins>dependencies</ins> are other targets or source files on which this target depends, written one after another, separated by spaces. The commands tell what to do to build the target. For example, rule:
    
    ```
      main.o: main.cpp tools.h stuff.h
	      g++ -Wall -g -c main.cpp
    ```
    
    - says that file main.o depends on main.cpp, tools.h and stuff.h. (If any of those files is modified, main.o needs to be rebuilt.) It also gives a command that will build main.o. Each command must be preceded by a tab character. That is how make determines that it is a command. 
 
 - <ins>Definitions</ins>
   - Definitions allow you to simplify rules and to avoid writing the same thing several times, just like variables:
   ```
    //defines $(NAME) to stand for the given text
    NAME = TEXT
   ```
   - Another example :
   ```
    CC     = g++
    CFLAGS = -Wall -g -c

    tells make that

    $(CC) $(CFLAGS) main.cpp

    should be replaced by

    g++ -Wall -g -c main.cpp
    
# Going deeper

The main structure of a Makefile consists of a series of rules and dependencies that specify how to build a target (usually an executable or library) from source files. Each rule in a Makefile consists of a target, zero or more prerequisites (dependencies), and a series of commands to be executed when the target is out-of-date relative to its dependencies. The basic structure of a rule in a Makefile is:

```
target: prerequisites
    command
    command
    ...
```

Here's what each of these components means:

- `target`: The name of the file that is generated by the rule. This can be an executable, a library, an object file, or any other file that needs to be built.
- `prerequisites`: The names of the files that are required to build the target. These are typically source files, headers, libraries, or other files that the target depends on.
- `command`: The command(s) to be executed in order to build the target. These are shell commands that are executed in the order they appear in the Makefile. Each command must start with a tab character (not spaces).

In addition to rules, a Makefile can also contain variables, which are used to store values that can be used later in the Makefile. Variables in Makefiles are typically defined at the top of the file using the syntax `VARIABLE_NAME = value`.

Here's a simple example of a Makefile that builds an executable called `myprogram` from two source files, `main.c` and `util.c`:

```
CC = gcc
CFLAGS = -Wall -Wextra -Werror
LDFLAGS = -lm
SRC = main.c util.c
OBJ = $(SRC:.c=.o)

myprogram: $(OBJ)
    $(CC) $(LDFLAGS) $(OBJ) -o $@

%.o: %.c
    $(CC) $(CFLAGS) -c $< -o $@

clean:
    rm -f $(OBJ) myprogram
```

In this example, `CC`, `CFLAGS`, and `LDFLAGS` are variables that store the compiler and linker flags. `SRC` is a variable that stores the names of the source files, and `OBJ` is a variable that is derived from `SRC` and stores the names of the object files. The `myprogram` rule depends on the `$(OBJ)` files and is built by linking them together with the `$(LDFLAGS)` variable. The `%o:%c` rule specifies how to build object files from source files, and the `clean` rule removes the object files and the `myprogram` executable.

# Macros

Makefiles have some macros by default:

<ins>Internal macros</ins>
- Internal macros are predefined in make.
- “make -p” to display a listing of all the macros, suffix
rules and targets in effect for the current build

<ins>Special macros</ins>
- The macro @ evaluates to the name of the current target:

```
  prog1 : $(objs)
  $(CXX) -o $@ $(objs)
```
- is equivalent to
```
prog1 : $(objs)
$(CXX) -o prog1 $(objs)
```

# Simply expanded variables

Both ``` ${CC}```   and  ``` $(CC)```  are valid references to call gcc. But if one tries to reassign a variable to itself, it will cause an infinite loop. Let's verify this:

```
CC = gcc
CC = ${CC}

all:
    @echo ${CC}
```
Running make will result in:
```
$ make
Makefile:8: *** Recursive variable 'CC' references itself (eventually).  Stop.
```

To avoid this scenario, we can use the ```:=``` operator (this is also called the simply expanded variable). We should have no problem running the makefile below:
```
CC := gcc
CC := ${CC}

all:
    @echo ${CC}
```
