# Matrix Sum and Product

Two classic array-based programs: adding two matrices, and multiplying two matrices. Both rely directly on the 2D array indexing and nested-loop traversal covered earlier.

## 1. Sum of Two Matrices

### Rule

Matrix addition is only defined when **both matrices have the same dimensions** (same number of rows and same number of columns). The sum is computed **element-wise** — each element in the result is the sum of the corresponding elements in the two input matrices.

```
C[i][j] = A[i][j] + B[i][j]
```

### Program

```c
#include <stdio.h>

int main() {
    int A[3][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    int B[3][3] = {
        {9, 8, 7},
        {6, 5, 4},
        {3, 2, 1}
    };

    int C[3][3];

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            C[i][j] = A[i][j] + B[i][j];
        }
    }

    printf("Sum of matrices:\n");
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

**Output:**
```
Sum of matrices:
10 10 10
10 10 10
10 10 10
```

### Why the Loop Structure Works

A single nested loop (`i` for rows, `j` for columns) is enough because addition only ever touches **matching positions** — there's no need to compare an element against anything outside its own `[i][j]` slot, unlike multiplication below.

---

## 2. Product of Two Matrices

### Rule

Matrix multiplication is **not** element-wise — it's fundamentally different from addition. For `A × B` to be valid:

```
columns of A  must equal  rows of B
```

If `A` is of size `m × n` and `B` is of size `n × p`, the result `C` is of size `m × p`. Each element `C[i][j]` is computed as the **dot product** of row `i` of `A` and column `j` of `B`:

```
C[i][j] = Σ (A[i][k] * B[k][j])   for k = 0 to n-1
```

### Visualizing One Cell

For `C[0][0]`, you take row `0` of `A` and column `0` of `B`, multiply corresponding elements, and sum them:

```
A row 0:    [1  2  3]
B col 0:    [7]
            [8]
            [9]

C[0][0] = (1*7) + (2*8) + (3*9) = 7 + 16 + 27 = 50
```

### Program

```c
#include <stdio.h>

int main() {
    int A[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };

    int B[3][2] = {
        {7, 8},
        {9, 10},
        {11, 12}
    };

    int C[2][2] = {0};   // result matrix, initialized to 0

    // rows of A
    for (int i = 0; i < 2; i++) {
        // columns of B
        for (int j = 0; j < 2; j++) {
            // shared dimension (cols of A == rows of B)
            for (int k = 0; k < 3; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }

    printf("Product of matrices:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

**Output:**
```
Product of matrices:
58 64
139 154
```

### Why Three Nested Loops

- `i` walks down the **rows of A** (and rows of the result).
- `j` walks across the **columns of B** (and columns of the result).
- `k` walks along the **shared dimension** — columns of `A` / rows of `B` — accumulating the dot product into `C[i][j]`.

This is why matrix multiplication is `O(m × n × p)` — noticeably more expensive than addition's `O(m × n)`, since every output cell requires its own full inner loop rather than a single lookup.

**Important:** `C[i][j] += ...` relies on `C` being initialized to `0` beforehand (`int C[2][2] = {0};`) — without that, the accumulation would start from garbage values instead of `0`.

## Note on Dimension Mismatch

If you tried `A[2][3] × B[2][2]` (columns of `A` = 3, but rows of `B` = 2), the multiplication is mathematically undefined. C won't stop you from writing the loops — it will just read/write out of bounds or produce garbage results, since C performs no automatic dimension checking. It's on the programmer to verify `cols(A) == rows(B)` before multiplying.
