A function is a self-contained program segment that carries out some specific, well-defined task.
#### Function Declaration:
Also called *function prototype*. Refers to the declaration of the properties of a function to the compiler. Function prototypes are not mandatory in C.
###### Properties:
1. Name of the function
2. Return type of the function
3. Number of parameters
4. Type of parameter n

Mentioning the name of the parameters is not necessary in the *function prototype*, but it is a must for function definition. 
##### Implicit Declaration:
If a function is called before it is declared or defined (if function prototype does not exist either), then the compiler implicitly assumes that the return type of the function is *int*. If the return type of the function isn't *int*, then a conflict occurs, producing the *conflicting types* error. This implicit assumption of the compiler is considered implicit declaration. 

If the data type of the arguments are not explicitly mentioned, they are assumed to be of *int* type.
#### <u>Parameter vs Argument: </u>
**Parameter:** A variable in the declaration and definition of a function. Also called *formal parameter*, or *formal argument*. Basically, placeholders that will receive values when the function is called. These are local to the function, meaning the rest of the program would not know about these.

**Argument:** Actual value of the parameter that gets passed to the function. Also called *actual parameter*, or *actual argument.*

*Formal argument and actual argument must share the same data type.*
Every function body contains compound statement, where there should be at least *one return* statement. If a function is declared to return a type like `int` or `float`, every path through it should return a value of that type. If it doesn't need to return anything, declare it `void`.
#### Call by Value:
Value of the actual parameters get copied to the formal parameters, and these two parameters store value in two different locations.

``` c
int func_by_val (int a, int b) {
	a = 2;
	b = 3;
}

int main() {
	int a = 1, b = 10;
	func_by_val(a, b);
	printf("%d %d", a, b); // output: a = 1, b = 10
}
```

#### Call by Reference:
Both actual and formal parameter refer to the same memory location. Hence, any changes made to the formal parameters will reflect in the actual parameters. Here, memory address is passed to the function, instead of value.
``` c
int func_by_val (int *pa, int *pb) {
	*pa = 2; // * dereference operator, & address/reference operator
	*pb = 3;
}

int main() {
	int a = 1, b = 10;
	func_by_val(&a, &b);
	printf("%d %d", a, b); // output: a = 2, b = 3
}
```

### <u>Recursion:</u>
Recursion is a process by which a function calls itself repeatedly, until some specified condition has been satisfied. When a recursive program is executed, the recursive function calls are not executed immediately. Rather, they are placed on a *stack* until the condition that terminates the recursion is encountered. The function calls are then executed in reverse order, as they are “popped” off the stack.

<b><u>Stack:</u></b> A stack is a *last-in, first-out data structure* in which successive data items are ‘‘pushed down’’ upon preceding data items. The data items are later removed (i.e., they are ‘‘popped’’) from the stack in *reverse order*, as indicated by the last-in, first-out designation.

### Q. Why is a function used?
A function is a named block of code that performs one specific task and can be called whenever it is needed. We use functions for these reasons:

1. **Reusability:** Code is written once and can be called many times, from different places, instead of being rewritten. This reduces code size and repetition.
2. **Modularity:** A large program is divided into small, independent parts, each doing one job. This makes the program easier to design, read, and understand.
3. **Easier debugging and testing:** Each function can be tested separately, and an error can be traced to a specific function. A fix made in one place applies everywhere the function is called.
4. **Easier maintenance:** To change how a task works, you only edit the function, not every place where that task is done.