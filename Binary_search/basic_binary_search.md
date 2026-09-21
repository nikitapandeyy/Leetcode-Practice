**Binary Search:** Sorted array → repeatedly check middle → eliminate half of the search space.

`mid = left + (right-left)//2`

`target < nums[mid]` → `right = mid-1`  
`target > nums[mid]` → `left = mid+1`  
`target == nums[mid]` → return `mid`

**Time: O(log n), Space: O(1)**