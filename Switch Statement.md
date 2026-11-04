# Switch Statement

The `switch` statement is an alternative to a long `if-else if` ladder, used when a single variable is being compared against a set of **specific constant values**.

## Structure

```c
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // code if no case matches
}
```

```c
int day = 3;

switch (day) {
    case 1:
        printf("Monday\n");
        break;
    case 2:
        printf("Tuesday\n");
        break;
    case 3:
        printf("Wednesday\n");
        break;
    default:
        printf("Invalid day\n");
}
```

**Output:** `Wednesday`

### How It Works

1. `expression` is evaluated **once**.
2. Its value is compared against each `case` label, top to bottom.
3. Execution jumps directly to the matching `case` and runs the code there.
4. If no `case` matches, the `default` block runs (if present).

## Why `break` Matters: Fall-Through

Without a `break`, execution doesn't stop after a matching case — it keeps running **every statement below it**, regardless of their case labels, until it hits a `break` or reaches the end of the `switch`. This is called **fall-through**.

```c
int day = 2;

switch (day) {
    case 1:
        printf("Monday\n");
    case 2:
        printf("Tuesday\n");
    case 3:
        printf("Wednesday\n");
        break;
    default:
        printf("Invalid day\n");
}
```

**Output:**
```
Tuesday
Wednesday
```

Even though `day == 2` only matches `case 2`, execution "falls through" into `case 3` as well, because there's no `break` after `printf("Tuesday\n")` to stop it. It only stops once it hits the `break` inside `case 3`.

### Fall-Through Can Be Useful — Intentionally

Grouping multiple case labels to share the same code is a common, deliberate use of fall-through:

```c
int month = 4;
int days;

switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
    case 4: case 6: case 9: case 11:
        days = 30;
        break;
    case 2:
        days = 28;
        break;
    default:
        days = 0;
}

printf("Days: %d\n", days);
```

**Output:** `Days: 30`

Here, several `case` labels stacked with no code between them all fall through into the **same** block — this is intentional fall-through, different from accidentally forgetting a `break`.

## `switch` vs `if-else` Ladder

| | `switch` | `if-else` ladder |
|---|---|---|
| Compares against | Constant values only (int/char) | Any condition/expression |
| Ranges (e.g. `x > 10`) | ❌ Not possible | ✅ Possible |
| Multiple matching values | Easy (stacked `case` labels) | Needs `\|\|` |
| Readability for many fixed cases | Cleaner | Gets messy |

## Important Restrictions

- The `expression` in `switch (expression)` and each `case value` must be an **integer type** (`int`, `char`) — `switch` cannot be used with `float`, `double`, or strings directly in C.
- Each `case value` must be a **constant** — you cannot write `case x:` where `x` is a variable, only fixed values like `case 3:` or `case 'A':`.

```c
switch (grade) {   // char is allowed (it's really an integer type in C)
    case 'A':
        printf("Excellent\n");
        break;
    case 'B':
        printf("Good\n");
        break;
    default:
        printf("Needs improvement\n");
}
```

## Note on `default`

`default` doesn't have to be the last label physically — C will still only run it when nothing else matches — but by convention it's almost always placed last for readability. Also note `default` doesn't require a `break` if it's genuinely the last block in the `switch`, though adding one is still good habit in case the `switch` is edited later and something gets added below it.
