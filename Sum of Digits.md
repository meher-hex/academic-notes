# Program: Sum of Digits of a Number

This program extracts each digit of a number one at a time using the modulus (`%`) and division (`/`) operators, and adds them up. It's a common building block for other digit-based problems (reversing a number, checking palindromes, Armstrong numbers, etc.).

## The Core Idea

Two operators do all the work:

- **`n % 10`** — gives the **last digit** of `n`.
- **`n / 10`** — **removes** the last digit of `n` (integer division drops the remainder).

Repeating this pair inside a loop peels off one digit at a time, from right to left, until nothing is left.

```c
int n = 1234;
int sum = 0;

while (n != 0) {
    int digit = n % 10;   // extract last digit
    sum = sum + digit;    // add it to the running total
    n = n / 10;           // remove last digit
}

printf("Sum of digits: %d\n", sum);
```

**Output:** `Sum of digits: 10`

### How It Works, Step by Step

For `n = 1234`:

| Step | `n` before | `n % 10` (digit) | `sum` after adding | `n / 10` (n after) |
|---|---|---|---|---|
| 1 | 1234 | 4 | 0 + 4 = 4 | 123 |
| 2 | 123 | 3 | 4 + 3 = 7 | 12 |
| 3 | 12 | 2 | 7 + 2 = 9 | 1 |
| 4 | 1 | 1 | 9 + 1 = 10 | 0 |

Once `n` becomes `0`, the `while (n != 0)` condition is false, and the loop stops. `sum` now holds `1 + 2 + 3 + 4 = 10`.

A `while` loop (rather than `for`) is the natural fit here, since the number of digits — and therefore the number of iterations — isn't known in advance.

## Full Program

```c
#include <stdio.h>

int main() {
    int n, sum = 0;

    printf("Enter a number: ");
    scanf("%d", &n);

    while (n != 0) {
        int digit = n % 10;
        sum = sum + digit;
        n = n / 10;
    }

    printf("Sum of digits: %d\n", sum);

    return 0;
}
```

**Sample Input:** `9081`
**Output:** `Sum of digits: 18`

## Handling Negative Numbers

If `n` is negative, `n % 10` can return a negative digit (e.g. `-4` instead of `4`), which would throw off the sum. Taking the absolute value first avoids this:

```c
if (n < 0) {
    n = -n;   // work with the positive version instead
}
```

Placing this check before the loop ensures every extracted digit is positive, regardless of the sign of the original input.

## Why the Original `n` Gets Destroyed

Notice the loop **modifies `n` directly** (`n = n / 10`) — by the time the loop ends, `n` is `0`, and the original number is gone unless it was saved elsewhere first. If the original value is needed later in the program (e.g. to print "Sum of digits of 1234 is 10"), it should be copied into another variable *before* the loop starts:

```c
int original = n;   // save a copy before n gets consumed
// ... loop modifies n as usual ...
printf("Sum of digits of %d is %d\n", original, sum);
```
