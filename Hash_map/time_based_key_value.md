# Time Based Key-Value Store — LC 981 📝

**Pattern:** HashMap + Binary Search
**Main concept:** Find the **rightmost timestamp ≤ target timestamp**

---

## 1. Problem

We need to support two operations:

### `set(key, value, timestamp)`

Store a value for a key at a particular timestamp.

Example:

```text
set("foo", "bar", 1)
set("foo", "bar2", 4)
set("foo", "bar3", 7)
```

Internally:

```text
foo → [
    ["bar", 1],
    ["bar2", 4],
    ["bar3", 7]
]
```

### `get(key, timestamp)`

Return the value associated with the **largest timestamp that is ≤ the requested timestamp**.

Example:

```text
get("foo", 5)
```

Available timestamps:

```text
1    4    7
     ↑
   target = 5
```

Timestamp `4` is the largest timestamp ≤ `5`.

Answer:

```text
"bar2"
```

---

# 2. Data Structure

Use a HashMap:

```text
dict
 ↓
key → list of [value, timestamp]
```

Example:

```text
foo → [["bar",1], ["bar2",4], ["bar3",7]]

cat → [["meow",2], ["meow2",6]]
```

Why?

We first need to find the correct **key**, then search its timestamps.

---

# 3. Why Binary Search?

The problem guarantees that timestamps for a key are given in **strictly increasing order**.

So:

```text
foo → 1, 4, 7, 10, 15
```

is already sorted.

Instead of checking every timestamp:

```text
1 → 4 → 7 → 10 → 15
```

we can use binary search.

That gives:

```text
O(log n)
```

instead of:

```text
O(n)
```

---

# 4. The important binary-search pattern ⭐

This is **not ordinary exact-match binary search**.

We're looking for:

> **The rightmost timestamp that is ≤ target.**

Suppose:

```text
timestamps = [1, 4, 7, 10]
target = 8
```

We want:

```text
7
```

because:

```text
1 ≤ 8 ✓
4 ≤ 8 ✓
7 ≤ 8 ✓
10 ≤ 8 ✗
```

So `7` is the **rightmost valid timestamp**.

---

# 5. How your binary search works

```python
res = ""

while l <= r:
    m = (l + r) // 2

    if values[m][1] <= timestamp:
        res = values[m][0]
        l = m + 1
    else:
        r = m - 1
```

### Case 1: timestamp is valid

```python
values[m][1] <= timestamp
```

We found a possible answer.

Save it:

```python
res = values[m][0]
```

But don't stop!

There might be a **later timestamp that is also valid**.

So:

```python
l = m + 1
```

Search to the right.

---

### Case 2: timestamp is too large

```python
values[m][1] > timestamp
```

This timestamp cannot be the answer.

Everything to its right is also too large because timestamps are sorted.

So:

```python
r = m - 1
```

Search left.

---

# 6. Example Dry Run

Suppose:

```text
foo → [["bar",1], ["bar2",4], ["bar3",7], ["bar4",10]]
```

Call:

```text
get("foo", 8)
```

We want timestamp `≤ 8`.

Initially:

```text
l = 0
r = 3
```

### First

```text
m = 1
timestamp = 4
```

`4 ≤ 8` ✅

Save:

```text
res = "bar2"
```

Search right:

```text
l = 2
```

### Second

```text
m = 2
timestamp = 7
```

`7 ≤ 8` ✅

Update:

```text
res = "bar3"
```

Search right:

```text
l = 3
```

### Third

```text
m = 3
timestamp = 10
```

`10 > 8` ❌

Search left:

```text
r = 2
```

Now:

```text
l > r
```

Stop.

Return:

```text
"bar3"
```

---

# 7. Complete Code

```python
class TimeMap:

    def __init__(self):
        self.dict = {}

    def set(self, key: str, value: str, timestamp: int) -> None:

        if key not in self.dict:
            self.dict[key] = []

        self.dict[key].append([value, timestamp])

    def get(self, key: str, timestamp: int) -> str:

        res = ""

        values = self.dict.get(key, [])

        left = 0
        right = len(values) - 1

        while left <= right:

            mid = (left + right) // 2

            if values[mid][1] <= timestamp:
                res = values[mid][0]
                left = mid + 1

            else:
                right = mid - 1

        return res
```

---

# 8. Why `res = ""`?

There might be **no timestamp ≤ target**.

Example:

```text
foo → [["bar", 5]]
```

Request:

```text
get("foo", 3)
```

There is no valid timestamp.

So return:

```text
""
```

That's why we initialize:

```python
res = ""
```

---

# 9. Why `dict.get(key, [])`?

```python
values = self.dict.get(key, [])
```

If the key exists:

```text
foo → [...]
```

we get its list.

If the key doesn't exist:

```text
baz → ?
```

we get:

```text
[]
```

instead of causing a KeyError.

---

# 10. Complexity

For `set()`:

```text
Time:  O(1)
```

because we're appending to the list.

For `get()`:

```text
Time: O(log n)
```

because of binary search over timestamps for that key.

Space:

```text
O(total number of set operations)
```

because we store every key-value-timestamp entry.

---

# 🧠 Pattern Recognition

When you see:

> **Key + values associated with timestamps + retrieve the latest value at or before a timestamp**

Think:

```text
HashMap
   ↓
key → sorted timestamp list
   ↓
Binary Search
   ↓
rightmost timestamp ≤ target
```

### ⭐ Most important takeaway

> **This is a "rightmost valid element" binary search.**

Condition:

```text
timestamp <= target
```

If valid → **save answer + move right**.

If invalid → **move left**.

That's the binary-search pattern you should remember from this problem.
