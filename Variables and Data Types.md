# Variables and Data Types in C

## What is a Variable?

A variable is a named location in memory used to store a value that can change during program execution.

## Declaring a Variable

Tells the compiler the variable's name and type, reserving memory for it — but doesn't give it a value yet. Same type of data can be declared in one line.

```c
int age;
int a, b;
```

## Initializing a Variable

Assigning a value to a variable, either at the same time as declaration or later.

```c
int age = 20;          // declared and initialized together
float price;
price = 99.99;          // declared first, initialized later
int a, b = 2;
```

**Rule:** A variable must be declared before it's used. Using it before initializing gives it an unpredictable "garbage value."

---

## Primitive Data Types

### `int` — Integer

Stores whole numbers (no decimal point).

| Property | Value (typical, 32-bit systems) |
|---|---|
| Size | 4 bytes (32 bits) |
| Range | -2,147,483,648 to 2,147,483,647 |

```c
int count = 150;
```

### `char` — Character

Stores a single character, internally represented as its ASCII integer value.

| Property | Value |
|---|---|
| Size | 1 byte (8 bits) |
| Range | -128 to 127 (signed) or 0 to 255 (unsigned) |

```c
char grade = 'A';
```

### `float` — Single-Precision Floating Point

Stores decimal (real) numbers.

| Property | Value |
|---|---|
| Size | 4 bytes (32 bits) |
| Range | ~1.2 × 10⁻³⁸ to ~3.4 × 10³⁸ |
| Precision | ~6-7 significant decimal digits |

```c
float pi = 3.14159f;   // 'f' suffix marks it as float, not double
```

### `double` — Double-Precision Floating Point

Same idea as `float`, but with more bits allocated for greater precision and range.

| Property | Value |
|---|---|
| Size | 8 bytes (64 bits) |
| Range | ~2.3 × 10⁻³⁰⁸ to ~1.7 × 10³⁰⁸ |
| Precision | ~15-16 significant decimal digits |

```c
double pi = 3.141592653589793;
```

---

## How Floating-Point Numbers Are Stored: Mantissa and Exponent

`float` and `double` don't store decimal numbers the way we write them. Instead, they use a format similar to scientific notation:

```
value = mantissa × 2^exponent
```

- **Mantissa (significand):** the actual digits of precision — think of it as the "meaningful number part."
- **Exponent:** determines the scale/magnitude — how far to shift the decimal point.

**Example (conceptually):** `3.14` might be stored as a mantissa representing `1.57` with an exponent that scales it to the correct position — similar to how `314 × 10⁻²` represents `3.14` in base-10 scientific notation, but done in binary.

**Bit allocation (IEEE 754 standard):**

| Type | Total bits | Sign | Exponent | Mantissa |
|---|---|---|---|---|
| `float` | 32 | 1 bit | 8 bits | 23 bits |
| `double` | 64 | 1 bit | 11 bits | 52 bits |

This is *why* `double` has more precision than `float` — it simply has more mantissa bits to represent the meaningful digits of the number, and more exponent bits to represent a wider range of magnitudes.

---

## Type Qualifiers

Qualifiers modify the size or behavior of a base data type.

### `short` and `long` — Modify Size

```c
short int a;   // typically 2 bytes, smaller range than int
long int b;    // typically 4 or 8 bytes (system-dependent), larger range than int
long double c; // extended precision beyond double
```

### `signed` and `unsigned` — Modify Range

- **signed** (default for `int` and `char`): can store both negative and positive values.
- **unsigned**: only stores zero and positive values, but doubles the positive range since no bit is used for sign.

```c
signed int x = -10;    // default behavior, same as just 'int x'
unsigned int y = 10;   // cannot hold negative values
```

**Example — `char` range depends on signed/unsigned:**

| | Range |
|---|---|
| `signed char` | -128 to 127 |
| `unsigned char` | 0 to 255 |

### `const` — Value Cannot Change

```c
const float PI = 3.14159;   // attempting to reassign PI later causes a compile error
```

---

## Literal Suffixes

When writing a number directly in code (a **literal**), you can attach a suffix to tell the compiler exactly which type to treat it as — instead of relying on the default. This matters because plain integer literals are treated as `int` by default, and plain decimal literals are treated as `double` by default, which isn't always what you want.

| Suffix | Meaning | Example | Type |
|---|---|---|---|
| `U` / `u` | Unsigned | `10U` | `unsigned int` |
| `L` / `l` | Long | `10L` | `long int` |
| `UL` / `ul` | Unsigned long | `10UL` | `unsigned long int` |
| `LL` / `ll` | Long long | `10LL` | `long long int` |
| `ULL` / `ull` | Unsigned long long | `10ULL` | `unsigned long long int` |
| `F` / `f` | Float | `3.14F` | `float` |
| `L` / `l` (on a decimal) | Long double | `3.14L` | `long double` |

```c
unsigned int a = 10U;
long int b = 100000L;
unsigned long int c = 100000UL;
float pi = 3.14F;       // without F, this literal defaults to double
```

**Why it matters:** Without the suffix, the compiler picks a default type for the literal based on its value and form — which can silently cause the wrong type to be used (e.g., overflow issues if a large number is treated as `int` instead of `long`). The suffix makes your intent explicit and avoids relying on default behavior. Suffixes can also be combined and are not case-sensitive (`UL`, `ul`, `Ul`, `uL` all work the same).

---

## Number Systems in C

C allows writing integer literals in different number systems by using a prefix.

| System | Base | Prefix | Example | Decimal Value |
|---|---|---|---|---|
| Decimal | 10 | (none) | `25` | 25 |
| Octal | 8 | `0` | `031` | 25 |
| Hexadecimal | 16 | `0x` or `0X` | `0x19` | 25 |

```c
int a = 25;      // decimal
int b = 031;     // octal — leading 0 tells compiler to interpret as base 8
int c = 0x19;    // hexadecimal — 0x tells compiler to interpret as base 16

printf("%d %d %d", a, b, c);  // all print 25 — same value, different literal forms
```

**Why this matters:** The number system used is just a different way of *writing* the same value in source code — internally, it's all stored as plain binary in memory. Hexadecimal is especially common when working with memory addresses or bit-level data, since it maps cleanly to binary (each hex digit = exactly 4 bits).
