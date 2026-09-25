## Longest Consecutive Sequence — LC 128 📝

**Pattern:** Hash Set + Sequence Start Detection

### 🎯 Goal

Find the length of the **longest sequence of consecutive integers**, regardless of their order.

Example:

```text
[100, 4, 200, 1, 3, 2]
```

Longest sequence:

```text
1 → 2 → 3 → 4
```

Answer = **4**

---

### 🧠 Core Idea

Put all numbers into a **set** so checking whether a number exists is **O(1)** on average.

The important trick is:

> **Only start building a sequence if `num - 1` does NOT exist.**

Why?

If:

```text
1, 2, 3, 4
```

When you reach `2`, `1` already exists, so `2` is **not the beginning**.

When you reach `1`:

```text
0 does not exist
```

So `1` is the **start of a sequence**.

Then check:

```text
2 → exists?
3 → exists?
4 → exists?
5 → doesn't exist
```

Sequence length = 4.

---

### Why this is important

Without the "sequence start" check, you might repeatedly scan the same sequence:

```text
1 → 2 → 3 → 4
2 → 3 → 4
3 → 4
4
```

That can become inefficient.

With:

> **`num - 1` not present → start sequence**

we only expand from the beginning.

---

### Your approach

You created:

```text
setn = set(nums)
```

That's the correct data structure.

Then:

```text
num - 1 not present
        ↓
   sequence starts
        ↓
keep checking num + 1
        ↓
   count length
        ↓
update maximum
```

### Complexity

**Time:** O(n) average

**Space:** O(n)

The important interview point is that although there's a `while` inside the `for`, we're **not repeatedly scanning every sequence**, because we only start expanding from sequence beginnings.

### ⭐ One-line takeaway

> **Hash Set + only expand when `num - 1` doesn't exist = O(n) Longest Consecutive Sequence.**
class Solution:
    def longestConsecutive(self, nums: list[int]) -> int:
        lon=1
        setn=set(nums)
        for num in nums:
            
            if num-1 not in nums:
                leng=1
                while num+1 in nums:
                    leng+=1
                    num=num+1
                lon=max(lon,leng)
        return lon
            
         