# LeetCode 102 — Binary Tree Level Order Traversal

**Pattern:** Binary Tree + BFS + Queue

## Approach
1. If `root` is `None`, return `[]`.
2. Initialize a queue with the root node.
3. While the queue is not empty:
   - Store the current queue size in `level_size`.
   - Process exactly `level_size` nodes.
   - Add each node's value to the current `level`.
   - Enqueue its existing left and right children.
   - Append the completed level to the result.
4. Return the result.

## Python Solution

```python
from collections import deque

class Solution:
    def levelOrder(self, root: TreeNode | None) -> list[list[int]]:
        if root is None:
            return []

        result = []
        queue = deque([root])

        while queue:
            level = []
            level_size = len(queue)

            for _ in range(level_size):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)

                if node.right:
                    queue.append(node.right)

            result.append(level)

        return result
```

## Complexity
Let `n` be the number of nodes.

- **Time:** `O(n)` — each node is processed exactly once.
- **Auxiliary space:** `O(n)` — the queue can hold up to `O(n)` nodes.
- **Output space:** `O(n)` — the result stores all node values.

## Key Takeaways
- BFS explores nodes level by level.
- `deque.popleft()` efficiently removes the front element.
- `level_size = len(queue)` captures the number of nodes at the current level.
- `range(level_size)` ensures newly added children are processed in the next level.
- Enqueue only existing children; never enqueue `None`.

**Remember:** BFS + queue + fixed queue size per level = level-order traversal.
