## Top K Frequent Elements — LC 347 📝

**Pattern:** HashMap + Bucket Sort

### Idea

1. Count frequency of every number using a HashMap.
2. Create buckets where **index = frequency**.
3. Put each number into its frequency bucket.
4. Traverse buckets **from highest frequency → lowest**.
5. Collect numbers until we have `k`.

```python
freq = {}
for num in nums:
    freq[num] = freq.get(num, 0) + 1

buckets = [[] for _ in range(len(nums) + 1)]

for num, count in freq.items():
    buckets[count].append(num)
```

### Key insight

> **Bucket index represents frequency.**

Example:

```text
freq:  1 → 3
       2 → 2
       3 → 4

bucket:
2 → [2]
3 → [1]
4 → [3]
```

Start from the highest bucket because we want the **most frequent** elements.

### Complexity

* **Time:** O(n)
* **Space:** O(n)

### Why `n + 1` buckets?

Maximum possible frequency of one number is `n`.

So we need indices:

```text
0 ... n
```

### One-line takeaway

> **Count → bucket by frequency → scan buckets backward → take k elements.**
Yes — **Bucket Sort + HashMap is an optimal approach for this problem** in the usual interview sense.

### Complexity

For `n = len(nums)`:

* Build frequency map → **O(n)**
* Build buckets → **O(n)**
* Scan buckets → **O(n)** worst case

So:

**Time: O(n)**
**Space: O(n)**

That's better asymptotically than:

* Sorting by frequency → **O(n log n)**
* Min-heap of size `k` → **O(n log k)**

### One important correction

Your final collection should be:

```python
for i in range(len(nums), 0, -1):
    for num in buck[i]:
        result.append(num)
        if len(result) == k:
            return result
```

So the complete optimal version is:

```python
class Solution:
    def topKFrequent(self, nums: list[int], k: int) -> list[int]:
        hash_map = {}

        for num in nums:
            hash_map[num] = 1 + hash_map.get(num, 0)

        buck = [[] for _ in range(len(nums) + 1)]

        for num, freq in hash_map.items():
            buck[freq].append(num)

        result = []

        for i in range(len(nums), 0, -1):
            for num in buck[i]:
                result.append(num)
                if len(result) == k:
                    return result
```

**Interview takeaway:**

> When you need the top K elements by frequency and frequencies are bounded by `n`, **HashMap + Bucket Sort gives O(n)**.
