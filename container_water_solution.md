# Container With Most Water - Problem and Solution

## Problem Description

Given an integer array `height` where `height[i]` represents the height of the i-th line. These lines form a container with the x-axis. Find two lines that together with the x-axis forms a container that contains the most water.

**Example:**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
```

Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water the container can contain is 49.

## Solution Approach: Two Pointers

### Algorithm Logic

1. **Two pointer technique**: Start with two pointers at the beginning (`left`) and end (`right`) of the array
2. **Area calculation**: Calculate current area at each step: `min(height[left], height[right]) * (right - left)`
3. **Pointer movement**: Move the pointer with the shorter line inward
4. **Maximum tracking**: Update maximum area at each step

### Why Move the Shorter Side?

- Water level is always determined by the shorter line
- Moving the taller side will never give us a larger area
- By moving the shorter side, we have a chance to find a taller line

## Code Implementation

```python
from typing import List

class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1 
        max_area = 0

        while left < right:
            # Calculate current area
            current_area = min(height[left], height[right]) * (right - left)
            max_area = max(current_area, max_area)

            # Move the pointer with shorter height
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

## Complexity Analysis

### Time Complexity: O(n)
- Each element is visited only once
- While loop runs at most n times (n = array length)
- Constant time operations in each iteration

### Space Complexity: O(1)
- Only constant extra space is used
- Variables: `left`, `right`, `max_area`, `current_area`
- Independent of input array size

## Step-by-Step Example

Input: `[1,8,6,2,5,4,8,3,7]`

| Step | Left | Right | Height[L] | Height[R] | Width | Area | Max Area |
|------|------|-------|-----------|-----------|-------|------|----------|
| 1    | 0    | 8     | 1         | 7         | 8     | 8    | 8        |
| 2    | 1    | 8     | 8         | 7         | 7     | 49   | 49       |
| 3    | 1    | 7     | 8         | 3         | 6     | 18   | 49       |
| 4    | 1    | 6     | 8         | 7         | 5     | 35   | 49       |
| 5    | 2    | 6     | 6         | 7         | 4     | 24   | 49       |
| 6    | 3    | 6     | 2         | 7         | 3     | 6    | 49       |
| 7    | 4    | 6     | 5         | 7         | 2     | 10   | 49       |
| 8    | 5    | 6     | 4         | 7         | 1     | 4    | 49       |

**Result: 49**

## Alternative Solutions

### Brute Force Approach
```python
def maxAreaBruteForce(self, height: List[int]) -> int:
    max_area = 0
    n = len(height)
    
    for i in range(n):
        for j in range(i + 1, n):
            area = min(height[i], height[j]) * (j - i)
            max_area = max(max_area, area)
    
    return max_area
```
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)

## Optimization Tips

1. **Early termination**: If current width × max height < max_area, we can break
2. **Preprocessing**: Some optimizations can be made by preprocessing the array
3. **Monotonic stack**: Can be used in certain scenarios

## Test Cases

```python
def test_maxArea():
    solution = Solution()
    
    # Test case 1
    assert solution.maxArea([1,8,6,2,5,4,8,3,7]) == 49
    
    # Test case 2
    assert solution.maxArea([1,1]) == 1
    
    # Test case 3
    assert solution.maxArea([4,3,2,1,4]) == 16
    
    # Test case 4
    assert solution.maxArea([1,2,1]) == 2
```

## Key Insights

### Why Two Pointers Work

The two-pointer approach works because:

1. **Greedy Choice**: At each step, we make the locally optimal choice
2. **Proof by Contradiction**: Moving the taller pointer cannot yield a better solution
3. **Optimal Substructure**: The problem has optimal substructure property

### Mathematical Proof

For any configuration with pointers at positions `i` and `j` where `height[i] <= height[j]`:
- Current area = `height[i] * (j - i)`
- If we move `j` to `j-1`, new area = `height[i] * (j-1-i)` which is smaller
- Therefore, we should move `i` to potentially find a taller line

## Edge Cases

1. **Minimum input**: Array with 2 elements
2. **All same heights**: All elements have the same value
3. **Ascending/Descending**: Monotonic arrays
4. **Single peak**: One tall element among short ones

## Conclusion

The two-pointer approach is the optimal solution for this problem. It provides linear time complexity O(n) and constant space complexity O(1). The algorithm's core principle is to eliminate suboptimal solutions at each step, ensuring we find the maximum area efficiently.