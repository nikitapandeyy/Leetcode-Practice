# LeetCode 695 — Max Area of Island

## 1. Pattern
**Grid + DFS + Connected Components**

Find the largest connected group of land cells (`1`) in a grid. Land connects horizontally and vertically, not diagonally.

## 2. Approach

1. Calculate the number of rows and columns.
2. Initialize `max_area = 0`.
3. Traverse every cell using nested loops.
4. If `grid[r][c] == 1`, a new, unvisited island has been found.
5. Call `dfs(r, c)` to calculate the island's area.
6. Inside DFS:
   - Mark the current cell as visited by changing `1` to `0`.
   - Initialize `area = 1` for the current cell.
   - Explore the four neighboring cells.
   - For each valid, unvisited land neighbor, add its DFS result to `area`.
   - Return the total area.
7. Update `max_area` with the largest area found.
8. Return `max_area`. If there are no islands, it remains `0`.

## 3. Code

```python
class Solution:
    def maxAreaOfIsland(self, grid: list[list[int]]) -> int:
        row = len(grid)
        col = len(grid[0])
        max_area = 0

        directions = [
            (-1, 0),
            (0, -1),
            (1, 0),
            (0, 1)
        ]

        def dfs(r, c):
            grid[r][c] = 0
            area = 1

            for dx, dy in directions:
                nr = r + dx
                nc = c + dy

                if 0 <= nr < row and 0 <= nc < col:
                    if grid[nr][nc] == 1:
                        area += dfs(nr, nc)

            return area

        for r in range(row):
            for c in range(col):
                if grid[r][c] == 1:
                    max_area = max(max_area, dfs(r, c))

        return max_area
```

## 4. Key Takeaways

- **`area = 1`:** Count the current land cell.
- **`grid[r][c] = 0`:** Mark the cell visited to prevent revisiting it.
- **`area += dfs(nr, nc)`:** Add the area of the connected neighboring land.
- **`return area`:** DFS returns the total size of one island.
- **`max_area = max(max_area, dfs(r, c))`:** Keep the largest island area found.
- **No separate visited set is needed:** Changing land to water tracks visited cells in place.
- **Integer values matter:** This problem uses `0` and `1`, not `"0"` and `"1"`.

### Common mistakes

1. Forgetting to initialize `area = 1`.
2. Calling DFS without adding its returned area.
3. Using inconsistent variable names, such as `ny` and `nc`.
4. Forgetting to mark a cell visited, causing repeated visits or infinite recursion.
5. Confusing the number of islands with the area of an island.

## 5. Complexity

Let `m` be the number of rows and `n` be the number of columns.

- **Time:** `O(m × n)` — each cell is visited at most once.
- **Space:** `O(m × n)` worst case — the recursive DFS stack can grow to the size of the grid.

## 6. Difference: Number of Islands vs. Max Area of Island

| Number of Islands — LC 200 | Max Area of Island — LC 695 |
|---|---|
| Count connected islands | Find the largest island |
| DFS explores an island | DFS calculates an island's area |
| Increment `count` by `1` | Add recursive results to `area` |
| Return `count` | Return `max_area` |

## 7. When to Use This Pattern

Use DFS when a grid problem asks you to:
- Find connected regions.
- Calculate the size of a region.
- Find the largest or smallest connected group.
- Traverse land cells without visiting any cell twice.

**Remember:** Find land → mark visited → recursively calculate connected area → update the maximum.
