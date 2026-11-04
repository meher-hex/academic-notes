# Typecasting

**Typecasting** means converting a value from one data type to another, either automatically (by the compiler) or manually (by the programmer).

## Implicit Typecasting (Automatic)

The compiler converts one type to another on its own, without you writing anything special — usually to avoid losing data, by converting "smaller" types up to "larger" ones.

```c
int a = 5;
float b = a;   // int automatically converted to float → 5.0

int x = 10;
double y = x / 3;   // x is int, but result still needs conversion — see below for the catch
```

## Explicit Typecasting (Manual)

You force a conversion yourself using `(type)` before a value or expression.

```c
float f = 9.5;
int a = (int)f;   // a = 9 — decimal part is dropped (truncated), not rounded
```

**Syntax:** `(target_type) expression`

## The Classic Integer Division Problem

This is where typecasting matters most in practice. Dividing two integers in C always produces an integer result — even if you store it in a `float`.

```c
int a = 5, b = 2;

float result = a / b;        // ❌ result = 2.0, not 2.5!
```

**Why:** `a / b` is evaluated *first*, using integer division (since both `a` and `b` are `int`), giving `2`. Only *after* that does it get converted to `float` for storage — but the decimal part is already lost by then.

**Fix — cast before the division happens:**

```c
float result = (float)a / b;   // ✅ result = 2.5
```

Here, `(float)a` converts `a` to `9.5`-style floating point *before* the division runs, so the division itself becomes floating-point division, giving the correct decimal result. Only one operand needs to be cast — C then promotes the other operand automatically to match, following the same implicit conversion rules covered in Operators.

## Common Uses of Explicit Casting

```c
int total = 17, count = 5;
float average = (float)total / count;   // force accurate decimal average

double d = 9.99;
int whole = (int)d;   // 9 — truncates, does NOT round to nearest

char ch = (char)65;   // 'A' — converts integer ASCII value to its character
```

**Note:** Casting `float`/`double` to `int` always **truncates** (cuts off the decimal part) — it does not round to the nearest whole number. `(int)9.99` gives `9`, not `10`.
