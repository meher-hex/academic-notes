# Comments

A **comment** is text in source code that the compiler completely ignores — it exists purely for humans reading the code, not for the program's execution.

## Single-Line Comment

Starts with `//` and continues to the end of that line.

```c
int age = 20;   // stores the user's age
```

## Multi-Line Comment

Starts with `/*` and ends with `*/`. Can span multiple lines.

```c
/*
   This program calculates the area of a circle.
   Author: ...
   Date: ...
*/
float area = PI * r * r;
```

**Note:** Multi-line comments cannot be nested — putting a `/* */` comment inside another `/* */` comment causes an error, since the compiler closes the comment at the *first* `*/` it finds.

```c
/* Outer comment /* inner comment */ this part is no longer a comment */  // ❌ breaks
```

## Why Use Comments

- Explain *why* code does something, not just *what* it does (the code itself already shows what).
- Leave notes for yourself or others reading the code later.
- Temporarily "turn off" a line of code during testing, without deleting it:

```c
// printf("Debug: %d", x);   // disabled for now
```

**Note:** Comments don't affect program size or execution speed — the compiler strips them out entirely before compiling.
