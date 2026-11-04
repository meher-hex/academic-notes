# Declaring Variables — I/O Formatting and Memory Behavior

This covers a set of practical details usually shown together in a demo program: how input/output formatting works, rounding vs. ceiling, storing hex values, checking memory size, viewing memory addresses, and how memory allocation actually behaves.

---

## Space and Newline as Terminators in `scanf()`

When reading input with `scanf()`, a **space, tab, or newline** signals the end of one input value — it doesn't get consumed as part of the value itself (for numeric and `%s` reads).

```c
int a, b;
scanf("%d %d", &a, &b);   // typing "5 10" — space separates the two values
```

For `%c` (single character), this becomes tricky — `scanf("%c", &ch)` will happily read a leftover space or newline character sitting in the input buffer from a previous read, rather than skipping it. A common fix is adding a leading space in the format string: `scanf(" %c", &ch);` — this tells `scanf` to skip any leading whitespace (including leftover newlines) before reading the character.

---

## Formatting Output — `%.10lf` and Precision Control

By default, `printf("%f", ...)` shows 6 digits after the decimal point. You can control this precisely:

```c
double pi = 3.14159265358979;

printf("%f", pi);        // 3.141593   (default: 6 decimal places)
printf("%.2f", pi);      // 3.14       (2 decimal places)
printf("%.10lf", pi);    // 3.1415926536   (10 decimal places, 'l' marks it as double)
```

**Breaking down `%.10lf`:**
- `%` — start of format specifier
- `.10` — show exactly 10 digits after the decimal point
- `l` — indicates the value is a `double` (technically optional for `printf`, but good practice and required for `scanf`)
- `f` — floating-point format

---

## Rounding vs. Ceiling

These are different operations, both from `math.h`:

```c
#include <math.h>

double x = 4.3;

round(x);   // 4.0 — rounds to nearest whole number
ceil(x);    // 5.0 — always rounds UP, regardless of decimal value
floor(x);   // 4.0 — always rounds DOWN, regardless of decimal value
```

```c
double y = 4.7;

round(y);   // 5.0
ceil(y);    // 5.0
floor(y);   // 4.0
```

**Key distinction:** `round()` looks at the decimal value to decide direction; `ceil()` and `floor()` always go one direction no matter what the decimal is (even `4.01` rounds up to `5` with `ceil()`).

---

## Storing and Printing Hexadecimal Values

```c
int num = 0x1A;         // storing a hex literal (equals 26 in decimal)

printf("%d", num);      // 26   — printed as decimal
printf("%x", num);      // 1a   — printed as hexadecimal (lowercase)
printf("%X", num);      // 1A   — printed as hexadecimal (uppercase)
```

Internally, the value is stored the same way regardless of the number system used to write or display it — hex is just a different way of *reading/writing* the same binary value.

---

## `sizeof()` Operator

Returns the number of bytes a data type or variable occupies in memory.

```c
printf("%lu", sizeof(int));      // 4 (typically)
printf("%lu", sizeof(char));     // 1
printf("%lu", sizeof(double));   // 8
printf("%lu", sizeof(a));        // works on a variable name too, not just a type
```

`%lu` is used since `sizeof()` returns an `unsigned long` value.

---

## Viewing a Variable's Memory Address

The `&` (address-of) operator reveals *where* a variable is stored in memory, not its value.

```c
int a = 10;
printf("%p", &a);   // prints something like 0x7ffee3a1c9ac
```

`%p` is the format specifier for printing a pointer/address. This is just a demonstration of *seeing* an address — not full pointer usage (declaring pointer variables, dereferencing, etc. — that's a separate, deeper topic).

---

## How Memory Allocation Actually Works

**Regular variables are not guaranteed to be stored in consecutive/sequential memory addresses**, even if declared one after another:

```c
int a = 1;
int b = 2;
int c = 3;

printf("%p\n", &a);
printf("%p\n", &b);
printf("%p\n", &c);
// addresses may NOT be neatly sequential (e.g., not exactly 4 bytes apart)
```

**Array elements, however, ARE guaranteed to be stored in contiguous (back-to-back) memory:**

```c
int arr[3] = {1, 2, 3};

printf("%p\n", &arr[0]);
printf("%p\n", &arr[1]);
printf("%p\n", &arr[2]);
// these WILL be sequential — each exactly sizeof(int) bytes apart
```

**Why this matters:** This contiguous layout is precisely what allows array indexing (`arr[i]`) to work efficiently — the compiler can calculate any element's address instantly using simple math (`base address + i × size of one element`), instead of searching for it. Regular separate variables have no such guarantee, since the compiler/OS can place them wherever it finds convenient space.
