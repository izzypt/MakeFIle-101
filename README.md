# MakeFile-101
Makefile fundamentals, concepts and ideas.

Another tutorials and references :
 1. https://makefiletutorial.com/
 2. http://www.cs.ecu.edu/karl/3300/spr16/Notes/System/make.html
 3. https://www.youtube.com/watch?v=_r7i5X0rXJk
 4. https://alextan.medium.com/makefile-101-56ba4590025b

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
