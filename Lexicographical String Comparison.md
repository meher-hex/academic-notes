# Lexicographical String Comparison

A common competitive-programming pattern: compare two strings the way a dictionary would, and report which one comes first. The specific variant here — print `-1` if the first string is lexicographically **greater** than the second — is a classic Codeforces-style problem.

## What "Lexicographical Order" Means

Lexicographical order is just **dictionary order**, applied character by character using ASCII values:

- Compare the strings character by character, left to right.
- The first position where the characters differ decides the result.
- If one string is a prefix of the other (e.g., `"cat"` vs `"catalog"`), the **shorter** one is considered smaller.
- Comparison is based on ASCII value, so uppercase letters (`'A'`–`'Z'` = 65–90) are considered "smaller" than lowercase letters (`'a'`–`'z'` = 97–122).

```
"apple"  vs "banana"   → 'a' (97) < 'b' (98)  → "apple" is smaller
"cat"    vs "car"      → 'c'='c', 'a'='a', 't' (116) > 'r' (114) → "cat" is bigger
"cat"    vs "catalog"  → "cat" is a prefix of "catalog" → "cat" is smaller (shorter wins)
```

## Manual Character-by-Character Comparison

```c
#include <stdio.h>

int main() {
    char s1[100], s2[100];

    scanf("%s", s1);
    scanf("%s", s2);

    int i = 0;
    int result = 0;   // 0 = equal so far

    while (s1[i] != '\0' && s2[i] != '\0') {
        if (s1[i] != s2[i]) {
            if (s1[i] > s2[i]) {
                result = 1;    // s1 is bigger
            } else {
                result = -1;   // s1 is smaller
            }
            break;
        }
        i++;
    }

    // if loop ended without a mismatch, check which string ran out first
    if (result == 0) {
        if (s1[i] != '\0') result = 1;       // s2 ended first -> s1 is longer -> s1 is bigger
        else if (s2[i] != '\0') result = -1; // s1 ended first -> s2 is bigger
        // if both ended together, strings are equal, result stays 0
    }

    if (result > 0) {
        printf("-1\n");   // s1 is bigger than s2
    } else {
        printf("1\n");
    }

    return 0;
}
```

### Walking Through the Logic

1. Loop through both strings simultaneously using index `i`, only while **neither** has hit its null terminator `'\0'`.
2. The moment a mismatched character is found, that single comparison decides the result — no need to look further, since the first difference always determines lexicographical order.
3. If the loop finishes with no mismatch (one full run without `break`), it means one string was a complete prefix of the other — whichever string still has characters left (`!= '\0'`) is the **longer**, and therefore lexicographically **bigger**, one.
4. Finally, translate the result: if `s1` turned out bigger, print `-1`; otherwise print `1`.

## Why Check the Null Terminator (`'\0'`)

C strings aren't stored with an explicit length — they're just `char` arrays terminated by a `'\0'` byte marking "end of string". The `while` loop condition `s1[i] != '\0' && s2[i] != '\0'` is what prevents reading past the end of whichever string is shorter — without it, you'd walk off into uninitialized memory once the shorter string's characters run out.

## Common Pitfall: Comparing with `==`

```c
if (s1 == s2)   // WRONG - compares memory addresses, NOT string contents
```

Since array names decay to pointers, `s1 == s2` checks whether both point to the *same memory location* — almost never true for two separately declared arrays, even if their contents are identical. You always need to walk the characters yourself (as above) to actually compare what's *inside* two C strings — this is one of the most common beginner mistakes with C strings.

## Dry Run Example

```
s1 = "hello"
s2 = "helzo"

i=0: 'h' == 'h' -> continue
i=1: 'e' == 'e' -> continue
i=2: 'l' == 'l' -> continue
i=3: 'l' (108) vs 'z' (122) -> mismatch, s1[3] < s2[3] -> result = -1

Output: 1   (s1 is NOT bigger than s2, so print 1)
```
