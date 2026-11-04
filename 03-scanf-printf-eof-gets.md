# `scanf()` and `printf()` In-Depth, EOF, and `gets()`

## `printf()` — Conversion Characters

`printf()` uses **format specifiers** (conversion characters) to determine how a value is interpreted and displayed.

| Specifier | Type | Example |
|---|---|---|
| `%d` | Signed integer | `printf("%d", 5);` → `5` |
| `%f` | Float/double | `printf("%f", 3.14);` → `3.140000` |
| `%c` | Character | `printf("%c", 'A');` → `A` |
| `%s` | String | `printf("%s", "Hi");` → `Hi` |
| `%u` | Unsigned integer | `printf("%u", 5);` → `5` |
| `%x` | Hexadecimal | `printf("%x", 255);` → `ff` |
| `%o` | Octal | `printf("%o", 8);` → `10` |
| `%%` | Literal `%` sign | `printf("%%");` → `%` |

### Width Specifiers (e.g., `%3d`)

A number placed between `%` and the conversion character sets a **minimum field width**. If the value has fewer digits than the width, it's padded with spaces (right-aligned by default).

```c
printf("%3d\n", 5);     // "  5"   (2 spaces + 5, total width 3)
printf("%3d\n", 500);   // "500"   (already 3 digits, no padding added)
printf("%-3d|\n", 5);   // "5  |"  (- makes it left-aligned instead)
```

**Note:** Width never truncates a value — if the number is wider than the specified width, the full number is still printed.

### Scientific Notation — `%e`

`%e` prints a floating-point value in **scientific notation**: one digit before the decimal point, followed by the fractional part, then `e` (exponent marker), a sign, and a 2-digit exponent.

```c
printf("%e\n", 25300.0);   // 2.530000e+04
printf("%e\n", 0.00253);   // 2.530000e-03
```

Reading it: `2.530000e+04` means `2.530000 × 10⁴` = `25300`. The notation `253e+02` similarly means `253 × 10²` = `25300` — the digits before `e` are the **mantissa**, and the number after `e` (with its sign) is the **power of 10** to multiply by.

### Width and Precision Together — `%8.3f`, `%15.3s`

The general form is `%width.precision` + conversion character. **Width** sets the minimum total space used; **precision**'s meaning depends on the type:

| Specifier | Width | Precision Means |
|---|---|---|
| `%8.3f` | 8 | 3 digits **after the decimal point** |
| `%15.3s` | 15 | Only the **first 3 characters** of the string are printed |

```c
printf("%8.3f\n", 3.14159);   // "   3.142"   (8 wide total, 3 decimal places, rounded)
printf("%15.3s\n", "Hello");  // "            Hel"  (15 wide total, only first 3 chars of "Hello")
```

**Note:** For `%f`, precision controls decimal places (default is 6 if omitted). For `%s`, precision controls the *maximum number of characters printed*, unlike width, which controls the *minimum total space*.

### Visualizing Field Width in Memory / Output

Width doesn't change how a value is stored in memory — it only affects how it's **printed** (padding with spaces in the output stream). It's easiest to visualize as boxes of fixed width in the output:

```
printf("%7f\n", 3.5);
Output (7 characters wide minimum — but %f defaults to 6 decimal places, so it often exceeds the width):
[3.500000]   <- actually 8 characters; width is a MINIMUM, not a cap

printf("%10.0f\n", 3.5);
Output (10 wide, 0 decimal places — precision 0 rounds to nearest whole number):
[         4]  <- 9 spaces + "4", rounded from 3.5
```

| Format | Width | Precision | Result for `3.5` |
|---|---|---|---|
| `%7f` | 7 (min) | default (6) | `3.500000` (8 chars — width is exceeded, not truncated) |
| `%10.0f` | 10 (min) | 0 decimals | `         4` (padded to 10, rounded to whole number) |

**Combined example** — same variable, three different specifiers in one `printf()`:

```c
float f = 123.45;
printf("%7f %7.1f %10.0f", f, f, f);
```

Working out each field separately (`_` = a blank space):

| Specifier | Precision used | Raw value | Width | Fits width? | Padded result |
|---|---|---|---|---|---|
| `%7f` | default → 6 decimals | `123.450000` (10 chars) | 7 | No — value is already longer than 7 | `123.450000` (no padding added) |
| `%7.1f` | 1 decimal | `123.5` (5 chars) | 7 | Yes — needs 2 more | `__123.5` |
| `%10.0f` | 0 decimals | `123` (3 chars, `.45` rounds down) | 10 | Yes — needs 7 more | `_______123` |

```
123.450000__123.5_______123
```
(the single spaces between fields above come from the literal spaces typed between `%7f`, `%7.1f`, and `%10.0f` in the format string itself, on top of any padding)

**Common misconception to avoid:** `%7f` does **not** mean "show 7 characters of `123.45`." Width only sets a *minimum* total length; **precision** (not width) controls decimal places, and `%f` defaults to 6 decimal places whenever no precision is explicitly given — that's why `%7f` still prints all six decimals (`123.450000`), not the shorter `123.45`.

