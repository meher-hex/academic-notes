# Character Input, Output, and Strings

## Character I/O — `getchar()` and `putchar()`

`getchar()` reads a **single character** from the input (keyboard), and `putchar()` writes a **single character** to the output (screen). Both work with one character at a time, unlike `scanf`/`printf` which can handle multiple values and formats.

```c
#include <stdio.h>

int main() {
    char ch;

    ch = getchar();   // reads one character from input
    putchar(ch);      // prints that character back

    return 0;
}
```

**Note:** `getchar()` returns an `int`, not a `char`. This matters because it needs to be able to return the special value `EOF` (which falls outside the range of a normal `char`).

---

## `char` and ASCII

A `char` variable actually stores a small integer — its **ASCII code**. `%c` prints it as a character; `%d` prints its underlying numeric value.

```c
char c = 'A';
printf("%c\n", c);   // A
printf("%d\n", c);   // 65  (ASCII value of 'A')
```

| Format Specifier | Interprets `char` as |
|---|---|
| `%c` | Character |
| `%d` | Integer (ASCII code) |

```c
char letter = 65;      // valid — 65 is ASCII for 'A'
printf("%c", letter);  // prints A
```

---

## Strings in C

**Definition:** A string in C is not a separate data type — it's an **array of characters terminated by a null character (`'\0'`)**. The null terminator marks where the string ends.

```c
char name[6] = "Hello";
// stored in memory as: 'H' 'e' 'l' 'l' 'o' '\0'
// size must be at least 6 to fit "Hello" + the null terminator
```

**Declaration and initialization:**
```c
char str1[20];               // uninitialized, holds up to 19 characters + '\0'
char str2[] = "World";       // size auto-calculated (6, including '\0')
char str3[10] = "Hi";        // remaining slots filled with '\0'
```

**Note:** Without the null terminator, functions like `printf("%s", ...)` won't know where the string ends and will keep reading garbage memory.

### Printing and Reading Strings

```c
#include <stdio.h>

int main() {
    char name[20];

    printf("Enter your name: ");
    scanf("%s", name);          // no & needed — array name already decays to an address

    printf("Hello, %s!\n", name);

    return 0;
}
```

**Note:** `scanf("%s", ...)` stops reading at the first whitespace — it cannot capture multi-word input (e.g., "John Smith" would only read "John").
