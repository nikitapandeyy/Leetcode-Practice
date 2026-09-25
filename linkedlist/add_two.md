## Add Two Numbers — LC 2 📝

**Pattern:** Linked List + Carry

### Key idea

The linked lists store digits in **reverse order**, so we naturally start from the **ones digit**.

```text
342 → 2 → 4 → 3
465 → 5 → 6 → 4

2 + 5 = 7
4 + 6 = 10 → digit 0, carry 1
3 + 4 + 1 = 8

Result: 7 → 0 → 8  = 807
```

### Formula

For every pair of digits:

```python
total = d1 + d2 + carry
digit = total % 10
carry = total // 10
```

* `% 10` → digit we put into the new node
* `// 10` → carry to the next position

### Code

```python
class Solution:
    def addTwoNumbers(
        self,
        l1: Optional[ListNode],
        l2: Optional[ListNode]
    ) -> Optional[ListNode]:

        dummy = ListNode(0)
        temp = dummy
        carry = 0

        while l1 or l2 or carry:

            d1 = l1.val if l1 else 0
            d2 = l2.val if l2 else 0

            total = d1 + d2 + carry

            digit = total % 10
            carry = total // 10

            temp.next = ListNode(digit)
            temp = temp.next

            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None

        return dummy.next
```

### Why `dummy`?

```python
dummy = ListNode(0)
temp = dummy
```

`dummy` gives us an easy starting point for building the result.

Afterward:

```python
return dummy.next
```

because `dummy` itself isn't part of the answer.

### Why `while l1 or l2 or carry`?

We continue if:

* `l1` still has digits, **or**
* `l2` still has digits, **or**
* there's a remaining carry.

Example:

```text
9 → 9
1

99 + 1 = 100
```

At the end, the remaining `carry = 1` creates the final node.

### Complexity

**Time:** `O(max(m, n))`
**Space:** `O(max(m, n))` for the result list.

### 🧠 One-line takeaway

> **Traverse both lists forward → add digits + carry → store `total % 10` → pass `total // 10` forward.**
