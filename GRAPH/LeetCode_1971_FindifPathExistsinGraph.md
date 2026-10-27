# LeetCode 1971 — Find if Path Exists in Graph

**Pattern:** Graph + Adjacency List + BFS

## Approach
1. Build an adjacency list from `edges`.
2. If `source == destination`, return `True`.
3. Initialize `visited = {source}` and a queue with `source`.
4. Use BFS to explore each node's unvisited neighbors.
5. If a neighbor is `destination`, return `True`.
6. If BFS finishes without finding the destination, return `False`.

## Python Solution

```python
from collections import defaultdict, deque

class Solution:
    def validPath(self, n: int, edges: list[list[int]], source: int, destination: int) -> bool:
        adj_list = defaultdict(list)

        for u, v in edges:
            adj_list[u].append(v)
            adj_list[v].append(u)

        if source == destination:
            return True

        visited = {source}
        queue = deque([source])

        while queue:
            node = queue.popleft()

            for neigh in adj_list[node]:
                if neigh not in visited:
                    if neigh == destination:
                        return True

                    visited.add(neigh)
                    queue.append(neigh)

        return False
```

## Complexity

Let `n` = number of vertices and `m` = number of edges.

- **Time:** `O(n + m)`
  - Building the adjacency list: `O(m)`
  - BFS: `O(n + m)` because each vertex is visited at most once and each edge is examined at most twice.
- **Space:** `O(n + m)`
  - Adjacency list: `O(n + m)`
  - Visited set and queue: `O(n)`

## Key Takeaways
- An undirected edge `[u, v]` means `u` is connected to `v` and `v` is connected to `u`.
- An adjacency list avoids scanning every edge for every node.
- Mark nodes as visited **before enqueueing** them to prevent duplicates.
- `deque.popleft()` efficiently removes the next BFS node.
- Without an adjacency list, repeatedly scanning all edges can take `O(n × m)` time.

**Remember:** BFS explores the graph level by level. If the destination is reachable, BFS will eventually find it.

#################################################################################################################
# LeetCode 1971: Find if Path Exists in Graph

## 1. Pattern
**Graph + DFS (Depth-First Search)**

Use DFS to determine whether a path exists between a `source` node and a `destination` node in an **undirected graph**.

## 2. Approach

1. **Build an adjacency list**
   - Each edge `[p, q]` means `p` connects to `q` and `q` connects to `p`, because the graph is undirected.
   - Store the neighbors of every node in `adjlist`.

2. **Create a visited set**
   - Use `visit` to prevent visiting the same node repeatedly.
   - This avoids infinite recursion when the graph contains cycles.

3. **Perform DFS from the source**
   - If `node == destination`, return `True`.
   - Mark the current node as visited.
   - Explore each unvisited neighbor recursively.
   - If any recursive call returns `True`, return `True`.
   - If no path is found, return `False`.

4. **Return the DFS result**
   - Call `dfs(source)` to check whether the destination is reachable.

## 3. Code

```python
from collections import defaultdict

class Solution:
    def validPath(self, n: int, edges: list[list[int]], source: int, destination: int) -> bool:
        adjlist = defaultdict(list)
        visit = set()

        # Build the undirected graph
        for p, q in edges:
            adjlist[p].append(q)
            adjlist[q].append(p)

        def dfs(node):
            # Destination reached
            if node == destination:
                return True

            # Mark current node as visited
            visit.add(node)

            # Explore unvisited neighbors
            for neigh in adjlist[node]:
                if neigh not in visit:
                    if dfs(neigh):
                        return True

            # No path found from this node
            return False

        return dfs(source)
```

## 4. Key Takeaways

- **Undirected graph:** Add both `p → q` and `q → p`.
- **Adjacency list:** Efficiently stores each node's neighbors.
- **Visited set:** Prevents repeated visits and infinite recursion.
- **Base case:** If `node == destination`, return `True`.
- **Recursive search:** If any neighbor leads to the destination, return `True`.
- **Return `False`:** If all reachable neighbors have been explored without finding the destination.
- **Final call:** Use `return dfs(source)` so the result is passed back to the caller.

### Common mistakes to avoid

1. **Variable-name typo:** Be consistent with `neigh`; Python treats `nigh` and `neigh` as different variables.
2. **Missing `return False`:** Without it, DFS may return `None` when no path exists.
3. **Missing `return dfs(source)`:** The main function must return the result of the search.
4. **Forgetting to mark nodes visited:** This can cause infinite recursion in cyclic graphs.
5. **Forgetting reverse edges:** The graph is undirected, so every edge must be added in both directions.

## 5. Complexity Analysis

Let:
- `V` = number of vertices (`n`)
- `E` = number of edges

**Time Complexity: `O(V + E)`**
- Building the adjacency list takes `O(V + E)` overall, including graph initialization if all vertices are represented.
- DFS visits each vertex at most once and examines each edge at most twice.

**Space Complexity: `O(V + E)`**
- Adjacency list: `O(V + E)`.
- Visited set: `O(V)`.
- Recursive call stack: up to `O(V)` in the worst case.

## 6. When to Use This Pattern

Use DFS when a graph problem asks whether:
- A path exists between two nodes.
- A node is reachable from another node.
- Two nodes belong to the same connected component.

**Remember:** Build the graph → track visited nodes → DFS from the source → return `True` if the destination is reached; otherwise return `False`.
