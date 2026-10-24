# LeetCode 994 — Rotting Oranges

**Pattern:** Multi-Source BFS + Grid + Level-by-Level Traversal

## Approach
1. Scan the grid:
   - Add all rotten oranges (`2`) to the queue.
   - Count all fresh oranges (`1`).
2. Explore the grid using BFS, starting from all rotten oranges simultaneously.
3. Process `len(queue)` oranges per level because each level represents one minute.
4. For every rotten orange, check its four adjacent cells.
5. If an adjacent cell contains a fresh orange:
   - Change it to `2`.
   - Decrease `fresh` by one.
   - Add its coordinates to the queue.
6. Increment `time` after processing each level.
7. Return `time` if no fresh oranges remain; otherwise, return `-1`.

## Python Solution

```python
from collections import deque

class Solution:
    def orangesRotting(self, grid: list[list[int]]) -> int:
        row, col = len(grid), len(grid[0])
        que = deque()
        fresh = 0
        time = 0

        for r in range(row):
            for c in range(col):
                if grid[r][c] == 2:
                    que.append((r, c))
                elif grid[r][c] == 1:
                    fresh += 1

        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

        while que and fresh > 0:
            level_size = len(que)

            for _ in range(level_size):
                r, c = que.popleft()

                for dr, dc in directions:
                    nr, nc = r + dr, c + dc

                    if 0 <= nr < row and 0 <= nc < col:
                        if grid[nr][nc] == 1:
                            grid[nr][nc] = 2
                            fresh -= 1
                            que.append((nr, nc))

            time += 1

        return time if fresh == 0 else -1
```

## Complexity

Let `m` = number of rows and `n` = number of columns.

- **Time:** `O(m × n)` — each cell is visited at most a constant number of times.
- **Space:** `O(m × n)` — the queue can store up to `O(m × n)` coordinates.

## Key Takeaways
- **Multi-source BFS:** Start with all rotten oranges, not just one.
- **Level size:** `len(queue)` tells you how many oranges to process during the current minute.
- **Fresh counter:** Count initially, then decrement whenever a fresh orange rots.
- **Four directions:** Up, down, left, and right only.
- **Unreachable oranges:** If `fresh > 0` after BFS, return `-1`.
- **No fresh oranges initially:** Return `0`.

**Remember:** Multi-source BFS + level-by-level processing is useful whenever multiple starting points spread simultaneously across a grid.