### Flags — `-`, `+`, `0`, `#`

Flags appear right after `%`, before width/precision, and modify how the value is formatted.

| Flag | Effect | Example |
|---|---|---|
| `-` | Left-justify within the field width (default is right-justify) | `printf("%-5d\|", 3);` → `3    \|` |
| `+` | Force a leading sign (`+` or `-`) even on positive numbers | `printf("%+d\n", 5);` → `+5` |
| `0` | Pad with leading zeros instead of spaces | `printf("%05d\n", 42);` → `00042` |
| `#` | Alternate form — depends on conversion type | see below |

**`#` with `%o` / `%x`:** adds the prefix that identifies the base (`0` for octal, `0x` for hex).

```c
printf("%#o\n", 8);    // 010   (leading 0 marks it as octal)
printf("%#x\n", 255);  // 0xff  (0x prefix marks it as hex)
```

**`#` with `%e` / `%f` / `%g`:** forces a decimal point to always appear, and (for `%g` specifically) keeps trailing zeros that would otherwise be trimmed.

```c
printf("%.0f\n", 3.0);    // 3     (no decimal point, since precision is 0)
printf("%#.0f\n", 3.0);   // 3.    (# forces the decimal point to show anyway)

printf("%g\n", 1.500);    // 1.5   (trailing zero normally dropped)
printf("%#g\n", 1.500);   // 1.50000  (# keeps trailing zeros to fill the default precision)
```

**Note:** Multiple flags can be combined in one specifier, e.g. `%+05d` forces a sign AND zero-pads: `printf("%+05d\n", 5);` → `+0005`.

---

## `scanf()` — How It Actually Works

The first argument of `scanf()` — the string containing `%d`, `%c`, etc. — is called the **control string** (or format string). It tells `scanf()` what type(s) of data to expect and how to interpret the characters typed by the user. Everything after the control string is a list of **addresses** telling `scanf()` where to store each value it reads.

```c
int age;
scanf("%d", &age);   // "%d" is the control string; &age is where the result is stored
```

`scanf()` returns an `int`: the **number of items successfully read and assigned**. This return value is often ignored, but it becomes important for detecting bad or missing input.

```c
int a, b;
int count = scanf("%d %d", &a, &b);
// count == 2 if both were read successfully
// count == 1 if only the first was read
// count == 0 or EOF if nothing could be read
```

### Why No `&` for Strings

`scanf("%d", &age)` needs `&` because `age` is a plain variable — its name alone just holds a value, not a location. But `scanf("%s", name)` does **not** use `&`, because an array name **already represents an address**: specifically, the address of its **first element** (index 0).

```c
char name[6];
scanf("%s", name);      // correct — name already means &name[0]
scanf("%s", &name);     // also technically works, but is redundant/non-idiomatic
scanf("%s", &name[0]);  // identical meaning to just "name"
```

**How the rest of the string gets filled in:** `scanf()` only needs the address of the *first* character. From there, it writes the first character to that address, then moves to the **next memory address** (`address + 1`) for the second character, `address + 2` for the third, and so on — since array elements are stored in **contiguous memory**.

```
Input typed: "Hi"

Address:   1000   1001   1002
Index:     [0]    [1]    [2]
Value:     'H'    'i'    '\0'
           ^
           name (= &name[0], the address scanf() was given)
```

### Termination Character

`scanf("%s", ...)` automatically appends a **null character (`'\0'`)** right after the last character it reads — this is what marks the end of the string in memory (see diagram above, index `[2]`). This happens regardless of what character terminated the input (a space, tab, or newline) — that terminating whitespace itself is *not* stored; only `'\0'` is placed in its position.

---

## Scansets — `%[...]` and `%[^...]`

A **scanset** lets `scanf()` read only characters from (or excluding) a specific set, instead of stopping at the first whitespace like `%s` does.

| Specifier | Meaning |
|---|---|
| `%[abc]` | Read characters **only** while they are `a`, `b`, or `c` |
| `%[^abc]` | Read characters **until** one of `a`, `b`, or `c` is found (the `^` negates the set) |
| `%[a-z]` | Range form — reads only lowercase letters |

```c
char word[20];

scanf("%[^\n]", word);   // reads an entire line, stopping only at '\n' (spaces allowed)
// unlike plain %s, this does NOT stop at spaces
```

```c
char vowels[20];
scanf("%[aeiou]", vowels);   // keeps reading as long as characters are vowels
```

**Note:** A scanset does **not** automatically skip leading whitespace the way `%d` or `%s` do, so it's commonly paired with the leading-space trick below when reading after a numeric input.

---

## Recommended: A Space Before the First Conversion Character

