# Practice Programs — Geometry

## 1. Grey Area of a Square with an Inscribed Circle

**Problem:** A square has side length `a`. A circle is inscribed inside it, meaning the circle's diameter equals `a`. Find the leftover ("grey") area — the square's area minus the circle's area.

**Logic:**
- Square area = `a × a`
- Circle's diameter = `a`, so radius = `a / 2`
- Circle area = `π × r²`
- Grey area = Square area − Circle area

```c
#include <stdio.h>
#define PI 3.14159

int main() {
    float a, radius, squareArea, circleArea, greyArea;

    printf("Enter the side of the square: ");
    scanf("%f", &a);

    radius = a / 2;
    squareArea = a * a;
    circleArea = PI * radius * radius;
    greyArea = squareArea - circleArea;

    printf("Grey area = %.2f\n", greyArea);

    return 0;
}
```

---

## 2. Area of a Triangle Given All Three Sides (Heron's Formula)

**Problem:** Given the lengths of all three sides of a triangle, find its area.

**Logic (Heron's Formula):**
```
s = (side1 + side2 + side3) / 2         // semi-perimeter
Area = √(s × (s - side1) × (s - side2) × (s - side3))
```

```c
#include <stdio.h>
#include <math.h>   // needed for sqrt()

int main() {
    float side1, side2, side3, s, area;

    printf("Enter three sides of the triangle: ");
    scanf("%f %f %f", &side1, &side2, &side3);

    s = (side1 + side2 + side3) / 2;
    area = sqrt(s * (s - side1) * (s - side2) * (s - side3));

    printf("Area of triangle = %.2f\n", area);

    return 0;
}
```

**Note:** This formula only gives a valid (real) result if the three sides can actually form a triangle — otherwise, the value inside the square root becomes negative, and `sqrt()` returns `nan`.

---

## 3. Angle Difference Between Hour and Minute Hand at a Given Time

**Problem:** Given a time (hours and minutes), find the angle between the hour hand and minute hand of an analog clock.

**Logic:**
- A full clock face is `360°`, divided into 12 hours → each hour mark = `30°`.
- **Minute hand angle** (from 12 o'clock position): moves `6°` per minute (`360° / 60 minutes`).
  ```
  minuteAngle = minutes × 6
  ```
- **Hour hand angle:** moves `30°` per hour, PLUS a small extra amount as minutes pass (since the hour hand creeps forward between hour marks too) — it moves `0.5°` per minute (`30° / 60 minutes`).
  ```
  hourAngle = (hours % 12) × 30 + minutes × 0.5
  ```
- **Difference:** subtract the two, take the absolute value, and if the result is more than 180°, subtract it from 360° (since the angle "the short way around" is what's usually wanted).

```c
#include <stdio.h>
#include <math.h>   // needed for fabs()

int main() {
    int hours, minutes;
    float hourAngle, minuteAngle, angle;

    printf("Enter time (hours minutes): ");
    scanf("%d %d", &hours, &minutes);

    hourAngle = (hours % 12) * 30 + minutes * 0.5;
    minuteAngle = minutes * 6;

    angle = fabs(hourAngle - minuteAngle);

    if (angle > 180) {
        angle = 360 - angle;
    }

    printf("Angle between hour and minute hand = %.2f degrees\n", angle);

    return 0;
}
```

**Example check:** At `3:00`, hour hand is at `90°` (3 × 30), minute hand is at `0°` → difference = `90°`, which matches what you'd expect looking at a clock.
