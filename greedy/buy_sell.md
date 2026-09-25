Yes — this is **Best Time to Buy and Sell Stock (LC 121)**, and your solution is correct. ✅

### Pattern: **Greedy / One-pass**

Your key idea is:

```python
buy = min(buy, price)
```

You keep track of the **cheapest price seen so far**.

Then:

```python
profit = price - buy
```

asks:

> If I sell today, what is my profit using the cheapest buying price I've seen?

And:

```python
maxp = max(maxp, profit)
```

keeps the best profit.

### Example

```text
prices = [7, 1, 5, 3, 6, 4]
```

Think:

```text
price    buy     profit    max_profit
7        7        0          0
1        1        0          0
5        1        4          4
3        1        2          4
6        1        5          5
4        1        3          5
```

Answer:

```text
5
```

Buy at `1`, sell at `6`.

### One small improvement

You don't need:

```python
maxp = float("-inf")
```

You can simply do:

```python
maxp = 0
```

because we aren't allowed to make a negative profit—we can always choose not to trade.

### Your code

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        buy = prices[0]
        maxp = 0

        for price in prices:
            buy = min(buy, price)
            profit = price - buy
            maxp = max(maxp, profit)

        return maxp
```

### Complexity

* **Time:** O(n)
* **Space:** O(1)

### 🧠 Pattern recognition

When you see:

> Buy once, sell once, maximize profit.

Think:

**Track the minimum so far + calculate current profit.**

The important insight is:

> **At every price, ask: "If I sell today, what is my best possible profit?"**
