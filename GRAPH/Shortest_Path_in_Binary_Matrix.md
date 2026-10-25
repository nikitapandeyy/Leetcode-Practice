# LeetCode 1091 — Shortest Path in Binary Matrix

Pattern: BFS + Grid + 8-Directional Traversal + Level-by-Level Processing

## Approach

1. Check the starting cell: If `grid[0][0] == 1`, return `-1` because the starting cell is blocked.
2. Initialize BFS:
   - `que = deque([(0, 0)])`
   - `visited = {(0, 0)}`
   - `road = 1` because the starting cell counts as one step.
3. Process level by level:
   - `level_size = len(que)` stores the number of cells at the current level.
   - Process exactly `level_size` cells before moving to the next level.
   - Increase `road` by `1` after processing the entire level.
4. Check the destination: If `(r, c) == (row - 1, col - 1)`, return `road`.
5. Explore all 8 directions: Up, down, left, right, and the four diagonals.
6. Validate neighbors:
   - Check that the neighbor is within grid boundaries.
   - Check that it has not been visited.
   - Check that its value is `0`.
   - Add it to the queue and mark it visited.
7. If the queue becomes empty without reaching the destination, return `-1`.

## Code

```
from collections import dequeclass Solution:    def shortestPathBinaryMatrix(self, grid: list[list[int]]) -> int:        if grid[0][0] == 1:            return -1        row = len(grid)        col = len(grid[0])        directions = [            (-1, -1), (-1, 0), (-1, 1),            (0, -1),            (0, 1),            (1, -1),  (1, 0),  (1, 1)        ]        road = 1        visited = {(0, 0)}        que = deque([(0, 0)])        while que:            level_size = len(que)            for _ in range(level_size):                r, c = que.popleft()                if r == row - 1 and c == col - 1:                    return road                for dx, dy in directions:                    nr = r + dx                    nc = c + dy                    if 0 <= nr < row and 0 <= nc < col:                        if (nr, nc) not in visited:                            if grid[nr][nc] == 0:                                que.append((nr, nc))                                visited.add((nr, nc))            road += 1        return -1
```

## Key Takeaways

- Why BFS? BFS explores paths in increasing order of length, so the first time we reach the destination, we have the shortest path.
- Why `level_size`? It lets us process only the cells at the current distance before moving to the next level.
- Why `road = 1`? The starting cell itself counts as one cell in the path.
- Why 8 directions? Diagonal movement is allowed, unlike in some other grid problems.
- Why `visited`? Prevents repeatedly exploring the same cell.
- Why mark visited when adding to the queue? Prevents the same cell from being added multiple times.

## Complexity

- Time: \\(O(n^2)\\) — each cell is visited at most once, with 8 neighbors checked.
- Space: \\(O(n^2)\\) — the queue and visited set can store up to all cells.
