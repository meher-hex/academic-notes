# IDE and Compilers

## IDE (Integrated Development Environment)

A software application that bundles together the tools needed to write and run programs, so you don't have to juggle separate tools manually.

Typically includes:
- **Code editor** — where you write your source code, usually with syntax highlighting.
- **Compiler/Interpreter access** — built-in way to compile and run your code without using a separate terminal.
- **Debugger** — lets you step through code line by line to inspect variable values and find issues.
- **Header files** — the IDE bundles standard library header files (like `stdio.h`, `math.h`) so `#include` statements work right out of the box, without you having to separately download or install them.

Examples: VS Code, Code::Blocks, Dev-C++, CLion.

## Compiler

A program that translates your source code (human-readable C code) into machine code (binary instructions the computer's processor can actually execute).

```
Source Code (.c file) → Compiler → Executable Program
```

## Compilers Only Catch Syntax Errors

An important distinction: a compiler can only detect **syntax errors** — mistakes in the structure/grammar of the code (missing semicolons, mismatched brackets, undeclared variables, etc.). It **cannot** detect **logical errors** — code that compiles and runs fine, but produces the wrong result because the logic itself is flawed.

```c
// Syntax error — compiler catches this immediately
int x = 5    // ❌ missing semicolon

// Logical error — compiler does NOT catch this; code runs but gives wrong output
int area = length + width;   // ❌ should be length * width, but compiles fine
```

**Why this matters:** A program compiling successfully doesn't mean it's correct — it only means the code follows valid C syntax. Catching logical errors requires actually testing the program's output against expected results, not just relying on the compiler.
