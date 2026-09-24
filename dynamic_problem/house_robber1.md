dynamic 
House Robber — LC 198

Pattern: 1D Dynamic Programming

Problem

Given money in houses, rob houses to get the maximum amount, but you cannot rob two adjacent houses.

Example:

nums = [2, 7, 9, 3, 1]

Best = 2 + 9 + 1 = 12
🧠 DP Definition

dp[i] = maximum money we can rob from houses 0 → i
The important point:

dp[i] is not the money in house i.
It is the best total considering everything up to house i.

Base Cases

For the first house:

dp[0] = nums[0]
For the first two houses, we can rob only one:

dp[1] = max(nums[0], nums[1])
Example:

[7, 2] → 7
[2, 7] → 7
🔑 Recurrence

At house i, there are 2 choices:

1. Skip current house

dp[i-1]
2. Rob current house

We cannot rob the previous house, so:

nums[i] + dp[i-2]
Therefore:

dp[i] = max(dp[i-1], nums[i] + dp[i-2])
Example

nums = [2, 7, 9, 3, 1]

dp[0] = 2
dp[1] = 7

dp[2] = max(7, 9 + 2)  = 11
dp[3] = max(11, 3 + 7) = 11
dp[4] = max(11, 1 + 11) = 12
Final answer:

12
Why i-2?

If we rob house i:

[i-2] [i-1] [i]
       ❌    ✅
We cannot use i-1, so the best previous amount comes from i-2.

Complexity

Time  → O(n)
Space → O(n)
⭐ Pattern Recognition

Think 1D DP when:

You move through an array/index by index.

At each position you have choices.

The current choice depends on previously solved positions.

You need to maximize/minimize something.

One-line takeaway

At each house: SKIP → dp[i-1]; ROB → nums[i] + dp[i-2]; take the maximum.





class Solution:

    def rob(self, nums: list[int]) -> int:

        if len(nums) == 1:

            return nums[0]



        dp = [0] * len(nums)



        dp[0] = nums[0]

        dp[1] = max(nums[0], nums[1])



        for i in range(2, len(nums)):

            skip = dp[i - 1]

            rob = nums[i] + dp[i - 2]



            dp[i] = max(skip, rob)



        return dp[-1]