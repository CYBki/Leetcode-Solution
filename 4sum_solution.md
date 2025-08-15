# 4Sum Problem Solution Analysis

## Problem Description

The 4Sum problem asks us to find all unique quadruplets in an array that sum up to a given target value. Given an array `nums` of `n` integers and an integer `target`, we need to return all unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:
- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are distinct
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

The solution set must not contain duplicate quadruplets.

## Solution Approach

This solution uses a generalized k-Sum approach with two-pointer technique:

### Key Components

1. **Generalized kSum Function**: A recursive function that can solve k-Sum problems for any k ≥ 2
2. **Two-Pointer twoSum**: An optimized base case for when k = 2
3. **Sorting**: The array is sorted first to enable the two-pointer technique and duplicate handling

### Algorithm Breakdown

#### 1. Main Function (`fourSum`)
```python
nums.sort()
return kSum(nums, target, 4)
```
- Sorts the input array to enable two-pointer technique
- Calls the generalized kSum function with k=4

#### 2. Generalized kSum Function
The recursive function handles the general case:

**Base Cases and Optimizations:**
- If array is empty, return empty result
- Calculate average value needed: `target // k`
- Early termination: if smallest element > average or largest element < average, return empty

**Recursive Case:**
- For k > 2: iterate through array, fix one element, recursively solve (k-1)Sum
- Skip duplicates to avoid duplicate quadruplets
- Combine current element with results from recursive calls

#### 3. Two-Pointer twoSum (Base Case)
When k=2, uses an optimized two-pointer approach:
- Left pointer starts at beginning, right pointer at end
- Move pointers based on current sum vs target
- **Smart duplicate handling**: Skip duplicates by moving the pointer that just encountered a duplicate element
  - If `curr_sum < target` OR left pointer is on a duplicate: move left pointer
  - If `curr_sum > target` OR right pointer is on a duplicate: move right pointer
  - This ensures we don't process the same pair multiple times

### Duplicate Handling Strategy

The solution handles duplicates at two levels:
1. **In kSum**: Skip duplicate elements when fixing the first element of each subset
2. **In twoSum**: Skip duplicates when moving left and right pointers

## Complete Solution Code

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:

        def kSum(nums: List[int], target: int, k: int) -> List[List[int]]:
            res = []

            # If we have run out of numbers to add, return res.
            if not nums:
                return res

            # There are k remaining values to add to the sum. The
            # average of these values is at least target // k.
            average_value = target // k

            # We cannot obtain a sum of target if the smallest value
            # in nums is greater than target // k or if the largest
            # value in nums is smaller than target // k.
            if average_value < nums[0] or nums[-1] < average_value:
                return res

            if k == 2:
                return twoSum(nums, target)

            for i in range(len(nums)):
                if i == 0 or nums[i - 1] != nums[i]:
                    for subset in kSum(nums[i + 1 :], target - nums[i], k - 1):
                        res.append([nums[i]] + subset)

            return res

        def twoSum(nums: List[int], target: int) -> List[List[int]]:
            res = []
            lo, hi = 0, len(nums) - 1

            while lo < hi:
                curr_sum = nums[lo] + nums[hi]
                if curr_sum < target or (lo > 0 and nums[lo] == nums[lo - 1]):
                    lo += 1
                elif curr_sum > target or (
                    hi < len(nums) - 1 and nums[hi] == nums[hi + 1]
                ):
                    hi -= 1
                else:
                    res.append([nums[lo], nums[hi]])
                    lo += 1
                    hi -= 1

            return res

        nums.sort()
        return kSum(nums, target, 4)
```

## Code Walkthrough

The complete solution consists of three main parts:

1. **Main function (`fourSum`)**: Sorts the array and initiates the kSum with k=4
2. **Generalized kSum function**: Handles the recursive breakdown of k-Sum problems
3. **Optimized twoSum function**: Uses two-pointer technique for the base case

## Complexity Analysis

### Time Complexity: O(n³)

**Detailed Breakdown:**
- **Sorting**: O(n log n)
- **Recursion levels**: 2 levels (4Sum → 3Sum → 2Sum)
- **First level (4Sum)**: Iterate through n elements, each calling 3Sum
- **Second level (3Sum)**: For each 4Sum iteration, iterate through ~n elements, each calling 2Sum
- **Third level (2Sum)**: Each call takes O(n) time with two pointers
- **Total**: O(n) × O(n) × O(n) = O(n³)

**Why not O(n⁴)?**
The two-pointer technique in twoSum reduces the innermost loop from O(n²) to O(n), giving us the crucial optimization from O(n⁴) to O(n³).

**Note**: The O(n log n) sorting cost is dominated by the O(n³) main algorithm.

### Space Complexity: O(n) to O(n³)

**Components:**
- **Recursion stack**: O(k) = O(4) = O(1) - constant depth for function calls
- **Array slicing**: O(n) - each recursive call creates new array slices `nums[i + 1:]`
- **Total auxiliary space**: O(n) due to array slicing
- **Result storage**: O(number of valid quadruplets)

**Detailed Analysis:**
- **Best case**: O(n) - when few or no valid quadruplets exist
- **Worst case**: O(n³) - when result storage dominates (many valid solutions)
- **Auxiliary space**: Always O(n) due to recursive array slicing

**Memory Optimization Note:**
The array slicing `nums[i + 1:]` creates new arrays at each recursive level, contributing O(n) space. This could be optimized by passing indices instead of slicing arrays.

### Comparison with Brute Force

| Approach | Time Complexity | Space Complexity | Notes |
|----------|----------------|------------------|--------|
| Brute Force | O(n⁴) | O(1) | Four nested loops |
| This Solution | O(n³) | O(n) to O(n³) | Two-pointer optimization + array slicing |

## Optimizations Used

1. **Early Termination**: Skip impossible cases using average value check
2. **Duplicate Skipping**: Avoid processing duplicate elements
3. **Two-Pointer Technique**: Linear scan instead of nested loops for 2Sum
4. **Sorted Array**: Enables both two-pointer technique and early termination

## Edge Cases Handled

- Empty array
- Array with fewer than 4 elements
- No valid quadruplets exist
- All elements are the same
- Target is very large or very small compared to array elements
- Multiple duplicate quadruplets in different positions

## Usage Example

```python
solution = Solution()

# Example 1
nums1 = [1, 0, -1, 0, -2, 2]
target1 = 0
result1 = solution.fourSum(nums1, target1)
# Output: [[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]

# Example 2
nums2 = [2, 2, 2, 2, 2]
target2 = 8
result2 = solution.fourSum(nums2, target2)
# Output: [[2, 2, 2, 2]]
```

This solution provides an efficient approach to the 4Sum problem with optimal time complexity and robust handling of edge cases and duplicates.