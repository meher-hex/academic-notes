# Program: Factors of a Number

Finding the **factors** of a number is a classic loop application — it combines a `for` loop with the modulus operator `%` to test every candidate number for exact divisibility.

A **factor** of `n` is any number that divides `n` completely, leaving a remainder of `0`.

## The Core Idea

To find all factors of `n`, check every integer from `1` up to `n` itself, and print the ones that divide `n` with no remainder.

```c
int n = 12;

for (int i = 1; i <= n; i++) {
    if (n % i == 0) {
        printf("%d ", i);
    }
}
```

**Output:** `1 2 3 4 6 12`

### How It Works

1. The loop variable `i` starts at `1` and goes up to `n`.
2. `n % i` gives the **remainder** when `n` is divided by `i`.
3. If that remainder is `0`, `i` divides `n` evenly — so `i` is a factor, and it gets printed.
4. Every number is at minimum divisible by `1` and by itself, which is why both always appear in the output.

## Full Program

```c
#include <stdio.h>

int main() {
    int n;

    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Factors of %d are: ", n);
    for (int i = 1; i <= n; i++) {
        if (n % i == 0) {
            printf("%d ", i);
        }
    }

    return 0;
}
```

**Sample Input:** `12`
**Output:** `Factors of 12 are: 1 2 3 4 6 12`

## Optimization: Looping Only to `n / 2`

Other than `n` itself, no factor of `n` can be **larger than `n / 2`** — so the loop's upper limit can be reduced, and `n` can be printed separately at the end.

```c
int n = 12;

printf("%d ", n);   // n is always a factor of itself
for (int i = 1; i <= n / 2; i++) {
    if (n % i == 0) {
        printf("%d ", i);
    }
}
```

**Output:** `12 1 2 3 4 6` — same factors, fewer iterations checked (this skips checking `7` through `11`, none of which could ever divide `12` evenly).

This doesn't change *how many* factors are found — it just avoids wasted comparisons, since the loop no longer checks numbers that could never be a match.

## Counting the Factors

The same loop structure can count factors instead of (or alongside) printing them, using a counter variable.

```c
int n = 12, count = 0;

for (int i = 1; i <= n; i++) {
    if (n % i == 0) {
        count++;
    }
}

printf("Number of factors: %d\n", count);
```

**Output:** `Number of factors: 6`

## Note on Prime Numbers

A number with **exactly two factors** (`1` and itself) is prime — this factor-counting logic is the direct foundation for the next topic, checking whether a number is prime.
