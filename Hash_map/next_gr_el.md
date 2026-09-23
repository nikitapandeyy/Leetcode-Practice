## Next Greater Element I — LC 496

**Pattern:** Monotonic Stack + HashMap

### 🎯 Goal

For every element in `nums1`, find the **first greater element to its right** in `nums2`.

### 💡 Core Idea

Use a stack to keep elements that are **waiting for their next greater element**.

When a new `num` arrives:

```python
while st and st[-1] < num:
    value = st.pop()
    ng[value] = num
```

Why?

> If `num > st[-1]`, then `num` is the **next greater element** of `st[-1]`.

After resolving all possible elements:

```python
st.append(num)
```

The current number now waits for its own greater element.

### 🔑 Stack Property

The stack is **monotonically decreasing**:

```text
[5, 3, 1]
```

If `4` arrives:

```text
1 → 4   pop 1
3 → 4   pop 3

stack = [5, 4]
```

### 🧠 Pattern Recognition

Whenever you see:

* Next greater element
* Next smaller element
* Previous greater element
* Previous smaller element
* Nearest greater/smaller element

👉 **Think Monotonic Stack.**

### Why HashMap?

We build:

```text
element → next greater element
```

Example:

```text
1 → 3
3 → 4
4 → -1
2 → -1
```

Then:

```python
return [ng[num] for num in nums1]
```

### Complexity

* **Time:** O(n)
* **Space:** O(n)

Even though there's a `while` inside the `for`, it's **O(n)** because every element is pushed once and popped at most once.

### ⭐ One-line takeaway

> **Monotonic Stack = keep elements waiting for an answer; when the current element can answer them, pop and record the relationship.**

class Solution:
    def nextGreaterElement(self, nums1: list[int], nums2: list[int]) -> list[int]:
        ng=defaultdict(lambda:-1)
        
        st=[]
        for num in nums2:
            while st and st[-1]<num:
                value=st.pop()
                
                ng[value]=num
            st.append(num)
        return [ng[num] for num in nums1]
        
