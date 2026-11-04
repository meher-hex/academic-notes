# Arrays

An **array** is a collection of elements of the *same data type*, stored in contiguous memory locations, accessed using a single variable name and an index.

```c
int marks[5];          // declares an array of 5 integers
float prices[10];      // declares an array of 10 floats
```

## Why Arrays?

Without arrays, storing 100 student marks would mean declaring 100 separate variables (`m1, m2, ..., m100`). Arrays let you group related data under one name and access any element using its **index**.

## 1. Declaration and Initialization

```c
int arr[5];                          // declaration only — values are garbage/unpredictable
int arr[5] = {1, 2, 3, 4, 5};        // declaration + initialization
int arr[] = {1, 2, 3, 4, 5};         // size inferred from number of elements → 5
int arr[5] = {1, 2};                 // remaining elements auto-initialized to 0 → {1,2,0,0,0}
int arr[5] = {0};                    // initializes entire array to 0
```

## 2. Indexing

Array indices in C start at `0`, not `1`.

```c
int arr[5] = {10, 20, 30, 40, 50};

arr[0]   // 10 — first element
arr[4]   // 50 — last element (index = size - 1)
arr[5]   // ❌ out of bounds — undefined behavior, not a compile error
```

**Important:** C does **not** check array bounds at runtime. Accessing `arr[5]` on a 5-element array won't crash immediately — it reads whatever memory happens to be sitting past the array, which can silently corrupt data or crash later.

## 3. Traversing an Array

```c
int arr[5] = {10, 20, 30, 40, 50};

for (int i = 0; i < 5; i++) {
    printf("%d ", arr[i]);
}
```

The loop condition `i < 5` (not `i <= 5`) is what keeps the loop inside valid bounds.

## 4. Arrays and Memory

An array's elements are stored **contiguously** (back-to-back) in memory. The array name itself decays into a **pointer to the first element** in most expressions.

```c
int arr[5] = {10, 20, 30, 40, 50};

printf("%d", *arr);       // 10 — same as arr[0]
printf("%d", *(arr + 2)); // 30 — same as arr[2]
```

This is why `arr[i]` and `*(arr + i)` are interchangeable in C — `arr[i]` is really just syntactic sugar for pointer arithmetic.

## 5. Passing Arrays to Functions

Arrays are **passed by reference** automatically (not by value) — the function receives a pointer to the original array, not a copy.

```c
void modify(int a[], int size) {
    a[0] = 100;   // modifies the original array
}

int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    modify(arr, 5);
    // arr[0] is now 100
}
```

Because the array decays to a pointer, the function has no way to know the array's size on its own — that's why `size` is passed explicitly as a second argument.

---

## 2D Arrays

A **2D array** is essentially an array of arrays — think of it as a grid with rows and columns.

```c
int matrix[3][4];   // 3 rows, 4 columns

int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Partial Row Initialization

If a row is given fewer values than the declared number of columns, the missing elements in *that row* are auto-filled with `0` — same rule as with 1D arrays, just applied per-row.

```c
int nums[3][4] = {
    {1, 2, 3},        // only 3 values given, but row has 4 columns
    {5, 6, 7, 8},     // fully specified — 4 values for 4 columns
    {9, 10, 11}       // only 3 values given
};
```

This results in:

```
nums[0] = {1, 2, 3, 0}     // missing 4th value → padded with 0
nums[1] = {5, 6, 7, 8}     // no padding needed
nums[2] = {9, 10, 11, 0}   // missing 4th value → padded with 0
```

Each row is padded **independently** — a short row doesn't "borrow" values from the next row. The number of columns per row always stays fixed at `4`; only the unfilled slots within each row default to `0`.

### Indexing a 2D Array

```c
matrix[row][col]

matrix[0][0]   // 1  — first row, first column
matrix[1][2]   // 6  — second row, third column
```

### Traversing a 2D Array

```c
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        printf("%d ", matrix[i][j]);
    }
    printf("\n");
}
```

The outer loop moves down **rows**, the inner loop moves across **columns** — this nested-loop pattern is the standard way to walk a grid.

### Memory Layout: Row-Major Order

C stores 2D arrays in **row-major order** — meaning the entire first row is stored in memory, followed by the entire second row, and so on.

```
matrix[2][3] = {{1,2,3},{4,5,6}}

Memory: 1 2 3 4 5 6
        └row 0┘└row 1┘
```

This matters because `matrix[i][j]` is internally calculated as an offset: `*(matrix + i * cols + j)`.

---

## 3D Arrays

A **3D array** extends the same idea one level further — an array of 2D arrays. Useful for representing things like multiple matrices stacked together, or a cube of data.

```c
int cube[2][3][4];   // 2 blocks, each 3 rows × 4 columns

int cube[2][2][2] = {
    {
        {1, 2},
        {3, 4}
    },
    {
        {5, 6},
        {7, 8}
    }
};
```

### Indexing a 3D Array

```c
cube[block][row][col]

cube[0][0][0]   // 1
cube[1][1][1]   // 8
```

### Traversing a 3D Array

```c
for (int i = 0; i < 2; i++) {          // block
    for (int j = 0; j < 2; j++) {      // row
        for (int k = 0; k < 2; k++) {  // column
            printf("%d ", cube[i][j][k]);
        }
    }
}
```

Same row-major logic applies, just extended one dimension further: the whole first block is stored before the second block begins.

## Note on Array Size and `sizeof`

```c
int arr[5];
int size = sizeof(arr) / sizeof(arr[0]);   // 5 — total bytes ÷ bytes per element
```

This trick only works on the *actual array*, not on a pointer to it (e.g., inside a function where the array has decayed to a pointer) — which is exactly why size must be passed as a separate parameter to functions, as noted above.

### Q. Why is '&' not placed before the name of any string variable to take a string as input using scanf() function?
A: A string variable is a character array. The scanf() function requires the memory address of the first index for input, and the name of an array already indicates the memory address of its first element. Hence, '&' is not required. 