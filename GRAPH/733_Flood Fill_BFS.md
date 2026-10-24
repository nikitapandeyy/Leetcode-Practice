# 733. Flood Fill — BFS

**Problem:** [LeetCode 733 — Flood Fill](https://leetcode.com/problems/flood-fill/)  
**Difficulty:** Easy  
**Pattern:** Graph Traversal → BFS on a 2D Grid  
**Problem ID:** 733

## Approach
- Treat the 2D grid as an implicit graph; no adjacency list is needed.
- Start BFS from `(sr, sc)` using a queue.
- Explore the four directions: up, down, left, right.
- Only visit neighbors whose color matches the **original starting color**.
- Recolor a pixel **before adding it to the queue** to prevent duplicate visits.
- If the original color equals the target color, return the image immediately.

## Python Solution

```python
from collections import deque

class Solution:
    def floodFill(self, image, sr, sc, color):
        rows = len(image)
        cols = len(image[0])
        original = image[sr][sc]

        if original == color:
            return image

        queue = deque([(sr, sc)])
        image[sr][sc] = color

        directions = [
            (-1, 0),  # Up
            (1, 0),   # Down
            (0, -1),  # Left
            (0, 1)    # Right
        ]

        while queue:
            r, c = queue.popleft()

            for dr, dc in directions:
                nr = r + dr
                nc = c + dc

                if 0 <= nr < rows and 0 <= nc < cols:
                    if image[nr][nc] == original:
                        image[nr][nc] = color
                        queue.append((nr, nc))

        return image
```

## Key Learnings
- **BFS:** Use a queue (`deque`) and `popleft()`.
- **Grid neighbors:** `(r + dr, c + dc)`.
- **Boundary check:** `0 <= nr < rows` and `0 <= nc < cols`.
- **Original color:** Save it before modifying the grid.
- **Visited tracking:** Recolor when enqueueing; a separate `visited` set is unnecessary.
- **Edge case:** If `original == color`, return the unchanged image.
- **Important:** Pixel value `0` is a valid color, not a blocked cell.

## Complexity
Let `m` = rows and `n` = columns.

- **Time:** `O(m × n)` — each reachable pixel is processed at most once, with four neighbor checks.
- **Auxiliary Space:** `O(m × n)` worst case — the BFS queue may store a number of coordinates proportional to the grid size.

## Pattern Recognition
Use BFS/DFS on a grid when you need to:
- Find all connected cells with a particular property.
- Fill a connected region.
- Count or explore connected components.
- Traverse cells using horizontal and vertical neighbors.

**Remember:** Save the condition you're searching for, check boundaries, and mark cells visited before adding them to the queue.
