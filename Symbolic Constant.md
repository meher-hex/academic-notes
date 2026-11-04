# Symbolic Constant

A **symbolic constant** is a name given to a fixed value, instead of using the raw value (a "magic number") directly in code. This is exactly what `#define` is used for.

```c
#define PI 3.14159
#define MAX_STUDENTS 100
```

## Why Use Symbolic Constants Instead of Raw Values?

```c
// Without symbolic constant — unclear what 3.14159 and 100 mean at a glance
float area = 3.14159 * r * r;
int scores[100];

// With symbolic constant — self-documenting, and easy to change everywhere at once
#define PI 3.14159
#define MAX_STUDENTS 100

float area = PI * r * r;
int scores[MAX_STUDENTS];
```

**Key benefits:**
- **Readability** — `MAX_STUDENTS` explains itself; `100` alone doesn't say what it represents.
- **Easy maintenance** — change the value once at the `#define` line, and it updates everywhere it's used in the file, instead of hunting down every occurrence of `100`.
- **Fewer typos/bugs** — reduces the risk of typing `3.14159` correctly in one place but `3.1415` in another.

## How It Works

As covered in Preprocessor Directives, `#define` performs a plain text substitution **before compilation**, done by the preprocessor. The symbolic constant itself isn't a variable — it has no memory address and can't be changed while the program runs.

```c
#define PI 3.14159

int main() {
    // PI = 3.14;   // ❌ Error — PI isn't a real variable, this doesn't compile
    float area = PI * 5 * 5;   // ✅ works fine
}
```

## Naming Convention

Symbolic constants are conventionally written in **UPPERCASE** to visually distinguish them from regular variables at a glance.

```c
#define MAX_LIMIT 100     // ✅ conventional
#define maxLimit 100      // works, but not the standard convention
```
