# LeetCode 104 — Maximum Depth of Binary Tree

**Pattern:** DFS + Recursion + Binary Tree

## Approach

1. **Base case:** If `node is None`, return `0` because an empty tree has depth `0`.
2. **Calculate left subtree depth:** `left = dfs(node.left)`.
3. **Calculate right subtree depth:** `right = dfs(node.right)`.
4. **Find maximum depth:** Return `1 + max(left, right)`.
   - `max(left, right)` selects the deeper subtree.
   - `+1` counts the current node.
5. Call `dfs(root)` to calculate the depth of the entire tree.

## Code

```python
class Solution:
    def maxDepth(self, root: TreeNode | None) -> int:
        def dfs(node):
            if node is None:
                return 0

            left = dfs(node.left)
            right = dfs(node.right)

            return 1 + max(left, right)

        return dfs(root)
```

## Example

```text
        1
       / \
      2   3
     /
    4
```

- Node `4`: depth = `1`
- Node `2`: depth = `1 + max(1, 0) = 2`
- Node `3`: depth = `1 + max(0, 0) = 1`
- Node `1`: depth = `1 + max(2, 1) = 3`

**Output:** `3`

## Key Takeaways

- **Why DFS?** It explores each subtree recursively to calculate its depth.
- **Why return `0` for `None`?** A missing node contributes no depth.
- **Why use `max(left, right)`?** We need the longest path from the current node to a leaf.
- **Why add `1`?** To count the current node.
- **Why call `dfs(node.left)` instead of using `node.left` directly?** `node.left` is a tree node, while `dfs(node.left)` returns its depth as an integer.
- **Why no `visited` set?** A normal binary tree has no cycles, so recursion can safely traverse its children.

## Complexity

Let `n` be the number of nodes and `h` be the tree height.

- **Time Complexity:** `O(n)` — each node is visited once.
- **Space Complexity:** `O(h)` — the recursion stack depends on the tree height; worst case `O(n)`, or `O(log n)` for a balanced tree.

## Recursion Formula

\[
\text{depth(node)} = 1 + \max(\text{depth(left)}, \text{depth(right)})
\]

**Remember:** Base case → solve left → solve right → combine the answers.

#######################################################################################################################
## Iterative Solution — BFS (Level Order Traversal)

**Pattern:** BFS + Queue + Level Counting

### Approach

1. If `root` is `None`, return `0`.
2. Initialize a queue with the root node using `deque`.
3. Set `depth = 0`.
4. While the queue is not empty:
   - Get `level_size = len(que)` to count nodes at the current level.
   - Process exactly `level_size` nodes.
   - Add each node's left and right children to the queue if they exist.
   - Increment `depth` after processing the entire level.
5. Return `depth`.

### Code

```python
from collections import deque

class Solution:
    def maxDepth(self, root: TreeNode | None) -> int:
        if root is None:
            return 0

        que = deque([root])
        depth = 0

        while que:
            level_size = len(que)

            for _ in range(level_size):
                node = que.popleft()

                if node.left:
                    que.append(node.left)

                if node.right:
                    que.append(node.right)

            depth += 1

        return depth
```

### Example

```text
        1          Level 1
       / \
      2   3        Level 2
     /
    4              Level 3
```

- Level 1: Process node `1` → `depth = 1`
- Level 2: Process nodes `2`, `3` → `depth = 2`
- Level 3: Process node `4` → `depth = 3`

**Output:** `3`

### Key Takeaways

- **Why BFS?** It visits the tree level by level.
- **Why `level_size = len(que)`?** It tells us how many nodes belong to the current level, so newly added children are processed in the next level.
- **Why increment `depth` after the `for` loop?** Each completed level adds one to the tree's depth.
- **Why use `deque`?** `popleft()` removes the first element efficiently.
- **Why no `visited` set?** A normal binary tree has no cycles.

### Complexity

Let `n` be the number of nodes.

- **Time Complexity:** `O(n)` — each node is processed once.
- **Space Complexity:** `O(n)` — the queue can hold up to `O(n)` nodes in the worst case.

### DFS vs. BFS

| DFS (Recursive) | BFS (Iterative) |
|---|---|
| Calculates left and right subtree depths | Counts levels |
| Uses the recursion call stack | Uses a queue |
| Returns `1 + max(left, right)` | Increments `depth` per level |
| Space: `O(h)` | Space: `O(n)` worst case |

**Remember:** DFS finds the maximum depth by combining subtree depths; BFS finds it by counting levels.
