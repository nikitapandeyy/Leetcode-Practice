# LeetCode 200 — Number of Islands

## 1. Problem

Given a 2D grid containing:

* `"1"` → land
* `"0"` → water

An island is a group of connected land cells.

Two land cells are connected **only horizontally or vertically**.

Diagonal connection does **not** count.

### Goal

Return the total number of islands.

---

# 2. The Most Important Idea

The key is to stop thinking of the grid as just a matrix.

Think of it as a **graph**.

### Grid

```text
1 1 0
1 0 0
0 0 1
```

Each land cell is a **node**.

Two nodes have an **edge** if their cells are directly adjacent:

```text
up
down
left
right
```

So:

```text
(0,0) ---- (0,1)
  |
(1,0)
```

These three cells form one connected component → **one island**.

The isolated `(2,2)` is another connected component → **another island**.

Therefore:

```text
Number of islands = Number of connected components of land
```

This is the fundamental graph concept behind the problem.

---

# 3. What Is a Connected Component?

A connected component is a group of nodes where you can travel from one node to another through edges.

For example:

```text
0 --- 1 --- 2

3 --- 4

5
```

There are three connected components:

```text
{0,1,2}
{3,4}
{5}
```

Therefore there are **3 connected components**.

For Number of Islands:

> Every connected component of `"1"` cells is one island.

---

# 4. How Do We Find Connected Components?

We use a graph traversal:

* DFS — Depth First Search
* BFS — Breadth First Search

For this problem, we learned **iterative DFS using a stack**.

The basic idea:

```text
Find unvisited land
        ↓
Start DFS
        ↓
Explore every connected land cell
        ↓
That entire region = ONE island
        ↓
Continue scanning the grid
        ↓
Find another unvisited land?
        ↓
Start another DFS
        ↓
island += 1
```

---

# 5. Why Do We Need `visited`?

Suppose we have:

```text
1 1
1 1
```

Starting from `(0,0)`, we can reach:

```text
(0,1)
(1,0)
(1,1)
```

But while moving around the island, we can encounter cells we've already seen.

Without remembering visited cells, DFS could repeatedly process the same cells.

So we maintain:

```python
visited = set()
```

For example:

```python
visited = {
    (0,0),
    (0,1),
    (1,0)
}
```

### Mental model

`visited` answers:

> "Have I already explored/discovered this cell?"

---

# 6. Why Do We Need a Stack?

DFS means:

> Go as deep as possible before coming back.

With iterative DFS, we use a **stack**.

```python
stack = [(r, c)]
```

The stack stores cells that still need to be explored.

### Mental model

`visited`:

> Where have I already been?

`stack`:

> Where do I need to go next?

These are two different jobs.

---

# 7. The DFS Process

Suppose:

```text
1 1 0
1 0 0
0 0 1
```

Start at:

```text
(0,0)
```

We put it into the stack:

```text
stack = [(0,0)]
```

and mark:

```text
visited = {(0,0)}
```

Then:

```python
cr, cc = stack.pop()
```

We explore its neighbors.

For `(0,0)`:

```text
up    → (-1,0)
down  → (1,0)
left  → (0,-1)
right → (0,1)
```

Only valid land cells should be added.

So:

```text
(1,0) → land
(0,1) → land
```

Add them to the stack and visited set.

Continue until:

```text
stack == []
```

At this point:

> We have completely explored that island.

---

# 8. The Four Directions

We represent movement using:

```python
directions = [
    (-1, 0),   # up
    (1, 0),    # down
    (0, -1),   # left
    (0, 1)     # right
]
```

For current cell:

```python
cr, cc
```

neighbor becomes:

```python
nr = cr + dr
nc = cc + dc
```

For example, if:

```text
current = (2,3)
```

Then:

```text
up    → (1,3)
down  → (3,3)
left  → (2,2)
right → (2,4)
```

---

# 9. The Three Conditions for a Valid Neighbor

This is one of the most important things to remember.

When exploring a neighbor, it must satisfy:

### Condition 1 — Inside the grid

```python
0 <= nr < rows
0 <= nc < cols
```

We don't want:

```text
(-1, 2)
(5, 3)
```

