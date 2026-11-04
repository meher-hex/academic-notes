# Sorting Algorithms

Four sorting/searching algorithms, all written using plain array indexing — no pointers, no user-defined functions. Everything lives inside `main()`, using only loops, `if` statements, and a temporary variable/array where needed.

---

## 1. Bubble Sort

### Idea

Repeatedly step through the array, comparing each pair of **adjacent** elements. If they're in the wrong order, swap them. After each full pass, the largest unsorted element "bubbles up" to its correct position at the end — so each pass needs to check one fewer element than the last.

### Program

```c
#include <stdio.h>

int main() {
    int arr[6] = {5, 2, 9, 1, 5, 6};
    int n = 6;

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }

    printf("Sorted array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }

    return 0;
}
```

**Output:**
```
Sorted array: 1 2 5 5 6 9
```

### Why the Loop Bounds Are `n - 1` and `n - i - 1`

- Outer loop runs `n - 1` times — after `n - 1` passes, the array is guaranteed sorted (the last element needs no pass of its own).
- Inner loop shrinks by `i` each time (`n - i - 1`) because after each pass, the last `i` elements are already in their final, correct position — re-checking them would be wasted work.

### Dry Run (first pass only)

```
arr = [5, 2, 9, 1, 5, 6]

j=0: 5 > 2 → swap → [2, 5, 9, 1, 5, 6]
j=1: 5 < 9 → no swap
j=2: 9 > 1 → swap → [2, 5, 1, 9, 5, 6]
j=3: 9 > 5 → swap → [2, 5, 1, 5, 9, 6]
j=4: 9 > 6 → swap → [2, 5, 1, 5, 6, 9]

End of pass 1: largest element (9) has "bubbled" to the last position.
```

**Time Complexity:** `O(n²)` in the worst and average case — the nested loop always does roughly `n × n` comparisons.

---

## 2. Binary Search

### Idea

Binary search only works on a **sorted** array. Instead of checking every element one by one, it repeatedly cuts the search space in half: compare the target with the middle element, and discard the half that can't possibly contain it.

### Program

```c
#include <stdio.h>

int main() {
    int arr[7] = {2, 4, 6, 8, 10, 12, 14};
    int n = 7;
    int target = 10;

    int low = 0;
    int high = n - 1;
    int found = -1;   // stays -1 if not found

    while (low <= high) {
        int mid = (low + high) / 2;

        if (arr[mid] == target) {
            found = mid;
            break;
        } else if (arr[mid] < target) {
            low = mid + 1;    // target is in the right half
        } else {
            high = mid - 1;   // target is in the left half
        }
    }

    if (found != -1) {
        printf("Found at index %d\n", found);
    } else {
        printf("Not found\n");
    }

    return 0;
}
```

**Output:**
```
Found at index 4
```

### Dry Run

```
arr = [2, 4, 6, 8, 10, 12, 14]   (indices 0-6)
target = 10

low=0, high=6: mid=3 → arr[3]=8 < 10 → search right → low=4
low=4, high=6: mid=5 → arr[5]=12 > 10 → search left → high=4
low=4, high=4: mid=4 → arr[4]=10 == 10 → found at index 4
```

### Why It's Faster Than Checking Every Element

Each comparison eliminates **half** of the remaining elements, not just one. That's why binary search runs in `O(log n)` time instead of linear search's `O(n)` — for an array of 1,000,000 elements, binary search needs at most ~20 comparisons instead of up to 1,000,000.

**Important:** If the array isn't sorted first, binary search's logic breaks down completely — discarding "half" the array only makes sense because a sorted order guarantees everything on one side of `mid` is smaller and everything on the other side is bigger.

---

## 3. Insertion Sort

### Idea

Think of sorting playing cards in your hand: you build up a sorted portion of the array one element at a time, picking the next unsorted element and shifting it into its correct place among the already-sorted elements to its left.

### Program

```c
#include <stdio.h>

int main() {
    int arr[6] = {9, 5, 1, 4, 3, 8};
    int n = 6;

    for (int i = 1; i < n; i++) {
        int key = arr[i];       // element to be inserted
        int j = i - 1;

        // shift elements greater than key one position right
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;   // place key in its correct spot
    }

    printf("Sorted array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }

    return 0;
}
```

**Output:**
```
Sorted array: 1 3 4 5 8 9
```

### Dry Run

