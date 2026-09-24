Yes — this is **correct** for Merge Intervals (LC 56). 👍

Your core logic is exactly the right pattern.

### Pattern: Sorting + Greedy

First:

```python
intervals.sort(key=lambda x: x[0])
```

This is the key step. Once intervals are sorted by start time, you only need to compare the current interval with the **last merged interval**.

Your condition:

```python
if merged[-1][-1] >= intervals[i][0]:
```

means:

> Does the current interval start before the previous merged interval ends?

If yes → they overlap → merge them.

And:

```python
if intervals[i][1] > merged[-1][1]:
    merged[-1][1] = intervals[i][1]
```

extends the merged interval when necessary.

### Small simplification

You could write:

```python
if intervals[i][1] > merged[-1][1]:
```

as:

```python
merged[-1][1] = max(merged[-1][1], intervals[i][1])
```

So your solution can be:

```python
class Solution:
    def merge(self, intervals: list[list[int]]) -> list[list[int]]:
        intervals.sort(key=lambda x: x[0])

        merged = [intervals[0]]

        for i in range(1, len(intervals)):
            if merged[-1][1] >= intervals[i][0]:
                merged[-1][1] = max(merged[-1][1], intervals[i][1])
            else:
                merged.append(intervals[i])

        return merged
```

### Complexity

* Sorting: **O(n log n)**
* Merge scan: **O(n)**
* Overall: **O(n log n)**
* Space: **O(n)** for the output.

### Key interview insight 🧠

> **Sort by start → compare with last merged interval → extend its end if overlapping → otherwise start a new interval.**

This is a good example of a **greedy pattern**: after sorting, the locally merged interval contains everything you need to make the next decision.