if those coordinates don't exist.

---

### Condition 2 — It must be land

```python
grid[nr][nc] == "1"
```

Water is not part of the island.

---

### Condition 3 — It must not already be visited

```python
(nr, nc) not in visited
```

Otherwise we may process the same cell repeatedly.

So the complete idea is:

```python
if inside_grid and land and not_visited:
```

Then:

```python
visited.add((nr, nc))
stack.append((nr, nc))
```

---

# 10. When Do We Increment `island`?

This is perhaps the most important part of the entire problem.

We scan every cell:

```python
for r in range(rows):
    for c in range(cols):
```

Suppose we encounter:

```text
grid[r][c] == "1"
```

and:

```text
(r,c) not in visited
```

That means:

> We have discovered a completely new connected component.

Therefore:

```python
island += 1
```

Then we start DFS.

### Important

We **do NOT** increment the count every time we discover a land cell.

Example:

```text
1 1 1
```

There are 3 land cells but only **1 island**.

DFS discovers all three.

So:

```text
new island found → +1
cells inside that island → +0
```

---

# 11. The Complete Mental Model

Memorize this **logic**, not the code:

```text
Scan every cell
      ↓
Is it unvisited land?
      ↓
YES
      ↓
New island!
      ↓
island += 1
      ↓
Start DFS
      ↓
Explore all connected land
      ↓
Mark cells visited
      ↓
DFS finishes
      ↓
Continue scanning
      ↓
Another unvisited land?
      ↓
Another island
```

---

# 12. Our Example

```text
1 1 0 0 0
1 0 0 1 1
0 0 0 0 1
0 1 1 0 0
```

### Island 1

Starting from:

```text
(0,0)
```

DFS reaches:

```text
(0,0)
(0,1)
(1,0)
```

So:

```text
Island 1 = {(0,0), (0,1), (1,0)}
```

---

### Island 2

Eventually scanning reaches:

```text
(1,3)
```

DFS reaches:

```text
(1,3)
(1,4)
(2,4)
```

So:

```text
Island 2 = {(1,3), (1,4), (2,4)}
```

---

### Island 3

Eventually:

```text
(3,1)
```

DFS reaches:

```text
(3,1)
(3,2)
```

So:

```text
Island 3 = {(3,1), (3,2)}
```

Therefore:

```text
answer = 3
```

---

# 13. Why `(1,3)` Is Not Connected to `(0,0)`

This is a graph question.

For two cells to belong to the same island, there must be a path consisting entirely of:

```text
land → land → land → ...
```

using only:

```text
up/down/left/right
```

There is no such path between the two regions.

Water cells break the edges.

Therefore:

```text
(0,0) region ≠ (1,3) region
```

and they are separate connected components.

---

# 14. Why Diagonal Doesn't Count

Consider:

```text
1 0
0 1
```

The two land cells touch diagonally.

But diagonal movement is not allowed.

Therefore:

```text
answer = 2
```

NOT:

```text
answer = 1
```

This is determined entirely by the problem's definition of connectivity.

---

# 15. Iterative DFS Template

Our learned implementation is:

```python
class Solution:
    def numIslands(self, grid):
        rows = len(grid)
        cols = len(grid[0])

        visited = set()
        island = 0

        for r in range(rows):
            for c in range(cols):

                if grid[r][c] == "1" and (r, c) not in visited:

                    island += 1

                    stack = [(r, c)]
                    visited.add((r, c))

                    while stack:

                        cr, cc = stack.pop()

                        directions = [
                            (-1, 0),
                            (1, 0),
                            (0, -1),
                            (0, 1)
                        ]

                        for dr, dc in directions:

                            nr = cr + dr
                            nc = cc + dc

                            if (
                                0 <= nr < rows
                                and 0 <= nc < cols
                                and grid[nr][nc] == "1"
                                and (nr, nc) not in visited
                            ):
                                visited.add((nr, nc))
                                stack.append((nr, nc))

        return island
```

---

# 16. Why `visited.add()` Happens Before `stack.append()`

When we discover a valid neighbor:

```python
visited.add((nr, nc))
stack.append((nr, nc))
```