```
arr = [9, 5, 1, 4, 3, 8]

i=1: key=5, shift 9 right → [9, 9, 1, 4, 3, 8] → place 5 → [5, 9, 1, 4, 3, 8]
i=2: key=1, shift 9,5 right → place 1 → [1, 5, 9, 4, 3, 8]
i=3: key=4, shift 9,5 right → place 4 → [1, 4, 5, 9, 3, 8]
i=4: key=3, shift 9,5,4 right → place 3 → [1, 3, 4, 5, 9, 8]
i=5: key=8, shift 9 right → place 8 → [1, 3, 4, 5, 8, 9]
```

### Why `arr[i]` Is Saved as `key` First

Since the shifting loop overwrites `arr[j + 1] = arr[j]`, the original value at index `i` would be lost if it weren't saved into `key` beforehand — `key` holds onto the value being inserted while its old slot gets overwritten during shifting.

**Time Complexity:** `O(n²)` worst case (reverse-sorted input), but close to `O(n)` if the array is already nearly sorted — the inner `while` loop barely runs in that case.

---

## 4. Merge Sort

### Idea

Merge sort is normally explained top-down: split the array in half, recursively sort each half, then merge the two sorted halves back together. But since that relies on a function calling itself (recursion), here's the same result built **bottom-up**, using only loops:

1. Treat the array as already "sorted" in blocks of size `1` (a single element is trivially sorted).
2. Merge adjacent blocks of size `1` into sorted blocks of size `2`.
3. Merge adjacent blocks of size `2` into sorted blocks of size `4`.
4. Keep doubling the block size and merging until one block covers the whole array.

### Program

```c
#include <stdio.h>

int main() {
    int arr[8] = {38, 27, 43, 3, 9, 82, 10, 5};
    int n = 8;
    int temp[8];   // temporary array to hold merged results

    for (int width = 1; width < n; width = width * 2) {

        for (int i = 0; i < n; i = i + 2 * width) {
            int left = i;
            int mid = i + width;
            if (mid > n) mid = n;
            int right = i + 2 * width;
            if (right > n) right = n;

            // merge arr[left..mid) and arr[mid..right) into temp[left..right)
            int a = left, b = mid, k = left;

            while (a < mid && b < right) {
                if (arr[a] <= arr[b]) {
                    temp[k] = arr[a];
                    a++;
                } else {
                    temp[k] = arr[b];
                    b++;
                }
                k++;
            }
            while (a < mid) {       // leftover elements in left block
                temp[k] = arr[a];
                a++;
                k++;
            }
            while (b < right) {     // leftover elements in right block
                temp[k] = arr[b];
                b++;
                k++;
            }
        }

        // copy the merged results back into arr for the next pass
        for (int i = 0; i < n; i++) {
            arr[i] = temp[i];
        }
    }

    printf("Sorted array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }

    return 0;
}
```

**Output:**
```
Sorted array: 3 5 9 10 27 38 43 82
```

### Dry Run (block sizes doubling)

```
arr = [38, 27, 43, 3, 9, 82, 10, 5]

width=1 (merge pairs of 1 into blocks of 2):
  [27,38] [3,43] [9,82] [5,10]

width=2 (merge pairs of 2 into blocks of 4):
  [3,27,38,43] [5,9,10,82]

width=4 (merge the two blocks of 4 into one block of 8):
  [3,5,9,10,27,38,43,82]
```

### Why the Merge Step Needs Three `while` Loops

- The first `while` runs as long as **both** blocks still have unmerged elements, comparing their fronts and taking the smaller one.
- The moment one block runs out, that loop stops — but the *other* block might still have leftover elements that are all still bigger than everything already merged. The second and third `while` loops simply copy over whatever's left in each block, since it's already guaranteed to be in sorted order internally.

### Why a Temporary Array Is Needed

Merging in-place (directly inside `arr`) would overwrite values you still need to compare — e.g., overwriting `arr[left]` while `a` is still pointing at it. Writing merged results into `temp` first, then copying `temp` back into `arr` after each full pass, avoids that conflict entirely.

**Time Complexity:** `O(n log n)` — there are `log n` passes (doubling the width each time), and each pass does `O(n)` work merging, since every element is touched exactly once per pass.

---

## Quick Comparison

| Algorithm | Best Case | Worst Case | Needs Sorted Input? | Extra Memory? |
|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | No | No |
| Binary Search | O(1) | O(log n) | **Yes** | No |
| Insertion Sort | O(n) | O(n²) | No | No |
| Merge Sort | O(n log n) | O(n log n) | No | Yes (`temp` array) |

Binary search isn't a sorting algorithm at all — it's a *search* algorithm that depends on the array already being sorted, which is why it's grouped here alongside the sorting methods that would typically run before it.
