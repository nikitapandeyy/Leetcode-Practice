# LeetCode 542 — 01 Matrix

Pattern: Multi-Source BFS + Grid + Shortest Distance

## Approach

1. Scan the matrix:
   - Add all cells containing `0` to the queue.
   - Change all original `1`s to `-1` to mark them as unvisited.
2. Start BFS from all zero cells simultaneously.
3. For every cell removed from the queue, check its four adjacent cells.
4. If an adjacent cell contains `-1`:
   - Set its distance to `mat[r][c] + 1`.
   - Add its coordinates to the queue.
5. Continue until the queue is empty.
6. Return the updated matrix.

## Python Solution

```
from collections import dequeclass Solution:    def updateMatrix(self, mat: list[list[int]]) -> list[list[int]]:        row = len(mat)        col = len(mat[0])        que = deque()        for r in range(row):            for c in range(col):                if mat[r][c] == 0:                    que.append((r, c))                else:                    mat[r][c] = -1        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]        while que:            r, c = que.popleft()            for dr, dc in directions:                nr = r + dr                nc = c + dc                if 0 <= nr < row and 0 <= nc < col:                    if mat[nr][nc] == -1:                        mat[nr][nc] = mat[r][c] + 1                        que.append((nr, nc))        return mat
```

## Complexity

Let `m` = number of rows and `n` = number of columns.

- Time: `O(m × n)` — each cell is added to the queue at most once, and each cell checks at most four neighbors.
- Space: `O(m × n)` — the queue can store up to `O(m × n)` coordinates.

## Key Takeaways

- Multi-source BFS: Start with all zero cells, not just one.
- Distance update: `mat[nr][nc] = mat[r][c] + 1`.
- Visited marker: Use `-1` for unvisited cells so it isn't confused with a valid distance of `1`.
- Four directions: Up, down, left, and right only.
- Boundary check: Validate neighbor coordinates before accessing the matrix.
- In-place update: Store distances directly in the input matrix.

Remember: Multi-source BFS is useful when you need the shortest distance from every cell to its nearest source.
