# Program: Printing Patterns

Pattern printing is where **nested loops** really come together: the **outer loop** controls the row, and the **inner loop** controls what gets printed on that row. Almost every pattern boils down to figuring out, for each row, *how many* things to print and *what* to print them with.

## The Golden Rule

For row `i`, the inner loop decides how many characters go on that line. Everything else is just adjusting that count and adding/removing spaces.

```c
for (int i = 1; i <= rows; i++) {       // controls the row
    for (int j = 1; j <= i; j++) {      // controls what's printed on that row
        printf("*");
    }
    printf("\n");                       // move to the next line after each row
}
```

The `printf("\n")` sits **outside** the inner loop but **inside** the outer loop — it runs once per row, after that row's stars are done, moving output to a new line.

## 1. Right Triangle — Without Spaces

The simplest pattern. Row `i` prints exactly `i` stars, with no gaps.

```c
int rows = 5;

for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= i; j++) {
        printf("*");
    }
    printf("\n");
}
```

**Output:**
```
*
**
***
****
*****
```

### How It Works

- Outer loop `i` runs from `1` to `5` — one pass per row.
- Inner loop `j` runs from `1` to `i` — so row `1` prints `1` star, row `2` prints `2` stars, and so on, since the inner loop's upper limit grows with `i`.

## 2. Right Triangle — With Spaces Between Stars

Same shape, but a space is printed after each star instead of nothing.

```c
int rows = 5;

for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= i; j++) {
        printf("* ");
    }
    printf("\n");
}
```

**Output:**
```
* 
* * 
* * * 
* * * * 
* * * * * 
```

Only the inner `printf` changed — `"* "` instead of `"*"`. The loop structure controlling *how many* stars per row stays identical; only *what's printed* per star changed.

## 3. Upper Triangle (Right-Aligned) — With Spaces

This is where **leading spaces** come in. To make the triangle lean right instead of left, each row needs a decreasing number of blank spaces printed *before* the stars — this needs a **second inner loop**, run before the star-printing loop.

```c
int rows = 5;

for (int i = 1; i <= rows; i++) {
    // leading spaces: more on early rows, fewer on later rows
    for (int j = 1; j <= rows - i; j++) {
        printf("  ");   // two spaces to line up with "* "
    }
    // stars: fewer on early rows, more on later rows
    for (int k = 1; k <= i; k++) {
        printf("* ");
    }
    printf("\n");
}
```

**Output:**
```
        * 
      * * 
    * * * 
  * * * * 
* * * * * 
```

### How It Works

- A row now has **two inner loops back to back**: one for spaces, one for stars.
- The spaces loop runs `rows - i` times — on row `1` that's `5 - 1 = 4` blocks of spaces (the most), and on row `5` it's `5 - 5 = 0` (none) — so the left padding **shrinks** as the star count **grows**, which is what pushes the triangle's shape toward the right/upper edge.
- The stars loop is unchanged from before — still `i` stars on row `i`.

This is the "upper triangle" pattern: the stars sit against the top-right, since each row's stars are right-aligned by the shrinking space prefix.

## 4. Upper and Lower Triangle Combined — With Spaces (Diamond)

Stacking an **upper triangle** on top of a **lower (inverted) triangle** produces a diamond. This means running the pattern logic twice: once counting rows **up** to the widest row, then once counting back **down**.

```c
int rows = 5;

// Upper half: grows from 1 star up to `rows` stars
for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= rows - i; j++) {
        printf("  ");
    }
    for (int k = 1; k <= i; k++) {
        printf("* ");
    }
    printf("\n");
}

// Lower half: shrinks back down from rows - 1 stars to 1 star
for (int i = rows - 1; i >= 1; i--) {
    for (int j = 1; j <= rows - i; j++) {
        printf("  ");
    }
    for (int k = 1; k <= i; k++) {
        printf("* ");
    }
    printf("\n");
}
```

**Output:**
```
        * 
      * * 
    * * * 
  * * * * 
* * * * * 
  * * * * 
    * * * 
      * * 
        * 
```

### How It Works

- The **upper half** is exactly the "upper triangle" pattern from section 3, unchanged — `i` climbs from `1` to `rows`.
- The **lower half** is the same body of code, but the outer loop now **counts down**, starting at `rows - 1` (not `rows`, to avoid printing the widest row twice) and decreasing to `1`.
- Because the space-loop formula (`rows - i`) and the star-loop formula (`i` stars) are reused unchanged in both halves, the padding math automatically mirrors itself as `i` shrinks back down — that's what makes the bottom half a reflection of the top half.

## Key Takeaways for Any Pattern

| Question to ask | Controls |
|---|---|
| How many rows? | The outer loop's range |
| How many symbols on row `i`? | The inner loop's range (often `i`, `rows - i + 1`, or similar) |
| Any leading spaces? | A separate inner loop *before* the symbol loop, usually shrinking as the symbol count grows |
| Does the shape invert partway? | Run the same logic twice: once with `i` increasing, once with `i` decreasing |

Getting a pattern right is almost always a matter of writing out the **spaces and symbols per row on paper first**, spotting the arithmetic relationship between the row number `i` and those counts, then translating that relationship directly into the inner loops' bounds.
