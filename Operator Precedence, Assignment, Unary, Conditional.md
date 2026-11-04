# Operator Precedence, Assignment, Unary, and Conditional Operators

## Operator Precedence

Determines the **order in which operators are evaluated** in an expression with multiple operators. Higher-precedence operators are evaluated before lower-precedence ones.

```c
int result = 5 + 3 * 2;   // 11, not 16 — * has higher precedence than +, so 3*2 happens first
```

**Common precedence order (highest to lowest, simplified):**

| Precedence | Operators |
|---|---|
| Highest | `()` (parentheses) |
| | `++` `--` (unary) `!` |
| | `*` `/` `%` |
| | `+` `-` |
| | `<` `<=` `>` `>=` |
| | `==` `!=` |
| | `&&` |
| | `\|\|` |
| | `= += -=` etc. (assignment) |
| Lowest | `,` (comma) |

**Associativity** decides order *between* operators of the *same* precedence — most operators are left-to-right, but assignment and unary operators are right-to-left.

```c
int a = 10 - 4 - 2;   // (10 - 4) - 2 = 4  (left to right, since - is left-associative)
int b, c;
b = c = 5;             // c = 5 happens first, then b = c (right to left, since = is right-associative)
```

**When in doubt, use parentheses** — they override default precedence and make intent explicit, which also improves readability.

---

## Assignment Operators

Assign a value to a variable. Beyond the basic `=`, C provides **compound assignment operators** that combine an operation with assignment in one step.

| Operator | Meaning | Equivalent To |
|---|---|---|
| `=` | Assign | `a = b` |
| `+=` | Add and assign | `a = a + b` |
| `-=` | Subtract and assign | `a = a - b` |
| `*=` | Multiply and assign | `a = a * b` |
| `/=` | Divide and assign | `a = a / b` |
| `%=` | Modulus and assign | `a = a % b` |

```c
int a = 10;
a += 5;    // a = 15  (same as a = a + 5)
a -= 3;    // a = 12
a *= 2;    // a = 24
a /= 4;    // a = 6
a %= 4;    // a = 2
```

---

## Unary Operators

Operate on a **single** operand (unlike binary operators, which need two).

| Operator | Meaning |
|---|---|
| `+` | Unary plus (indicates positive, rarely needed explicitly) |
| `-` | Unary minus (negates the value) |
| `++` | Increment (adds 1) |
| `--` | Decrement (subtracts 1) |
| `!` | Logical NOT |

### Pre-increment vs. Post-increment

This distinction matters when the increment is used *within* a larger expression, not on its own line.

```c
int a = 5;

int x = ++a;   // PRE:  a becomes 6 FIRST, then x is assigned 6   → a=6, x=6
```

```c
int a = 5;

int y = a++;   // POST: y is assigned 5 FIRST (a's current value), THEN a becomes 6  → a=6, y=5
```

**Same logic applies to `--` (pre-decrement `--a` vs. post-decrement `a--`).**

**When used on their own line** (not inside another expression), pre and post produce the same end result — the distinction only matters when the value is used *at the same time* it's being changed.

```c
int a = 5;
a++;   // a = 6 — no difference here, since the result isn't used anywhere else
```

---

## Conditional (Ternary) Operator

A shorthand for a simple `if-else`, written as a single expression. It's the only operator in C that takes **three** operands.

**Syntax:**
```
condition ? value_if_true : value_if_false
```

```c
int a = 10, b = 20;
int max = (a > b) ? a : b;   // max = 20

// Equivalent if-else:
int max;
if (a > b) {
    max = a;
} else {
    max = b;
}
```

**Why it's called "ternary":** it's the only C operator with three parts (condition, true-value, false-value) — most operators are unary (one operand) or binary (two operands).

```c
int num = 7;
printf("%s", (num % 2 == 0) ? "Even" : "Odd");   // prints "Odd"
```
