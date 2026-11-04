**N.B.** *Main study content starts from Section 1.5, Page - 8 of Schaum's Outlines Programming with C by Byron Gottfried (2nd Edition).*

**Introductory Info on C (INSIGNIFICANT):**
- general-purpose, structured programming language
- used for systems programming (writing operating systems) as well
- developed in the 1970s by Dennis Ritchie 
- outgrowth of earlier languages BCPL & B (also developed at Bell Laboratories)
- standardized version adheres to the ANSI (American National Standards Institute) std.

General Purpose Languages: Pascal, Fortran, BASIC
Simulation Languages: CSMP, SIMAN
List-Processing Languages (widely used for AI applications): LISP

**Structure of a C program:**
Every C program consists of one or more modules called functions. One of the functions must be called *main*. *The program will always begin by executing the `main` function, which may access other functions.* 

Each function must contain:
1. A function heading, which consists of the function name, followed by an *optional* list of arguments, enclosed in parentheses.
2. A list of argument declarations, if arguments are included in the heading.
3. A compound statement, which comprises the remainder of the function.

**Argument:** Symbol representing information being passed between the function and other parts of the program. *Also referred to as parameters.*

Each compound statement is enclosed within a pair of braces, i.e., { }. The braces may contain one or more expression statements. *Each expression statement must end with a semicolon (;).*

Comments may appear anywhere within a program, as long as they are placed within the
*delimiters* `/* and */` *(// is used for single-line comments)*.

**Uppercase and lowercase characters are not equivalent in C.** 

Three advantages of high-level language over machine language:
- Simplicity
- Uniformity
- Portability (Machine Independence)

##### <u>DEFINITIONS:</u>
**Program:** The appropriate set of instructions given to a computer to process a particular set of data is called a program.

**Source Program:** The original program written in high-level programming language is called the source program.

**Object Program:** The resulting machine-understandable program produced from the source program is called object program.

**Compilation & Interpretation:** A program that is written in a high-level language must, however, be translated into machine language before it can be executed. This is known as compilation or interpretation, depending on how it is carried out.

**Compiler:** Compilers translate the entire program into machine language before executing any of the instructions.

**Interpreter:** Interpreters proceed through a program by translating and then executing single instructions or small groups of instructions.

*A compiler or interpreter is itself a computer program.*

**Compound Statement:**
**Expression Statement:**
**Assignment Statement:**
**Header File:** A header file is a file with a `.h` extension that contains declarations of functions (prototypes), macros, constants, and data types. It is included in a C program with the `#include` preprocessor directive, so that the compiler knows about the functions and definitions it provides before they are used. For example, `stdio.h` provides the declarations for `printf()` and `scanf()`, and `math.h` provides those for `sqrt()` and `pow()`. *Standard headers use angle brackets (`#include <stdio.h>`), and user-defined headers use double quotes (`#include "myheader.h"`).*


