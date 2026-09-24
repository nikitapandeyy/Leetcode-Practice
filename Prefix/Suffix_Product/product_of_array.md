Product of Array Except Self — LC 238

Pattern: Prefix/Suffix Product
Technique: Two passes, no division

🎯 Goal

For every index i, calculate:

product of all elements EXCEPT nums[i]
Example:

nums = [1, 2, 3, 4]

answer = [24, 12, 8, 6]
🧠 Core Idea

For every position:

answer[i] =
(product of everything LEFT)
×
(product of everything RIGHT)
Example:

nums:       1   2   3   4

left:       1   1   2   6
right:     24  12   4   1

answer:    24  12   8   6
Pass 1 — Left Product

Maintain a running product pr.

pr = 1

for i in range(len(nums)):
    result[i] = pr
    pr *= nums[i]
After this:

result[i] = product of elements before i
For [1,2,3,4]:

result = [1, 1, 2, 6]
Pass 2 — Right Product

Traverse from right → left.

Maintain sf = suffix/right product.

sf = 1

for i in range(len(nums)-1, -1, -1):
    result[i] *= sf
    sf *= nums[i]
Now:

result[i] =
left product × right product
⭐ Important Insight

We don't need to create separate left and right arrays.

We can:

Store the left product directly in result.

Traverse backward and multiply by the right product.

This gives O(1) extra space apart from the output array.

Why no division?

Division fails when the array contains 0.

The prefix/suffix approach naturally handles:

[1, 2, 0, 4]
without any special division logic.

Complexity

Time  → O(n)
Space → O(1) extra space
(Output array is not counted as extra space.)

🔑 One-line takeaway

Product Except Self = prefix product from the left × suffix product from the right.





class Solution:

    def productExceptSelf(self, nums: list[int]) -> list[int]:

        result = [0] * len(nums)



        pr = 1

        for i in range(len(nums)):

            result[i] = pr

            pr = pr * nums[i]



        sf = 1

        for i in range(len(nums) - 1, -1, -1):

            result[i] = sf * result[i]

            sf = sf * nums[i]



        return result