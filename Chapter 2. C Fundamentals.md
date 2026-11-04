- each escape sequence such as `\b, \t, \n` (backspace, horizontal tab, newline) represents a single character

### Identifiers
Identifiers are names that are given to various program elements, such as variables, functions and arrays.

**Keyword:** Certain reserved words that have standard, predefined meanings in C.
<u>Standard Keywords (All in lowercase; <i>ANSI C has 32 keywords</i>)</u>
auto, break, case, char, const, continue, default, do,
double, else, enum, extern, float, for, goto, if, 
int, long, register, return, short, signed, sizeof, static,
struct, switch, typedef, union, unsigned, void, volatile, while

### RULES:
1. first character must be a letter
2. cannot be a reserved keyword
3. may start with an underscore
4. no other character except underscore

### Data Types
##### `int` — Integer
Stores whole numbers (no decimal point).

| Property | Value (typical, 32-bit systems)        |
| -------- | -------------------------------------- |
| Size     | 4 bytes (32 bits)                      |
| Range    | - 2<sup>31</sup> to 2<sup>31</sup> - 1 |

```c
int count = 150;
```

##### `char` — Character
Stores a single character, internally represented as its ASCII integer value.

| Property | Value |
|---|---|
| Size | 1 byte (8 bits) |
| Range | -128 to 127 (signed) or 0 to 255 (unsigned) |

```c
char grade = 'A';
```

##### `float` — Single-Precision Floating Point
Stores floating-point numbers (a number with a decimal point and/or an exponent).

| Property  | Value                           |
| --------- | ------------------------------- |
| Size      | 4 bytes (32 bits)               |
| Range     | ~1.2 × 10⁻³⁸ to ~3.4 × 10³⁸     |
| Precision | ~6-7 significant decimal digits |

```c
float pi = 3.14159f;   // 'f' suffix marks it as float, not double
```

##### `double` — Double-Precision Floating Point

Same idea as `float`, but with more bits allocated for greater precision and range.

| Property | Value |
|---|---|
| Size | 8 bytes (64 bits) |
| Range | ~2.3 × 10⁻³⁰⁸ to ~1.7 × 10³⁰⁸ |
| Precision | ~15-16 significant decimal digits |

```c
double pi = 3.141592653589793;
```
