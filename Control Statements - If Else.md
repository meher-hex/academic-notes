# Control Statements — if, else-if, else

**Control statements** let a program make decisions — instead of executing every line top to bottom unconditionally, certain blocks of code only run if specific conditions are met.

## 1. The `if` Statement

The simplest control statement. The block inside `{ }` runs **only if** the condition evaluates to true (non-zero).

```c
if (condition) {
    // runs only if condition is true
}
```

```c
int age = 20;

if (age >= 18) {
    printf("You are an adult.\n");
}
```

If `condition` is false (`0`), the block is skipped entirely and execution continues with whatever comes after it.

## 2. The `if-else` Statement

Adds an alternative block that runs when the condition is false — exactly one of the two blocks executes, never both.

```c
if (condition) {
    // runs if condition is true
} else {
    // runs if condition is false
}
```

```c
int age = 15;

if (age >= 18) {
    printf("You are an adult.\n");
} else {
    printf("You are a minor.\n");
}
```

**Output:** `You are a minor.`

## 3. The `if-else if-else` Ladder

Used when there are **more than two** possible outcomes. C checks each condition in order, top to bottom, and runs the block belonging to the **first** one that's true — every condition after that is skipped, even if it would also be true.

```c
if (condition1) {
    // runs if condition1 is true
} else if (condition2) {
    // runs if condition1 is false AND condition2 is true
} else if (condition3) {
    // runs if condition1 and condition2 are false AND condition3 is true
} else {
    // runs if none of the above conditions are true
}
```

```c
int marks = 72;

if (marks >= 90) {
    printf("Grade A\n");
} else if (marks >= 75) {
    printf("Grade B\n");
} else if (marks >= 60) {
    printf("Grade C\n");
} else {
    printf("Grade F\n");
}
```

**Output:** `Grade C`

### Why Order Matters

```c
int marks = 95;

if (marks >= 60) {
    printf("Grade C\n");   // this runs — first true condition wins
} else if (marks >= 90) {
    printf("Grade A\n");   // never checked, even though 95 >= 90 is also true
}
```

Here `marks >= 60` is checked first and is already true, so C never even looks at `marks >= 90` — the ladder must be written from the **most specific/highest condition down to the least**, otherwise later, more accurate conditions get silently skipped.

## 4. Nested `if` Statements

An `if` (or `if-else`) can be placed entirely inside another `if`'s block — useful when a second decision only makes sense after the first condition is already satisfied.

```c
int age = 25;
int hasLicense = 1;   // 1 = true, 0 = false

if (age >= 18) {
    if (hasLicense) {
        printf("You can drive.\n");
    } else {
        printf("You need a license first.\n");
    }
} else {
    printf("You are too young to drive.\n");
}
```

The inner `if-else` only runs at all if the outer condition (`age >= 18`) is true — if it's false, the inner block is never even reached.

## Note on the Dangling `else` Problem

An `else` always binds to the **nearest unmatched `if`** — this matters when braces `{ }` are left out.

```c
if (a > 0)
    if (b > 0)
        printf("both positive\n");
    else
        printf("a positive, b not positive\n");   // binds to the INNER if, not the outer one
```

Even though the indentation might *suggest* the `else` belongs to the outer `if (a > 0)`, C actually attaches it to the closest `if` above it — `if (b > 0)`. This is exactly why it's good practice to always use `{ }` explicitly, even for single-statement blocks, to avoid this kind of ambiguity.

## Note on Truthiness

C has no distinct boolean type in older standards — `if` simply checks whether the condition's value is **zero (false) or non-zero (true)**.

```c
int x = 5;

if (x) {              // true, because x is non-zero (5)
    printf("x is truthy\n");
}

if (x - 5) {           // false, because x - 5 evaluates to 0
    printf("this won't print\n");
}
```

This is why a common (and sometimes accidental) bug is writing `if (x = 5)` — a single `=` **assigns** 5 to `x` and the whole expression evaluates to `5` (non-zero → true), rather than comparing with `==`.
