

**Pattern:** Sliding Window + HashSet

**Goal:** Find the longest substring with **no duplicate characters**.

### Core Idea

Maintain a window `[left ... right]` containing only unique characters.

- `right` → expand window
    
- `left` → shrink window
    
- `seen` → stores characters inside current window
    
- If duplicate appears → remove from left until valid
    

### Key Logic

```text
if s[right] is already in seen:
    remove s[left]
    move left
    repeat until duplicate is gone

add s[right]
update maximum length
```

### Window Length

```python
right - left + 1
```

### Why `while`?

A duplicate may require removing **multiple characters**, so keep shrinking until the window is valid.

### Complexity

- **Time:** O(n)
    
- **Space:** O(min(n, charset))
    

### One-line takeaway

> **Expand right; when duplicate makes the window invalid, shrink left until all characters are unique.**