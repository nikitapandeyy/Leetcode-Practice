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


################################################################################################################################################





# LeetCode 733 — Flood Fill

**Pattern:** DFS + Grid + Recursion + Four-Directional Traversal

## Approach

1. **Get the grid dimensions**
   - `row = len(image)`
   - `col = len(image[0])`

2. **Store the original color**
   - `orig = image[sr][sc]`
   - We must identify the original color because only connected pixels with this color should change.

3. **Handle the base case**
   - If `orig == color`, return `image`.
   - If the original color is already the target color, no changes are needed.

4. **Define the four directions**
   - Up: `(-1, 0)`
   - Down: `(1, 0)`
   - Left: `(0, -1)`
   - Right: `(0, 1)`
   - Only horizontal and vertical neighbors count; diagonal neighbors are not included.

5. **Create a DFS function**
   - Check whether the current row and column are within the image boundaries.
   - Check whether the pixel has already been visited.
   - Check whether its color matches `orig`.
   - If all conditions are satisfied:
     - Change its color to `color`.
     - Mark it as visited.
     - Recursively explore its four neighbors.

6. **Start DFS**
   - Call `dfs(sr, sc)` to begin from the starting pixel.

7. **Return the modified image**
   - `return image`

## Code

```python
class Solution:
    def floodFill(self, image: list[list[int]], sr: int, sc: int, color: int) -> list[list[int]]:
        row = len(image)
        col = len(image[0])

        orig = image[sr][sc]

        if orig == color:
            return image

        directions = [
            (-1, 0),
            (0, -1),
            (1, 0),
            (0, 1)
        ]

        visited = set()

        def dfs(r, c):
            if 0 <= r < row and 0 <= c < col:
                if (r, c) not in visited:
                    if image[r][c] == orig:
                        image[r][c] = color
                        visited.add((r, c))

                        for dx, dy in directions:
                            nr = r + dx
                            nc = c + dy
                            dfs(nr, nc)

        dfs(sr, sc)
        return image
```

## Key Takeaways

- **Why DFS?** DFS explores all connected pixels that share the original color.
- **Why store `orig`?** It lets us distinguish pixels that should change from pixels that should remain unchanged.
- **Why check boundaries?** To prevent accessing coordinates outside the image.
- **Why use `visited`?** To prevent repeatedly processing the same pixel.
- **Why check `orig == color`?** Otherwise, recoloring could cause unnecessary or repeated exploration.
- **Why four directions?** Flood Fill considers only horizontal and vertical neighbors, not diagonals.
- **Why modify the image directly?** We can update each pixel in place instead of creating another grid.

## Complexity

Let `m` be the number of rows and `n` be the number of columns.

- **Time Complexity:** `O(m × n)` — in the worst case, every pixel is visited once, and each pixel checks four neighbors.
- **Space Complexity:** `O(m × n)` — the `visited` set and recursive DFS call stack can each require up to `O(m × n)` space.

## Optimization

A separate `visited` set is not necessary for this problem.

Because each matching pixel is changed from `orig` to `color`, the modified color itself can prevent that pixel from being processed again. This works because we return early when `orig == color`.

Removing `visited` simplifies the code, but the worst-case time and recursive stack space complexities remain `O(m × n)`.
