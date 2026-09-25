# Trapping Rain Water — LC 42 📝

**Pattern:** Two Pointers + Running Maximums
**Goal:** Calculate how much water can be trapped between bars.

---

## 1. Understand the problem

Example:

```text
height = [4, 2, 0, 3, 2, 5]
```

Diagram:

```text
5                         █
4   █                     █
3   █           █         █
2   █   █       █   █     █
1   █   █   █   █   █     █
    -----------------------
    4   2   0   3   2   5
```

Water can sit in the valleys because there is a taller boundary on both sides.

For example, at height `2`:

```text
left boundary = 4
right boundary = 5

water = min(4, 5) - 2
      = 2
```

### Fundamental formula

For every position:

```text
water[i] = min(max_left, max_right) - height[i]
```

The difficulty is finding `max_left` and `max_right` efficiently.

---

# 2. Two-pointer insight ⭐

Instead of creating two arrays for left and right maximums, use:

```text
left →                         ← right
```

and maintain:

```text
leftmax
rightmax
```

Diagram:

```text
        left                    right
         ↓                       ↓
[ 4,  2,  0,  3,  2,  5 ]
  ↑                           ↑
leftmax                    rightmax
```

At every step, compare:

```python
height[left] <= height[right]
```

### Why?

The **shorter side determines the maximum water level**.

If:

```text
height[left] < height[right]
```

we know there is already a right boundary at least as tall as the left boundary.

Therefore, we can safely calculate the water on the **left side**.

Similarly, if the right side is shorter, process the right.

---

# 3. What does `leftmax` mean?

This is extremely important.

```text
left     → INDEX
leftmax  → HEIGHT
```

For example:

```text
height = [4, 2, 0, 3, 2, 5]
```

When `left` reaches the first bar:

```text
height[left] = 4
```

we set:

```text
leftmax = 4
```

We do **NOT** do:

```python
leftmax = left
```

because `left` is just an index.

Same idea:

```text
right     → index
rightmax  → height
```

---

# 4. When do we add water?

Suppose:

```text
leftmax = 4
height[left] = 2
```

Then:

```text
water += 4 - 2
```

because there are `2` units of empty space above that bar.

But if:

```text
height[left] > leftmax
```

then this is a new boundary:

```text
leftmax = height[left]
```

There is **no water at this position**.

---

# 5. Code

```python
class Solution:
    def trap(self, height: list[int]) -> int:

        left = 0
        right = len(height) - 1

        leftmax = 0
        rightmax = 0
        water = 0

        while left < right:

            if height[left] <= height[right]:

                if height[left] > leftmax:
                    leftmax = height[left]
                else:
                    water += leftmax - height[left]

                left += 1

            else:

                if height[right] > rightmax:
                    rightmax = height[right]
                else:
                    water += rightmax - height[right]

                right -= 1

        return water
```

---

# 6. Dry run

For:

```text
[4, 2, 0, 3, 2, 5]
```

Initially:

```text
left = 0
right = 5
leftmax = 0
rightmax = 0
water = 0
```

### Step 1

```text
height[left] = 4
height[right] = 5
```

Left is smaller.

```text
4 > leftmax
```

So:

```text
leftmax = 4
```

No water.

Move:

```text
left → 1
```

---

### Step 2

```text
height[left] = 2
height[right] = 5
```

Left is smaller.

```text
2 < leftmax(4)
```

So:

```text
water += 4 - 2
       += 2
```

---

### Step 3

```text
height[left] = 0
```

Still left side is smaller.

```text
water += 4 - 0
       += 4
```

Total:

```text
water = 6
```

And so on.

Final answer:

```text
9
```

---

# 7. Why is the algorithm O(n)?

Although there is a `while` loop, we're not repeatedly scanning the array.

Every iteration moves either:

```text
left += 1
```

or:

```text
right -= 1
```

Each pointer moves at most `n` times.

Therefore:

```text
Time  = O(n)
Space = O(1)
```

---

# 🧠 Pattern recognition

When you see:

> Calculate water trapped between bars.

Think:

```text
Two boundaries
      ↓
left + right pointers
      ↓
leftmax + rightmax
      ↓
process the shorter side
      ↓
calculate trapped water
```

### ⭐ One-line interview note

> **Trapping Rain Water = two pointers; process the shorter boundary because it determines the water level, while maintaining the maximum height seen from each side.**

### Most important distinction

```text
left/right       → positions (indices)
leftmax/rightmax → heights (values)
```

That was the main issue in your implementation, and once you keep those two concepts separate, the code becomes much easier to reason about.
