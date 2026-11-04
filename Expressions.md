# Expressions

An **expression** is any valid combination of operators, constants, and variables that evaluates to a single value.

```c
5 + 3          // evaluates to 8
a * b - c      // evaluates based on values of a, b, c
x > 10         // evaluates to 1 (true) or 0 (false)
```

## Types of Expressions

### 1. Constant Expression

Made up entirely of constant values — no variables involved. The compiler can evaluate it at compile time.

```c
5 + 10         // constant expression, always evaluates to 15
```

### 2. Integral Expression

An expression that produces an integer result.

```c
int a = 5, b = 2;
int result = a + b;   // integral expression → 7
```

### 3. Float Expression

An expression that produces a floating-point (decimal) result.

```c
float x = 5.0, y = 2.0;
float result = x / y;   // float expression → 2.5
```

### 4. Relational Expression

Compares two values using relational operators (`<`, `>`, `<=`, `>=`, `==`, `!=`). Always evaluates to `1` (true) or `0` (false) in C.

```c
5 > 3          // relational expression → 1
a == b         // relational expression → 0 or 1
```

### 5. Logical Expression

Combines relational expressions or boolean-like values using logical operators (`&&`, `||`, `!`). Also evaluates to `1` or `0`.

```c
(a > 5) && (b < 10)   // logical expression → 1 or 0
```

### 6. Arithmetic Expression

Any expression combining variables/constants using arithmetic operators (`+`, `-`, `*`, `/`, `%`).

```c
(a + b) * c - d
```

## Note on Evaluation

Every expression in C **evaluates to a value** — even relational and logical ones (which evaluate to `0` or `1`, since C doesn't have a true boolean type in older standards). This is why something like `if (a > b)` works — the condition itself is just an expression producing `0` or `1`, and `if` checks whether that result is non-zero.

## Example: Writing a Real-World Formula as an Expression

The quadratic formula:

```
x = (-b ± √(b² - 4ac)) / 2a
```

...can be written directly as a C expression, splitting the `±` into two separate expressions (one for each root):

```c
#include <math.h>   // needed for sqrt()

double a = 1, b = -3, c = 2;

double root1 = (-b + sqrt(b * b - 4 * a * c)) / (2 * a);
double root2 = (-b - sqrt(b * b - 4 * a * c)) / (2 * a);
```

- `b * b - 4 * a * c` is the **discriminant** — computed first since it's needed by both roots.
- `sqrt()` comes from `math.h` and returns the square root of its argument.
- Parentheses matter here: `(2 * a)` must be grouped, otherwise `/ 2 * a` would divide by 2 first and then multiply by `a` — a different result entirely, due to operator precedence (division and multiplication share the same precedence and evaluate left to right).

**Note on complex roots:** If the discriminant (`b² - 4ac`) is negative, the mathematical answer involves imaginary numbers, which `sqrt()` cannot handle directly (it would return `nan` for a negative input). This is still solvable in C, but requires handling the real part and imaginary part as two separate values, rather than one combined expression — e.g., computing the real part as `-b / (2a)` and the imaginary part as `sqrt(4ac - b²) / (2a)` separately, since standard C arithmetic only works on real numbers.

