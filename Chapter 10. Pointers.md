**Pointer:** A pointer is a *variable* that represents the *location* (rather than the value) of a data item, such as a variable or an array element.

The address of a variable's memory location can be determined by the expression `&variable`, where & is a *unary* operator, called the *address operator*, that evaluates the address of its operand.

	int v = 10;
	int *pv = &v;

The data item represented by v (i.e., the data item stored in v’s memory cells) can be accessed by the expression `*pv`, where * is a *unary* operator, called the *indirection operator*, that operates *only* on a pointer variable. Therefore, `*pv` and v both represent the same data item (i.e., the contents of the same memory cells).

Adjacent memory cells within a computer are numbered consecutively, from the beginning to the end of the memory area. *The number associated with each memory cell is known as the memory cell’s address.* Most computers use a hexadecimal numbering system to designate the addresses of consecutive memory cells, though some computers use an octal numbering system.

```c
#include <stdio.h>
  
int main() {

    int a = 10, b; // b contains garbage data, it lives on the stack
    int *pa, *pb; // uninitialized pointer variables contain indeterminate garbage memory address

    pa = &a; // assigning the memory address of 'a' to 'pa'
    b = *pa; // assigning the VALUE of 'a' to 'b' by accessing the memory of 'a' through 'pa'
    pb = &b; // assigning the memory address of 'b' to 'pb'

    printf("a = %d, pa = %x, *pa = %d\n", a, pa, *pa); // output: 10 61ff14 10
    printf("b = %d, pb = %x, *pb = %d\n", b, pb, *pb); // output: 10 61ff10 10

    return 0;
}
```

Uninitialized pointers are known as *wild* pointers. Attempting to read or dereference (modify the memory it points to) an uninitialized local pointer causes **undefined behavior**.

- `b = *pa;` ---> `*pa` accesses the value of the variable it is pointing to, which is a. the value is 10, so 10 gets stored in b.
- %x or %X is used for printing pointers (difference: x means the memory address will be printed in lowercase while X means it will be printed in uppercase).

***Pointer must be of the same type as the variable it is pointing to.*** Multiple pointers can point to the same variable. *The variable a pointer is pointing to must be declared earlier in the program.*

The use of a pointer as a function argument permits the corresponding data item to be altered globally from within the function. Meaning, if the data item is altered inside the function, then it is altered globally as well (as in everywhere it is used in the entire program).

### Passing Pointers to Functions
```c
#include <stdio.h>

void func_by_val (int a, int b) {
    a = 2;
    b = 3;
    printf("Within func_by_val: a = %d, b = %d\n", a, b); // output: 2 3
    return;
}

void func_by_ref (int *pa, int *pb) { // even (int *, int *) would have worked
    *pa = 2; 
    *pb = 3;
    printf("Within func_by_ref: a = %d, b = %d\n", *pa, *pb); // output: 2 3
    return;
}

int main() {
    int a = 10, b = 5;

    printf("Before function call: a = %d, b = %d\n", a, b); // output: 10 5
    func_by_val(a, b);
    printf("After function call: a = %d, b = %d\n", a, b); // output: 10 5
    
    printf("Before function call: a = %d, b = %d\n", a, b); // output: 10 5
    func_by_ref(&a, &b);
    printf("After function call: a = %d, b = %d\n", a, b); // output: 2 3

    return 0;
}
```

An array name is treated as a pointer when it is passed to a function. In C, arrays are always passed to functions by reference, meaning the function receives the memory address of the first element rather than a copy of the entire array. This behavior is known as **array decay**.

```c
#include <stdio.h>

void Print(char line[]) {
    for (int i = 0; line[i] != 0; i++)
        printf("%c", line[i]);
    return;
}

int main() {
    char str[] = "Bangladesh";

    Print(str); // Bangladesh
    return 0;
}
```

```c
#include <stdio.h>

void Print(char *line) {
    while (*line) {
        printf("%c", *line);
        line++;
    }
}

int main() {
    char str[] = "Bangladesh";
    Print(str); // Bangladesh
    return 0;
}
```

A function can return a pointer to the calling portion of the program. The function definition and any corresponding function declarations must indicate that the function will return a pointer. This is
accomplished by preceding the function name by an asterisk. The asterisk must appear in both the function definition and the function declarations.

### Static Memory Allocation
The memory allocated during compile time is called static memory. This memory is fixed, i.e. cannot be increased or decreased during run time.

### Dynamic Memory Allocation
The process of allocating memory at the time of execution is called dynamic memory allocation.

**Heap:** A segment of memory where dynamic memory allocation takes place. An area of memory where memory is allocated or deallocated without any order or randomly.

Built-in Functions used for this: malloc(), calloc(), realloc(), free()
##### <u>malloc()</u>
Declared in <stdlib.h>. Short for memory allocation, used to dynamically allocate a single large block of contiguous memory according to the size specified.

allocates a memory block according to the size specified in the *heap*, and on success, returns a pointer to the first byte of the allocated memory, else returns NULL.

Syntax: (void* )malloc(size_t size) // size_t is defined as unsigned int in <stdlib.h>

``` c
int *ptr = (int *) malloc(4); // 4 bytes stored in heap, ptr points to first byte
```
##### <u>calloc()</u>
Allocates multiple blocks. Takes two arguments instead of one. Memory allocated by calloc is initialized to zero. Stands for clear allocation.

``` c
int *ptr = (int *) calloc(10, sizeof(int));
```

Both malloc and calloc return NULL when sufficient memory is not available in heap.
## Linked List

Types:
1. Single Linked List - Navigation forward only
2. Doubly Linked List - Navigation both ways possible
3. Circular Linked List - Last element linked to first element
#### Single Linked List
List made up of nodes consisting of two parts.
- data - contains actual data
- link - contains address of the next node of the list
First node is accessed with a pointer. It's called *head*. Last node's link contains NULL.