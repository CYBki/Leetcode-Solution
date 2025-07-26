# 3Sum Problem - Solution and Analysis

## Problem Definition

The **3Sum** problem is about finding all unique triplets in an integer array that sum up to zero.

### Problem Requirements:
- Input: Integer array `nums`
- Output: All unique triplets that sum to 0
- Cannot use the same index multiple times
- Result list should not contain duplicate triplets

### Example:
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

## Solution Strategy

This solution uses the **Two Pointers** technique. The approach consists of the following steps:

### 1. Sorting
Sort the array in ascending order. This makes it easier to skip duplicates and apply the two pointers technique.

### 2. Main Loop
For each element, treat it as the first element and find the remaining two elements using two pointers.

### 3. Optimizations
- If current element > 0, break the loop since we can't achieve zero sum with positive numbers
- Skip duplicate values to avoid duplicate triplets

## Code Explanation

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        target_sum = 0
        nums.sort()  # Sort the array
        result = []

        for i in range(len(nums)):
            # Optimization: Can't achieve zero sum with positive numbers
            if nums[i] > 0:
                break
            
            # Skip duplicate values
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            # Two pointers technique
            left = i + 1
            right = len(nums) - 1

            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]

                if current_sum == target_sum:
                    result.append([nums[i], nums[left], nums[right]])

                    # Skip duplicates
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    left += 1
                    right -= 1
                elif current_sum < target_sum:
                    left += 1  
                else:
                    right -= 1  

        return result
```

### Algorithm Steps:

1. **Sorting**: `nums.sort()` - O(n log n)
2. **Main loop**: For each element (i)
3. **Optimization check**: Break if `nums[i] > 0`
4. **Duplicate check**: Skip processing same value again
5. **Two Pointers**: `left = i + 1`, `right = len(nums) - 1`
6. **Sum check**:
   - `current_sum == 0`: Add to result, skip duplicates
   - `current_sum < 0`: `left++` (increase sum)
   - `current_sum > 0`: `right--` (decrease sum)

## Complexity Analysis

### Time Complexity: O(n²)
- **Sorting**: O(n log n)
- **Main loop**: O(n) - At most n iterations
- **Two pointers**: O(n) - At most n steps per main loop iteration
- **Total**: O(n log n) + O(n × n) = O(n²)

### Space Complexity: O(1) or O(n)
- **Auxiliary Space**: O(1) - Only using a few pointers and variables
- **Output Space**: O(k) - k is the number of triplets found (worst case O(n²))
- **Sorting Space**: Depends on sorting algorithm used (typically O(log n))

**Note**: If we don't count output space, auxiliary space complexity is O(1).

## Advantages and Disadvantages

### ✅ Advantages:
- Optimal time complexity O(n²)
- Minimal space usage
- Efficient duplicate handling
- Elegant solution using two pointers technique

### ❌ Disadvantages:
- Modifies input array (sorting)
- Additional O(n log n) sorting cost

## Alternative Approaches

### 1. Brute Force - O(n³)
Use three nested loops to check all combinations.

### 2. HashSet Approach - O(n²)
For each pair of elements, search for the third element in a hashset.

### 3. Two-Phase Hash
Store all pairs in a hashmap first, then search for the third element.

**Conclusion**: The two pointers approach is the most optimal solution in terms of both time and space complexity.

## Test Cases

```python
# Test Case 1
nums = [-1,0,1,2,-1,-4]
# Expected: [[-1,-1,2],[-1,0,1]]

# Test Case 2  
nums = [0,1,1]
# Expected: []

# Test Case 3
nums = [0,0,0]
# Expected: [[0,0,0]]
```

This solution is one of the most efficient approaches for LeetCode problem 15 (3Sum) and is frequently asked in technical interviews.
