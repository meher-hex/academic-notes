# Loops

**Loops** let a block of code run **repeatedly** as long as a condition holds — instead of writing the same statement over and over, a loop handles the repetition for you.

C has three loop structures: `for`, `while`, and `do-while`. They all do the same basic job but differ in **when** the condition is checked and how naturally they fit different situations.

## 1. The `for` Loop

Best used when the number of iterations is **known in advance** (or easily calculable). It packs initialization, condition, and update into one line.

### Structure

```c
for (initialization; condition; update) {
    // code to repeat
}
```

```c
for (int i = 1; i <= 5; i++) {
    printf("%d\n", i);
}
```

**Output:**
```
1
2
3
4
5
```

### How It Works

1. `initialization` runs **once**, before the loop starts (e.g. `int i = 1`).
2. `condition` is checked **before every iteration** — if true, the loop body runs; if false, the loop ends immediately.
3. The loop body executes.
4. `update` runs (e.g. `i++`), then control goes back to step 2.

So the order per cycle is: **check condition → run body → update → check condition again...**

## 2. The `while` Loop

Best used when the number of iterations is **not known in advance** — the loop just keeps going as long as some condition stays true.

### Structure

```c
while (condition) {
    // code to repeat
}
```

```c
int i = 1;

while (i <= 5) {
    printf("%d\n", i);
    i++;
}
```

**Output:**
```
1
2
3
4
5
```

### How It Works

1. `condition` is checked **first**.
2. If true, the body runs, then control jumps back to re-check `condition`.
3. If false (even on the very first check), the body **never runs at all**.

This is called an **entry-controlled** loop — the condition is a gatekeeper checked *before* entry.

## 3. The `do-while` Loop

Guarantees the loop body runs **at least once**, because the condition is checked **after** the body, not before.

### Structure

```c
do {
    // code to repeat
} while (condition);
```

```c
int i = 1;

do {
    printf("%d\n", i);
    i++;
} while (i <= 5);
```

**Output:**
```
1
2
3
4
5
```

### How It Works

1. The body runs **once**, unconditionally.
2. `condition` is checked.
3. If true, control jumps back to the top of the body; if false, the loop ends.

This is called an **exit-controlled** loop. Note the required semicolon `;` after `while (condition)` — easy to forget.

```c
int x = 100;

do {
    printf("This runs once.\n");
} while (x < 10);
```

**Output:** `This runs once.` — even though `x < 10` is false from the start, the body already ran before the condition was ever checked.

## `for` vs `while` vs `do-while`

| | `for` | `while` | `do-while` |
|---|---|---|---|
| Condition checked | Before each iteration | Before each iteration | After each iteration |
| Minimum runs | 0 | 0 | 1 |
| Best for | Known/fixed iteration count | Unknown iteration count | Body that must run at least once (e.g. menus) |
| Init/update location | Built into the loop header | Written separately | Written separately |

## Nested Loops

A loop can be placed entirely inside another loop's body — the **inner loop completes all of its iterations for every single iteration of the outer loop**. This is the backbone of pattern printing.

```c
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        printf("%d ", j);
    }
    printf("\n");
}
```

**Output:**
```
1 2 3 
1 2 3 
1 2 3 
```

The outer loop runs 3 times; for **each** of those times, the inner loop runs its full 3 iterations — so the inner body executes 3 × 3 = 9 times total.

## `break` and `continue`

- **`break`** exits the loop entirely, immediately — no further iterations happen, even if the condition would still be true.
- **`continue`** skips the *rest* of the current iteration's body and jumps straight to the next condition check (or update, in a `for` loop) — the loop keeps going, it just skips ahead.

```c
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;   // skip printing 3, but keep looping
    }
    if (i == 5) {
        break;      // stop the loop entirely once i == 5
    }
    printf("%d\n", i);
}
```

**Output:**
```
1
2
4
```

`3` is skipped because of `continue`, and the loop stops before printing `5` because of `break`.

## Important Notes

- Forgetting the `update` step in a `while` loop (e.g. forgetting `i++`) causes an **infinite loop**, since the condition never becomes false.
- In a `for` loop, any of the three parts (`initialization`, `condition`, `update`) can be left empty — `for (;;) { }` is a valid infinite loop.
- A loop body with only one statement doesn't strictly need `{ }`, but — just like with `if`, using them explicitly is good practice to avoid ambiguity later if more lines are added.
