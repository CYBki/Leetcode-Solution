# Find All Duplicates in Array - LeetCode Problem Solution

## Problem Description

Given an integer array nums of length n where all the integers of nums are in the range [1, n] and each integer appears at most twice, return an array of all the integers that appears twice.

**Constraints:**
- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`
- Each element appears **at most twice**

**Follow-up Challenge:** You must write an algorithm that runs in O(n) time and uses only constant auxiliary space, excluding the space needed to store the output

### Examples

| Input | Output | Explanation |
|-------|--------|-------------|
| `[4,3,2,7,8,2,3,1]` | `[2,3]` | Numbers 2 and 3 appear twice |
| `[1,1,2]` | `[1]` | Only number 1 appears twice |
| `[1]` | `[]` | No duplicates found |

## Solution Approach

### Algorithm: Hash Set Tracking

The solution uses a **hash set** to track elements we've seen before and identify duplicates on the fly.

**Strategy:**
1. Use a set to keep track of elements we've already encountered
2. For each element in the array:
   - If it's already in the set → it's a duplicate, add to result
   - If it's not in the set → add it to the set for future tracking
3. Return all found duplicates

### Why This Approach Works

- **Linear scan**: We only need to traverse the array once
- **Immediate detection**: Duplicates are identified as soon as we encounter them the second time
- **No sorting required**: Unlike some approaches, we don't need to modify the input
- **Handles multiple duplicates**: Works correctly even if the same number appears twice

## Implementation

```python
from typing import List

class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        seen, dup = set(), []
        
        for i in nums:
            if i in seen:
                dup.append(i)
            else:
                seen.add(i)
        
        return dup
```

## Step-by-Step Walkthrough

Let's trace through the example `nums = [4,3,2,7,8,2,3,1]`:

| Step | Current Element | Seen Set | Found in Set? | Action | Duplicates List |
|------|----------------|----------|---------------|--------|-----------------|
| 1    | 4              | {}       | No            | Add 4 to seen | [] |
| 2    | 3              | {4}      | No            | Add 3 to seen | [] |
| 3    | 2              | {4,3}    | No            | Add 2 to seen | [] |
| 4    | 7              | {4,3,2}  | No            | Add 7 to seen | [] |
| 5    | 8              | {4,3,2,7}| No            | Add 8 to seen | [] |
| 6    | 2              | {4,3,2,7,8}| **Yes**    | Add 2 to duplicates | [2] |
| 7    | 3              | {4,3,2,7,8}| **Yes**    | Add 3 to duplicates | [2,3] |
| 8    | 1              | {4,3,2,7,8}| No         | Add 1 to seen | [2,3] |

**Final Result:** `[2,3]`

## Complexity Analysis

### Time Complexity: O(n)
- **Linear traversal**: We iterate through the array exactly once
- **Set operations**: Both `in` and `add` operations on a set are O(1) on average
- **Total**: O(n) × O(1) = O(n)

### Space Complexity: O(n)
- **Hash set storage**: In the worst case, we store all n elements in the set (when no duplicates exist)
- **Result list**: Stores at most n/2 duplicates (since each number appears at most twice)
- **Total auxiliary space**: O(n) for the set + O(k) for duplicates where k ≤ n/2
- **Overall**: O(n)

### Detailed Complexity Breakdown

**Why O(n) time?**
1. Single pass through the array: O(n)
2. Hash set lookup: O(1) average case
3. Hash set insertion: O(1) average case
4. List append: O(1) amortized

**Space complexity considerations:**
- **Best case**: O(1) auxiliary space (all elements are unique, but we still need the set)
- **Worst case**: O(n) auxiliary space (no duplicates, set contains all elements)
- **Average case**: O(n) auxiliary space

## Alternative Approaches

### 1. Sorting Approach
```python
def findDuplicates(self, nums: List[int]) -> List[int]:
    nums.sort()
    duplicates = []
    
    for i in range(1, len(nums)):
        if nums[i] == nums[i-1] and nums[i] not in duplicates:
            duplicates.append(nums[i])
    
    return duplicates
```

**Pros:** O(1) auxiliary space (if in-place sorting allowed)
**Cons:** O(n log n) time complexity, modifies input

### 2. Array Marking Approach (Optimal for Follow-up)
```python
def findDuplicates(self, nums: List[int]) -> List[int]:
    result = []
    
    for num in nums:
        index = abs(num) - 1
        if nums[index] < 0:
            result.append(abs(num))
        else:
            nums[index] = -nums[index]
    
    return result
```

**Pros:** O(n) time, O(1) auxiliary space
**Cons:** Modifies the input array

### 3. Frequency Counter Approach
```python
def findDuplicates(self, nums: List[int]) -> List[int]:
    from collections import Counter
    
    count = Counter(nums)
    return [num for num, freq in count.items() if freq == 2]
```

**Pros:** Very readable, handles any frequency
**Cons:** O(n) space, two passes through data

## Approach Comparison

| Approach | Time Complexity | Space Complexity | Modifies Input | Readability |
|----------|----------------|------------------|----------------|-------------|
| **Hash Set (Our Solution)** | O(n) | O(n) | No | High |
| Array Marking | O(n) | O(1) | Yes | Medium |
| Sorting | O(n log n) | O(1) | Yes | High |
| Frequency Counter | O(n) | O(n) | No | Very High |

## Key Insights

### Problem Constraints Analysis
- **Range [1, n]**: This constraint enables the array marking approach
- **At most twice**: Ensures we never have more than one duplicate of each number
- **Length n**: Guarantees that if there are duplicates, some numbers must be missing

### When to Use This Solution
- **Input preservation required**: When you cannot modify the original array
- **Simplicity preferred**: When code readability is more important than optimal space
- **General case**: When the constraint "range [1, n]" might not hold in variations

### Edge Cases Handled
1. **Empty array**: Returns empty list
2. **Single element**: Returns empty list (no duplicates possible)
3. **No duplicates**: Returns empty list after checking all elements
4. **All duplicates**: Returns list with n/2 elements

## Test Cases

```python
def test_solution():
    solution = Solution()
    
    test_cases = [
        ([4,3,2,7,8,2,3,1], [2,3]),
        ([1,1,2], [1]),
        ([1], []),
        ([1,2,3,4,5], []),
        ([1,1,2,2,3,3], [1,2,3]),
        ([5,4,6,7,9,3,10,9,5,6], [5,6,9])
    ]
    
    for i, (nums, expected) in enumerate(test_cases):
        result = solution.findDuplicates(nums.copy())  # Use copy to preserve original
        # Sort for comparison since order might differ
        result.sort()
        expected.sort()
        status = "✓" if result == expected else "✗"
        print(f"Test {i+1}: {nums} → {result} (Expected: {expected}) {status}")

test_solution()
```

## Summary

The hash set approach provides an excellent balance between **simplicity**, **readability**, and **performance**. While it doesn't achieve the O(1) space complexity mentioned in the follow-up challenge, it offers:

- ✅ **O(n) time complexity** - meets the time requirement
- ✅ **Clear and maintainable code** - easy to understand and debug  
- ✅ **Input preservation** - doesn't modify the original array
- ✅ **Robust handling** - works correctly for all edge cases

For interview scenarios where code clarity and correctness are prioritized over optimal space usage, this solution demonstrates strong problem-solving skills and clean coding practices.