# Operators — Logical and Modulus (In-Depth)

## Logical Operators

Used to combine or invert conditions. Operate on the truth value of expressions (`0` = false, any non-zero = true), and always produce `0` or `1` as a result.

| Operator | Name | Meaning |
|---|---|---|
| `&&` | Logical AND | True only if **both** conditions are true |
| `\|\|` | Logical OR | True if **at least one** condition is true |
| `!` | Logical NOT | Inverts the truth value |

```c
int a = 5, b = 10;

(a > 0) && (b > 0)   // 1 (both true)
(a > 0) || (b < 0)   // 1 (at least one true)
!(a > 0)             // 0 (inverts true → false)
```

### Short-Circuit Evaluation

C stops evaluating a logical expression as soon as the overall result is already determined — it doesn't waste time checking the rest.

```c
(a > 0) && (b / a > 2)
```
- If `a > 0` is false, C never even evaluates `b / a > 2` — because with AND, one false condition already makes the whole thing false.
- This is actually useful for safety: if `a` were `0`, checking `a > 0` first and short-circuiting avoids ever running `b / a`, which would otherwise cause a divide-by-zero error.

Similarly, with `||`, if the first condition is already true, the second is never checked (since OR only needs one true condition).

---

## Modulus Operator (`%`)

Returns the **remainder** of integer division, not the quotient.

```c
10 % 3    // 1  → because 10 = 3×3 + 1
17 % 5    // 2  → because 17 = 5×3 + 2
9 % 3     // 0  → because 9 divides evenly
```

**Important:** `%` only works with integers in C — using it with `float`/`double` causes a compile error.

### Modulus and Congruency

Two numbers are said to be **congruent modulo n** if they leave the same remainder when divided by `n`. Written as:

```
a ≡ b (mod n)
```

This is exactly what the `%` operator checks:

```c
if (a % n == b % n) {
    // a and b are congruent mod n
}
```

**Example:** `17` and `29` are congruent mod `4`, since:
```
17 % 4 = 1
29 % 4 = 1     // same remainder → congruent
```

This concept is the backbone of things like checking if numbers fall in the same "cycle" — e.g., days of the week (`day % 7`), or clock arithmetic (`hour % 12`).

### Distributive Property of Modulus

Modulus distributes over addition and multiplication in a specific way — useful for keeping numbers small in calculations (common in competitive programming to avoid overflow):

```
(a + b) % n = ((a % n) + (b % n)) % n
(a × b) % n = ((a % n) × (b % n)) % n
```

**Example — addition:**
```c
int a = 14, b = 9, n = 5;

(a + b) % n              // (14 + 9) % 5 = 23 % 5 = 3
((a % n) + (b % n)) % n  // (4 + 4) % 5 = 8 % 5 = 3   ✅ same result
```

**Example — multiplication:**
```c
int a = 14, b = 9, n = 5;

(a * b) % n              // (14 * 9) % 5 = 126 % 5 = 1
((a % n) * (b % n)) % n  // (4 * 4) % 5 = 16 % 5 = 1   ✅ same result
```

**Why this matters:** When numbers get very large (e.g., factorials, large powers), computing the full value first can overflow `int`/`long`. Applying `%` at each step using the distributive property keeps intermediate values small while still arriving at the correct final remainder.

**Note:** Subtraction behaves slightly differently and can produce a negative result in C (unlike in pure math, where mod results are always non-negative):

```c
(-7) % 3   // -1 in C, not 2 — C's % keeps the sign of the dividend (the first number)
```

To force a proper non-negative result when subtraction might go negative, a common technique is:
```c
((a - b) % n + n) % n
```

### What Happens if the Two Operands Are of Different Types?

`%` requires both operands to be **integer types** — but they don't have to be the *same* integer type. If they differ (e.g., `int` and `long`, or `int` and `char`), C automatically converts the smaller/lower-ranked type up to match the larger one before performing the operation. This is called **implicit type promotion** (part of C's "usual arithmetic conversions").

```c
int a = 17;
long b = 5;

long result = a % b;   // 'a' is promoted from int to long before the operation runs
```

**General promotion order (smaller → larger):** `char` → `short` → `int` → `long` → `long long`. Whichever operand has the "larger" type, the other gets temporarily converted to match it for the calculation — the original variable's type doesn't change, only the value used in that specific operation.

```c
char c = 10;
int x = 3;

int result = c % x;   // 'c' is promoted from char to int before the modulus runs
```

**Important:** This promotion only applies between integer types. Mixing `%` with a `float` or `double` operand at all is still a compile error, regardless of promotion rules — promotion can't turn a floating-point type into an integer type.

```c
float f = 5.0;
int x = 2;

f % x;   // ❌ still a compile error — % cannot involve float/double, promotion doesn't fix this
```

