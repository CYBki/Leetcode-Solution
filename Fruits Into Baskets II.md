# Fruit Basket Algorithm - Counting Unplaced Fruits

## Problem Description

This algorithm solves the fruit basket placement problem. Each fruit has a weight and each basket has a capacity. A fruit can only be placed in a basket if the basket's capacity is greater than or equal to the fruit's weight, and each basket can only be used once.

## Algorithm Approach

### Intuition

Since the input size is small, we can simulate the process directly. We iterate over each fruit from left to right and try to find a basket that can hold it. A fruit can only be placed in a basket if the basket's capacity is greater than or equal to the fruit's requirement.

**Core Rules:**
- A fruit can only be placed in a basket if `basket_capacity >= fruit_weight`
- Once a basket is used, it cannot be used again (capacity is set to 0)
- For each fruit that cannot be placed, increment the counter

### Algorithm Steps

1. Initialize `count = 0` (counter for unplaced fruits)
2. For each fruit:
   - Set `unset = 1` (fruit not yet placed)
   - Check all baskets:
     - If `fruit_weight <= basket_capacity`:
       - Use the basket (set capacity to 0)
       - Set `unset = 0`
       - Break from loop
   - Add `unset` to `count` (increment if fruit couldn't be placed)

## Code Implementation

```python
class Solution:
    def numOfUnplacedFruits(self, fruits: List[int], baskets: List[int]) -> int:
        count = 0
        n = len(baskets)
        
        for fruit in fruits:
            unset = 1  # Fruit not yet placed
            
            for i in range(n):
                if fruit <= baskets[i]:  # Is basket capacity sufficient?
                    baskets[i] = 0       # Use the basket
                    unset = 0            # Fruit is placed
                    break
            
            count += unset  # Increment counter if not placed
        
        return count
```

## Complexity Analysis

### Time Complexity: O(n × m)
- `n`: number of fruits (length of fruits array)
- `m`: number of baskets (length of baskets array)
- In the worst case, all baskets are checked for each fruit

### Space Complexity: O(1)
- Only a constant number of additional variables are used (`count`, `unset`, `i`)
- No extra space is used despite modifying input arrays

## Example Walkthrough

```python
fruits = [3, 1, 4, 2]
baskets = [2, 3, 1, 5]

# Step by step:
# Fruit 3: baskets[1]=3 >= 3 ✓ → baskets = [2, 0, 1, 5], count = 0
# Fruit 1: baskets[2]=1 >= 1 ✓ → baskets = [2, 0, 0, 5], count = 0  
# Fruit 4: baskets[3]=5 >= 4 ✓ → baskets = [2, 0, 0, 0], count = 0
# Fruit 2: baskets[0]=2 >= 2 ✓ → baskets = [0, 0, 0, 0], count = 0

# Result: 0 (all fruits were placed)
```

## Algorithm Characteristics

### Advantages
- **Simple and intuitive**: Greedy approach with easy implementation
- **Low space complexity**: Minimal additional memory usage
- **Effective for small inputs**: Direct simulation approach works well

### Disadvantages
- **May not be optimal**: Selecting first available basket doesn't guarantee best solution
- **High time complexity**: O(n²) performance for large inputs
- **Destructive operation**: Modifies the baskets array

## Optimization Suggestions

1. **Sorting**: Sort fruits in descending order, baskets in ascending order
2. **Binary Search**: Use binary search to find suitable baskets efficiently
3. **Heap Usage**: Use priority queue for more efficient basket management

## Test Scenarios

```python
# Test Case 1: All fruits can be placed
fruits = [1, 2, 3]
baskets = [3, 2, 1]
# Expected: 0

# Test Case 2: Some fruits cannot be placed
fruits = [5, 4, 3]
baskets = [2, 1]
# Expected: 2

# Test Case 3: No fruits can be placed
fruits = [10, 20]
baskets = [1, 2]
# Expected: 2
```

## Algorithm Flow Diagram

```
For each fruit:
    ├── Check all baskets sequentially
    ├── Find first basket with sufficient capacity
    ├── If found:
    │   ├── Mark basket as used (capacity = 0)
    │   └── Move to next fruit
    └── If not found:
        └── Increment unplaced counter
```

## Edge Cases

1. **Empty arrays**: Handle empty fruits or baskets arrays
2. **All fruits too large**: When no basket can hold any fruit
3. **More fruits than baskets**: Guaranteed some fruits will be unplaced
4. **Zero capacity baskets**: Baskets with 0 capacity cannot hold any fruit

This algorithm uses a simple greedy approach to solve the problem, but may not always produce the optimal solution for all cases. For larger datasets, consider implementing the suggested optimizations.