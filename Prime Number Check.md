# Program: Check if a Number is Prime

A **prime number** is a number greater than `1` that has **exactly two factors**: `1` and itself. This program builds directly on the previous topic — instead of listing all factors, it just checks whether *any* factor exists other than `1` and `n`.

## The Core Idea

Loop from `2` up to `n - 1` (skipping `1` and `n`, since every number is trivially divisible by those) and check if any of those numbers divide `n` evenly. If **none** do, `n` is prime.

```c
int n = 7;
int isPrime = 1;   // assume prime until proven otherwise

for (int i = 2; i < n; i++) {
    if (n % i == 0) {
        isPrime = 0;   // found a factor other than 1 and n — not prime
        break;
    }
}

if (isPrime) {
    printf("%d is prime\n", n);
} else {
    printf("%d is not prime\n", n);
}
```

**Output:** `7 is prime`

### How It Works

1. A flag variable `isPrime` starts at `1` (true), assuming the number is prime.
2. The loop checks every `i` from `2` to `n - 1` for exact divisibility using `n % i == 0`.
3. The moment **one** such `i` is found, `n` cannot be prime — so `isPrime` is set to `0`, and `break` exits the loop immediately, since there's no need to keep checking.
4. If the loop finishes **without ever finding** a factor, `isPrime` stays `1`, confirming `n` is prime.

## Full Program

```c
#include <stdio.h>

int main() {
    int n;

    printf("Enter a number: ");
    scanf("%d", &n);

    int isPrime = 1;

    if (n <= 1) {
        isPrime = 0;   // 0, 1, and negatives are never prime
    } else {
        for (int i = 2; i < n; i++) {
            if (n % i == 0) {
                isPrime = 0;
                break;
            }
        }
    }

    if (isPrime) {
        printf("%d is prime\n", n);
    } else {
        printf("%d is not prime\n", n);
    }

    return 0;
}
```

**Sample Input:** `15`
**Output:** `15 is not prime`

The `n <= 1` check matters: by definition, primes are greater than `1`, so `0`, `1`, and negative numbers must be excluded up front — otherwise the loop `for (i = 2; i < n; i++)` wouldn't even execute for something like `n = 1` (since `2 < 1` is false), and `isPrime` would incorrectly stay `1`.

## Why `break` Matters Here

Without `break`, the loop would keep running all the way to `n - 1` even after already finding a factor — wasting iterations on a question that's already answered. `break` stops the search the instant the answer is known, which is exactly the kind of "exit early" job `break` is suited for.

## Optimization: Looping Only to `n / 2`

Just like with printing factors, no factor of `n` (other than `n` itself) can exceed `n / 2`, so the loop's range can shrink:

```c
for (int i = 2; i <= n / 2; i++) {
    if (n % i == 0) {
        isPrime = 0;
        break;
    }
}
```

This checks fewer numbers but reaches the exact same conclusion — it's the same optimization idea from the factors topic, applied here.

## Prime Check vs Listing Factors

| | Listing Factors | Prime Check |
|---|---|---|
| Loop range | `1` to `n` | `2` to `n - 1` (or `n / 2`) |
| Purpose | Print/count *every* factor | Detect if *any* factor exists besides `1` and `n` |
| Early exit | Not needed — every factor matters | `break` used — one factor is enough to disprove primality |