We mark it visited **immediately when we discover it**.

Why?

Imagine two different cells both discover the same neighbor before that neighbor gets popped.

If we only mark it visited when popping, the same cell could be added to the stack multiple times.

Marking it immediately prevents duplicate work.

### Rule

> Mark a node visited when you discover it.

This pattern will appear repeatedly in graph problems.

---

# 17. Why DFS `while stack` Is Inside the `if`

This was the bug you encountered.

Correct:

```python
if new_unvisited_land:

    island += 1
    stack = [(r,c)]

    while stack:
        ...
```

Why?

Because DFS should start **only when a new island is discovered**.

If `while stack` were outside the `if`, the logic would become structurally confusing and could process the stack independently of discovering a new island.

Think:

```text
NEW ISLAND
   ↓
CREATE STACK
   ↓
RUN DFS
   ↓
FINISH ISLAND
```

---

# 18. Complexity

Let:

```text
m = number of rows
n = number of columns
```

There are:

```text
m × n
```

cells.

Each cell is visited at most once.

For each visited land cell, we check exactly 4 directions.

Therefore:

### Time

```text
O(m × n)
```

Why?

```text
m × n cells
× 4 neighbors
= 4mn
= O(mn)
```

The constant `4` disappears in Big-O notation.

### Space

Worst case, the entire grid is land.

Then:

```text
visited → O(mn)
stack   → O(mn)
```

So overall auxiliary space:

```text
O(mn)
```

---

# 19. The Graph Concepts We Learned

Number of Islands secretly teaches several major graph concepts.

| Concept             | In Number of Islands                    |
| ------------------- | --------------------------------------- |
| Node                | A grid cell                             |
| Edge                | Adjacent land cells                     |
| Graph               | The grid interpreted as connected cells |
| Neighbor            | Up/down/left/right cell                 |
| Traversal           | DFS                                     |
| Worklist            | Stack                                   |
| Memory              | Visited set                             |
| Connected component | One island                              |
| Component counting  | `island += 1`                           |
| Grid boundaries     | Valid-coordinate check                  |

---

# 20. The Pattern to Recognize in Future Problems

Whenever you see:

> "How many groups are there?"

or:

> "How many separate regions?"

or:

> "How many connected components?"

your brain should immediately ask:

```text
What are my nodes?
What are my edges?
What defines connectivity?
DFS or BFS?
Do I need visited?
Am I counting connected components?
```

For Number of Islands:

```text
nodes      → land cells
edges      → 4-direction adjacency
traversal  → DFS
visited    → yes
goal       → count connected components
```

---

# 21. One-Line Pattern

Write this in your memory:

> **Number of Islands = Count connected components in a grid using DFS/BFS.**

And the deeper pattern:

> **Find an unvisited node → count a new component → traverse the entire component → mark everything visited → continue.**

This pattern is going to come back again and again in graph problems.

---

# 22. What You Should Be Able to Explain Without Code

Before considering this problem mastered, you should be able to answer these without looking anything up:

1. Why is a grid a graph?
2. What is a node?
3. What is an edge?
4. Why are there only 4 neighbors?
5. Why do we need `visited`?
6. What does the stack do?
7. When do we increment `island`?
8. Why don't we increment for every `1`?
9. Why doesn't diagonal connection count?
10. Why is the complexity `O(m × n)`?
11. Why do we mark a cell visited when we discover it?
12. What exactly is a connected component?

If you can answer those, you haven't just memorized **Number of Islands**—you've learned the first major graph pattern.

---

## Graph Mastery Progress

```text
✅ Graph = nodes + edges
✅ Grid → Graph conversion
✅ Neighbors
✅ 4-direction movement
✅ Boundary checking
✅ Visited
✅ Stack
✅ Iterative DFS
✅ Connected components
✅ Component counting
⬜ BFS
⬜ Graph represented explicitly
⬜ Adjacency list
⬜ Adjacency matrix
⬜ Cycle detection
⬜ Topological sort
⬜ Union Find / DSU
⬜ Shortest paths
⬜ Dijkstra
⬜ MST
⬜ Advanced graph patterns
```

**Problem #1 complete: Number of Islands.**