When mixing numeric/character input with `%c` or a scanset, leftover whitespace (especially the `\n` left behind by pressing Enter) can be read as if it were real input. A **space in the control string before a conversion specifier** tells `scanf()` to skip *any* amount of leading whitespace (spaces, tabs, newlines) before reading that value.

```c
int n;
char ch;

scanf("%d", &n);     // user types: 5 <Enter>   → n = 5, but '\n' is left in the input buffer
scanf("%c", &ch);     // WITHOUT the fix: immediately reads that leftover '\n' as ch!

scanf(" %c", &ch);    // WITH a leading space: skips the leftover '\n' first, then reads the real character
```

**Note:** `%d`, `%f`, and `%s` already skip leading whitespace automatically — the leading-space fix mainly matters before `%c` and scansets (`%[...]`), which read whitespace literally by default.

---

## `%*d` — Assignment Suppression

Placing `*` right after `%` tells `scanf()` to **read and discard** a value instead of storing it anywhere — useful for skipping a value in the input you don't care about.

```c
int day, year;

// Input format: "14-09-2026" (day-month-year), but the month is not needed
scanf("%d-%*d-%d", &day, &year);
// The month value is read (so scanf's position in the input moves past it correctly)
// but it is thrown away — no variable receives it, and no & is passed for it
```

**On "garbage values":** A *garbage value* is whatever unpredictable leftover data happens to sit in a variable's memory before it's properly initialized or assigned. This is different from `%*d` — `%*d` doesn't create a garbage value, it deliberately *avoids* writing to memory at all for that item. Garbage values instead show up when a variable is **read or printed before ever being assigned**, e.g.:

```c
int x;
printf("%d\n", x);   // prints a garbage value — x was never initialized
```

---

## `EOF` (End Of File)

`EOF` is a special integer constant (usually `-1`, defined in `<stdio.h>`) that signals **no more input is available** — either the actual end of a file, or (when typing manually) the user sending an end-of-input signal from the keyboard (`Ctrl+D` on Linux/Mac, `Ctrl+Z` then Enter on Windows).

Both `getchar()` and `scanf()` can return `EOF`:
- `getchar()` returns `EOF` instead of a character when there's nothing left to read.
- `scanf()` returns `EOF` instead of a count when it fails to read *anything* because the input source is exhausted.

### Using EOF in an `if` statement

```c
#include <stdio.h>

int main() {
    int ch = getchar();

    if (ch == EOF) {
        printf("No input received.\n");
    } else {
        printf("You entered: %c\n", ch);
    }

    return 0;
}
```

### Using EOF to Loop Until Input Ends

This is the most common real use of `EOF` — reading an unknown amount of input until it runs out.

```c
#include <stdio.h>

int main() {
    char ch;

    printf("Type characters (Ctrl+D / Ctrl+Z to stop):\n");

    while ((ch = getchar()) != EOF) {
        putchar(ch);   // echo each character back
    }

    printf("\nInput ended.\n");
    return 0;
}
```

**Why the parentheses `(ch = getchar())`?** Assignment (`=`) has lower precedence than comparison (`!=`). Without parentheses, `ch = getchar() != EOF` would evaluate `getchar() != EOF` *first* (giving `1` or `0`), then assign that `1`/`0` into `ch` — not the actual character.

### Using EOF with `scanf()` in a loop

```c
#include <stdio.h>

int main() {
    int num, sum = 0;

    printf("Enter numbers (Ctrl+D / Ctrl+Z to stop):\n");

    while (scanf("%d", &num) != EOF) {
        sum += num;
    }

    printf("Sum = %d\n", sum);
    return 0;
}
```

**Note:** `scanf(...) != EOF` is preferred over `scanf(...) == 1` when reading an unknown number of values, because it specifically checks for the input source running out, rather than just one failed conversion.

---

## `gets()` — Reading a Full Line

Unlike `scanf("%s", ...)`, which stops at whitespace, `gets()` reads an **entire line** (including spaces) until a newline.

```c
#include <stdio.h>

int main() {
    char sentence[50];

    printf("Enter a sentence: ");
    gets(sentence);   // reads until Enter, spaces included

    printf("You entered: %s\n", sentence);
    return 0;
}
```

**Note:** `gets()` is considered **unsafe and deprecated** (removed from the C11 standard) because it has no way to limit how many characters it reads — if the input is longer than the array, it overflows adjacent memory (a **buffer overflow**). Modern code uses `fgets(sentence, 50, stdin)` instead, which takes a size limit as an argument.

## `puts()` — Printing a Full Line

`puts()` is the output counterpart to `gets()`: it prints a string followed automatically by a newline (`\n`), without needing a format specifier like `printf("%s", ...)` does.

```c
#include <stdio.h>

int main() {
    char sentence[50];

    printf("Enter a sentence: ");
    gets(sentence);
    puts(sentence);   // prints the string, then moves to a new line automatically

    return 0;
}
```

**Note:** `puts()` can only print a plain string — it cannot format or mix in other values the way `printf()` can, and it always appends `\n`, even if the string doesn't already end with one.
