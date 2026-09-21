

**Pattern:** Modified Binary Search

### Key Idea ⭐

A rotated sorted array always has **at least one sorted half**.

Example:

```text
[4, 5, 6, 7, 0, 1, 2]
 L     M        R
```

Left half is sorted:

```text
4 → 5 → 6 → 7
```

Find which half is sorted, then check whether the `target` lies inside it.

---

### Logic

```text
nums[mid] == target
        ↓
      found
```

Otherwise:

**1. Left half is sorted**

```python
if nums[left] <= nums[mid]:
```

Check:

```python
if nums[left] <= target < nums[mid]:
    right = mid - 1
else:
    left = mid + 1
```

**2. Right half is sorted**

```python
else:
```

Check:

```python
if nums[mid] < target <= nums[right]:
    left = mid + 1
else:
    right = mid - 1
```

---

### Code

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:

        left = 0
        right = len(nums) - 1

        while left <= right:

            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid

            # Left half is sorted
            if nums[left] <= nums[mid]:

                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # Right half is sorted
            else:

                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

### Mental Model

```text
1. Find mid
2. Is left half sorted?
      ↓
   YES → Is target inside it?
              ↓
          YES: go left
          NO:  go right

   NO → Right half must be sorted
              ↓
        Is target inside it?
              ↓
          YES: go right
          NO:  go left
```

### Complexity

**Time:** `O(log n)`  
**Space:** `O(1)`

### One-line takeaway

> **Rotated Binary Search = identify the sorted half → check if target belongs there → discard the other half.**