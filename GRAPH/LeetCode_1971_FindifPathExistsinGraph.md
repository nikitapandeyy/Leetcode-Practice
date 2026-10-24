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
