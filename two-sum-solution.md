# Two Sum Problem - Detailed Solution

## Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

### Example

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

## Solution

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        """
        Finds two numbers in the list that add up to the target.
        Args:
        - nums: List of integers
        - target: Target sum
        Returns:
        - List containing the indices of the two numbers that add up to the target
        """
        hashmap = {}  # Initialize a hashmap to store number-frequency pairs
        for i, num in enumerate(nums):
            complement = target - num  # Calculate the complement needed to reach the target
            if complement in hashmap:
                # If the complement exists in the hashmap, return the indices of the two numbers
                return [hashmap[complement], i]
            # Otherwise, store the current number and its index in the hashmap
            hashmap[num] = i
```

## Step-by-Step Explanation

### Core Idea

For each number, we ask: "What number do I need to add to this number to reach the target?"

```
If current number = 7 and target = 9
Then complement = 9 - 7 = 2
```

### Walkthrough with Example

Let's trace through `nums = [2, 7, 11, 15]`, `target = 9`:

#### Step 1: i=0, num=2
```python
complement = 9 - 2 = 7
# Is 7 in hashmap? No (hashmap is empty)
# Add 2 to hashmap: hashmap = {2: 0}
```

#### Step 2: i=1, num=7
```python
complement = 9 - 7 = 2
# Is 2 in hashmap? YES! (at index 0)
# Return [hashmap[2], 1] = [0, 1]
```

**Result:** `[0, 1]` ✓

### Visual Representation

```
Initial: hashmap = {}

Step 1 (i=0, num=2):
- Looking for: 7 (because 2 + 7 = 9)
- 7 not found → hashmap = {2: 0}

Step 2 (i=1, num=7):
- Looking for: 2 (because 7 + 2 = 9)  
- 2 FOUND! → at index 0
- Answer: [0, 1]
```

## Key Points

### Why HashMap?

1. **Fast Lookup:** O(1) average time complexity
2. **Store Index Information:** We need to return indices, not values

### Why Add to HashMap After Checking?

This prevents using the same element twice:

```python
# Wrong approach (adding before checking):
nums = [3, 3], target = 6
# Would incorrectly return [0, 0]

# Correct approach (adding after checking):
nums = [3, 3], target = 6
# Correctly returns [0, 1]
```

## Complexity Analysis

### Time Complexity: O(n)
- We traverse the list containing n elements only once
- Each lookup in the hash table costs O(1) time

### Space Complexity: O(n)
- In the worst case, we need to store n-1 elements in the hash table

## Alternative Solutions Comparison

| Approach | Time Complexity | Space Complexity | Description |
|----------|----------------|------------------|-------------|
| **HashMap** | O(n) | O(n) | Optimal time, uses extra space |
| **Brute Force** | O(n²) | O(1) | Nested loops, no extra space |
| **Sort + Two Pointers** | O(n log n) | O(n) | Sort first, then use two pointers |

### Brute Force Solution
```python
def twoSum_bruteforce(nums, target):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```

## Practice Tips

1. **Understand the Problem:** We need indices, not values
2. **Choose the Right Data Structure:** HashMap for O(1) lookups
3. **Algorithm Design:**
   - Calculate complement for each number
   - Check if complement was seen before
   - If yes, return indices
   - If no, store current number and continue

## Common Mistakes to Avoid

1. Using the same element twice
2. Returning values instead of indices
3. Adding to hashmap before checking (can cause same element usage)

## Related Problems

- 3Sum
- 4Sum
- Two Sum II (sorted array)
- Two Sum III (data structure design)

---

*This solution provides optimal O(n) time complexity by trading off space for speed, making it the preferred approach for most scenarios.*